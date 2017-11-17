---
title: "適用於 Java 之 Azure 管理程式庫的使用概念和模式"
description: 
keywords: "Azure, Java, SDK, API, Maven, Gradle, 驗證, active directory, 服務主體"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: f452468b-7aae-4944-abad-0b1aaf19170d
ms.openlocfilehash: 052c4de1e8f9ff0ece5f36d1c3514bad8c04cfec
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="azure-management-library-concepts"></a><span data-ttu-id="1846c-103">Azure 管理程式庫概念</span><span class="sxs-lookup"><span data-stu-id="1846c-103">Azure management library concepts</span></span>

## <a name="build-resources-through-a-fluent-interface"></a><span data-ttu-id="1846c-104">透過 Fluent 介面建置資源</span><span class="sxs-lookup"><span data-stu-id="1846c-104">Build resources through a fluent interface</span></span>

<span data-ttu-id="1846c-105">Fluent 介面是一種模式，這種模式會使用能夠正確設定物件屬性的方法鏈結來建立物件。</span><span class="sxs-lookup"><span data-stu-id="1846c-105">A fluent interface is a pattern that creates objects using a method chain that correctly configures the object's attributes.</span></span> <span data-ttu-id="1846c-106">例如，若要建立新的 Azure 儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="1846c-106">For example, to create a new Azure Storage account</span></span>

```java
StorageAccount storage = azure.storageAccounts().define(storageAccountName)
    .withRegion(region)
    .withNewResourceGroup(resourceGroup)
    .create();
```

<span data-ttu-id="1846c-107">當您在方法鏈結中逐步進行時，您的 IDE 會建議下一個要在 Fluent 交談中呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="1846c-107">As you go through the method chain, your IDE suggests the next method to call in the fluent conversation.</span></span>   

![透過 Fluent 鏈結完成工作之 IntelliJ 命令的 GIF 動圖](media/intelliJFluent.gif)

<span data-ttu-id="1846c-109">只要 IDE 所建議的方法對於您要定義的 Azure 資源有用，就請將這些方法鏈結起來。</span><span class="sxs-lookup"><span data-stu-id="1846c-109">Chain the methods suggested by the IDE as long as they make sense for the Azure resource being defined.</span></span> <span data-ttu-id="1846c-110">如果您在鏈結中遺漏了必要方法，IDE 會將它反白顯示並指出錯誤。</span><span class="sxs-lookup"><span data-stu-id="1846c-110">If you are missing a required method in the chain your IDE will highlight it with an error.</span></span>

## <a name="resource-collections"></a><span data-ttu-id="1846c-111">資源集合</span><span class="sxs-lookup"><span data-stu-id="1846c-111">Resource collections</span></span>

<span data-ttu-id="1846c-112">管理程式庫只有一個進入點，透過最上層的 `com.microsoft.azure.management.Azure` 物件進入即可建立和更新資源。</span><span class="sxs-lookup"><span data-stu-id="1846c-112">The management library has a single point of entry through the top-level `com.microsoft.azure.management.Azure` object to create and update resources.</span></span> <span data-ttu-id="1846c-113">請使用 `Azure` 物件中定義的資源集合方法，來選取要處理哪種類型的資源。</span><span class="sxs-lookup"><span data-stu-id="1846c-113">Select which type of resources to work with using the resource collection methods defined in the `Azure` object.</span></span> <span data-ttu-id="1846c-114">例如，SQL Database：</span><span class="sxs-lookup"><span data-stu-id="1846c-114">For example, SQL Database:</span></span>

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName)
    .withAdministratorLogin(administratorLogin)
    .withAdministratorPassword(administratorPassword)
    .create();
```

## <a name="lists-and-iterations"></a><span data-ttu-id="1846c-115">清單和反覆執行</span><span class="sxs-lookup"><span data-stu-id="1846c-115">Lists and iterations</span></span>

<span data-ttu-id="1846c-116">每個資源集合都會有 `list()` 方法以便傳回該資源在您目前的訂用帳戶中的每個執行個體。</span><span class="sxs-lookup"><span data-stu-id="1846c-116">Each resource collection has a `list()` method to return every instance of that resource in your current subscription.</span></span> <span data-ttu-id="1846c-117">例如，`azure.sqlServers().list()` 會傳回訂用帳戶中的所有 SQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="1846c-117">For example, `azure.sqlServers().list()` returns all SQL databases in the subscription.</span></span>

<span data-ttu-id="1846c-118">使用 `listByResourceGroup(String groupname)` 方法可將傳回的清單侷限在特定的 [Azure 資源群組](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)。</span><span class="sxs-lookup"><span data-stu-id="1846c-118">Use the `listByResourceGroup(String groupname)` method to scope the returned List to a specific [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).</span></span>  

<span data-ttu-id="1846c-119">請依照您對一般的 `List<T>` 所進行的操作方式，來對傳回的 `PagedList<T>` 集合進行搜尋並反覆執行：</span><span class="sxs-lookup"><span data-stu-id="1846c-119">Search and iterate over the returned `PagedList<T>` collection just as you would a normal `List<T>`:</span></span>

```java
PagedList<VirtualMachine> vms = azure.virtualMachines().list();
for (VirtualMachine vm : vms) {
    System.out.println("Found virtual machine with ID " + vm.id());
}
```   

## <a name="collections-returned-from-queries"></a><span data-ttu-id="1846c-120">查詢所傳回的集合</span><span class="sxs-lookup"><span data-stu-id="1846c-120">Collections returned from queries</span></span>

<span data-ttu-id="1846c-121">管理程式庫會根據結果的結構，從查詢中傳回特定的集合類型。</span><span class="sxs-lookup"><span data-stu-id="1846c-121">The management libraries return specific collection types from queries based on the structure of the results.</span></span>

- <span data-ttu-id="1846c-122">`List<T>`：容易搜尋並反覆執行的未排序資料。</span><span class="sxs-lookup"><span data-stu-id="1846c-122">`List<T>`: Unordered data that is easy to search and iterate over.</span></span>
- <span data-ttu-id="1846c-123">`Map<T>`：對應是指一對索引鍵/值，索引鍵是唯一的，值則不一定唯一。</span><span class="sxs-lookup"><span data-stu-id="1846c-123">`Map<T>`: Maps are key/value pairs with unique keys, but not necessarily unique values.</span></span> <span data-ttu-id="1846c-124">舉例來說，App Service Web 應用程式的應用程式設定就是對應。</span><span class="sxs-lookup"><span data-stu-id="1846c-124">An example of a Map would be app settings for an App Service Web App.</span></span>
- <span data-ttu-id="1846c-125">`Set<T>`：集合具有唯一的索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="1846c-125">`Set<T>`: Sets have unique keys and values.</span></span> <span data-ttu-id="1846c-126">集合的範例則是連結至虛擬機器的網路，這個集合會有唯一的識別碼 (索引鍵) 和唯一的網路組態 (值)。</span><span class="sxs-lookup"><span data-stu-id="1846c-126">An example of a Set would be networks attached to a virtual machine, which would have both an unique identifier (the key) and a unique network configuration (the value).</span></span>

## <a name="actionable-verbs"></a><span data-ttu-id="1846c-127">可採取動作的動詞命令</span><span class="sxs-lookup"><span data-stu-id="1846c-127">Actionable verbs</span></span>

<span data-ttu-id="1846c-128">其名稱中有動詞命令的方法會在 Azure 中立即採取動作。</span><span class="sxs-lookup"><span data-stu-id="1846c-128">Methods with verbs in their names take immediate action in Azure.</span></span> <span data-ttu-id="1846c-129">這些方法會同步運作，而且在完成之前會封鎖目前執行緒的執行。</span><span class="sxs-lookup"><span data-stu-id="1846c-129">These methods work synchronously and block execution in the current thread until they complete.</span></span> 

| <span data-ttu-id="1846c-130">指令動詞</span><span class="sxs-lookup"><span data-stu-id="1846c-130">Verb</span></span>   |  <span data-ttu-id="1846c-131">範例用法</span><span class="sxs-lookup"><span data-stu-id="1846c-131">Sample Usage</span></span> |
|--------|---------------|
| <span data-ttu-id="1846c-132">create</span><span class="sxs-lookup"><span data-stu-id="1846c-132">create</span></span> | `azure.virtualMachines().create(listOfVMCreatables)` |
| <span data-ttu-id="1846c-133">apply</span><span class="sxs-lookup"><span data-stu-id="1846c-133">apply</span></span>  | `virtualMachineScaleSet.update().withCapacity(6).apply()` |
| <span data-ttu-id="1846c-134">delete</span><span class="sxs-lookup"><span data-stu-id="1846c-134">delete</span></span> | `azure.disks().deleteById(id)` | 
| <span data-ttu-id="1846c-135">list</span><span class="sxs-lookup"><span data-stu-id="1846c-135">list</span></span>   | `azure.sqlServers().list()` | 
| <span data-ttu-id="1846c-136">get</span><span class="sxs-lookup"><span data-stu-id="1846c-136">get</span></span>    | `VirtualMachine vm  = azure.virtualMachines().getByResourceGroup(group, vmName)` |

>[!NOTE]
> <span data-ttu-id="1846c-137">`define()` 和 `update()` 是動詞命令，但除非後面接著 `create()` 或 `apply()`，否則不會封鎖。</span><span class="sxs-lookup"><span data-stu-id="1846c-137">`define()` and `update()` are verbs but do not block unless followed by a `create()` or `apply()`.</span></span>
 
<span data-ttu-id="1846c-138">這些方法中有一部分可透過使用[回應式擴充功能](https://github.com/ReactiveX/RxJava)而具有非同步版本，非同步版本的方法會有 `Async` 後置詞。</span><span class="sxs-lookup"><span data-stu-id="1846c-138">Asynchronous versions of some of these  methods exist with the `Async` suffix using [Reactive extensions](https://github.com/ReactiveX/RxJava).</span></span> 

<span data-ttu-id="1846c-139">某些物件具有其他方法，從而會在 Azure 中變更資源的狀態。</span><span class="sxs-lookup"><span data-stu-id="1846c-139">Some objects have other methods with that change the state of the resource in Azure.</span></span> <span data-ttu-id="1846c-140">例如，`VirtualMachine` 上的 `restart()`。</span><span class="sxs-lookup"><span data-stu-id="1846c-140">For example, `restart()` on a `VirtualMachine`.</span></span>

```java
VirtualMachine vmToRestart = azure.getVirtualMachines().getById(id);
vmToRestart.restart();
```
<span data-ttu-id="1846c-141">這些方法不一定會有非同步版本，而且在完成之前會封鎖其執行緒的執行。</span><span class="sxs-lookup"><span data-stu-id="1846c-141">These methods do not always have asynchronous versions and will block execution on their thread until they complete.</span></span>

<a name="Creatables"></a>

## <a name="lazy-resource-creation"></a><span data-ttu-id="1846c-142">資源建立緩慢</span><span class="sxs-lookup"><span data-stu-id="1846c-142">Lazy resource creation</span></span>

<span data-ttu-id="1846c-143">在建立 Azure 資源時，如果新的資源需要仰賴另一項尚不存在的資源時，就會引發一項挑戰。</span><span class="sxs-lookup"><span data-stu-id="1846c-143">A challenge when creating Azure resources arises when a new resource depends on another resource that doesn't yet exist.</span></span> <span data-ttu-id="1846c-144">針對這個情形，我們以「在建立新的虛擬機器時保留公用 IP 位址並設定磁碟」為例。</span><span class="sxs-lookup"><span data-stu-id="1846c-144">An example of this scenario is reserving a public IP address and setting up a disk when creating a new virtual machine.</span></span> <span data-ttu-id="1846c-145">您不想要確認是否保留位址或建立磁碟，您只想要確保虛擬機器於建立時具有這些資源。</span><span class="sxs-lookup"><span data-stu-id="1846c-145">You don't want to verify reserving the address or the creating the disk, you just want to ensure the virtual machine has those resources when it is created.</span></span>

<span data-ttu-id="1846c-146">`Creatable<T>` 物件可讓您定義要在程式碼中使用的 Azure 資源，而不必等待訂用帳戶建立這些資源。</span><span class="sxs-lookup"><span data-stu-id="1846c-146">`Creatable<T>` objects let you define Azure resources for use in your code without waiting around for them to be created in your subscription.</span></span> <span data-ttu-id="1846c-147">管理程式庫會將 `Creatable<T>` 物件的建立工作推遲，直到要用到時再加以建立。</span><span class="sxs-lookup"><span data-stu-id="1846c-147">The management libraries defer creating  `Creatable<T>` objects until they are needed.</span></span>

<span data-ttu-id="1846c-148">透過 `define()` 動詞命令為 Azure 資源產生 `Creatable<T>` 物件：</span><span class="sxs-lookup"><span data-stu-id="1846c-148">Generate `Creatable<T>` objects for Azure resources through the `define()` verb:</span></span>

```java
Creatable<PublicIPAddress> publicIPAddressCreatable = azure.publicIPAddresses().define(publicIPAddressName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName);
```

<span data-ttu-id="1846c-149">在此程式碼執行當下，此範例中由 `Creatable<PublicIPAddress>` 所定義的 Azure 資源尚未存在於訂用帳戶中。</span><span class="sxs-lookup"><span data-stu-id="1846c-149">The Azure resource defined by the `Creatable<PublicIPAddress>` in this example does not yet exist in your subscription when this code executes.</span></span>  <span data-ttu-id="1846c-150">請使用 `publicIPAddressCreatable` 物件來建立其他具有此 IP 位址的 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="1846c-150">Use the `publicIPAddressCreatable` object to create other Azure resources with this IP address.</span></span> 

```java
Creatable<VirtualMachine> vmCreatable = azure.virtualMachines().define("creatableVM")
    .withNewPrimaryPublicIPAddress(publicIPAddressCreatable)
```

<span data-ttu-id="1846c-151">當您使用 `create()` 在 Azure 中建置任何以此物件定義的資源時，訂用帳戶中就會產生 `Creatable<T>` 資源。</span><span class="sxs-lookup"><span data-stu-id="1846c-151">The `Creatable<T>` resources are generated in your subscription when any resource that is defined using the object is built in Azure using `create()`.</span></span> <span data-ttu-id="1846c-152">繼續進行 IP 位址及虛擬機器的範例：</span><span class="sxs-lookup"><span data-stu-id="1846c-152">Continuing the IP address and virtual machine example:</span></span>

```java
CreatedResources<VirtualMachine> virtualMachine = azure.virtualMachines().create(vmCreatable);
```

<span data-ttu-id="1846c-153">將 `Creatable<T>` 傳遞至 `create()` 呼叫會傳回 `CreatedResources` 物件，而非傳回單一資源物件。</span><span class="sxs-lookup"><span data-stu-id="1846c-153">Passing the `Creatable<T>` to `create()` calls returns a `CreatedResources` object instead of a single resource object.</span></span>  <span data-ttu-id="1846c-154">`CreatedResources<T>` 物件可讓您存取 `create()` 呼叫所建立的所有資源，而不只是存取呼叫中具類型的資源。</span><span class="sxs-lookup"><span data-stu-id="1846c-154">The `CreatedResources<T>` object lets you access all resources created by the `create()` call, not just the typed resource in the call.</span></span> <span data-ttu-id="1846c-155">若要存取在 Azure 中為上述範例所建立之虛擬機器而建立的公用 IP 位址：</span><span class="sxs-lookup"><span data-stu-id="1846c-155">To access the public IP address created in Azure for the virtual machine created in the above example:</span></span>

```java
PublicIPAddress pip = (PublicIPAddress) virtualMachine.createdRelatedResource(publicIPAddressCreatable.key());
```    

## <a name="exception-handling"></a><span data-ttu-id="1846c-156">例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="1846c-156">Exception handling</span></span>

<span data-ttu-id="1846c-157">管理程式庫的例外狀況類別可擴充 `com.microsoft.rest.RestException`。</span><span class="sxs-lookup"><span data-stu-id="1846c-157">The management libraries' Exception classes extend `com.microsoft.rest.RestException`.</span></span> <span data-ttu-id="1846c-158">在相關的 `try` 陳述式之後加上 `catch (RestException exception)` 區塊，即可攔截管理程式庫所產生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1846c-158">Catch exceptions generated by the management libraries with a `catch (RestException exception)` block after the relevant `try` statement.</span></span>

## <a name="logs-and-trace"></a><span data-ttu-id="1846c-159">記錄和追蹤</span><span class="sxs-lookup"><span data-stu-id="1846c-159">Logs and trace</span></span>

<span data-ttu-id="1846c-160">在使用 `withLogLevel()` 建置進入點 `Azure` 物件時，可從管理程式庫設定記錄數量。</span><span class="sxs-lookup"><span data-stu-id="1846c-160">Configure the amount of logging from the management library when you build the entry point `Azure` object using `withLogLevel()`.</span></span> <span data-ttu-id="1846c-161">可用的追蹤層級如下：</span><span class="sxs-lookup"><span data-stu-id="1846c-161">The following trace levels exist:</span></span>

| <span data-ttu-id="1846c-162">追蹤層級</span><span class="sxs-lookup"><span data-stu-id="1846c-162">Trace level</span></span> | <span data-ttu-id="1846c-163">啟用記錄</span><span class="sxs-lookup"><span data-stu-id="1846c-163">Logging enabled</span></span> 
| ------------ | ---------------
| <span data-ttu-id="1846c-164">com.microsoft.rest.LogLevel.NONE</span><span class="sxs-lookup"><span data-stu-id="1846c-164">com.microsoft.rest.LogLevel.NONE</span></span> | <span data-ttu-id="1846c-165">不輸出</span><span class="sxs-lookup"><span data-stu-id="1846c-165">No output</span></span>
| <span data-ttu-id="1846c-166">com.microsoft.rest.LogLevel.BASIC</span><span class="sxs-lookup"><span data-stu-id="1846c-166">com.microsoft.rest.LogLevel.BASIC</span></span> | <span data-ttu-id="1846c-167">記錄基礎 REST 呼叫的 URL、回應碼和時間</span><span class="sxs-lookup"><span data-stu-id="1846c-167">Logs the URLs to underlying REST calls, response codes and times</span></span>
| <span data-ttu-id="1846c-168">com.microsoft.rest.LogLevel.BODY</span><span class="sxs-lookup"><span data-stu-id="1846c-168">com.microsoft.rest.LogLevel.BODY</span></span> | <span data-ttu-id="1846c-169">BASIC 中的所有項目，以及 REST 呼叫的要求和回應內文</span><span class="sxs-lookup"><span data-stu-id="1846c-169">Everything in BASIC plus request and response bodies for the REST calls</span></span>
| <span data-ttu-id="1846c-170">com.microsoft.rest.LogLevel.HEADERS</span><span class="sxs-lookup"><span data-stu-id="1846c-170">com.microsoft.rest.LogLevel.HEADERS</span></span> | <span data-ttu-id="1846c-171">BASIC 中的所有項目，以及 REST 呼叫的要求和回應標頭</span><span class="sxs-lookup"><span data-stu-id="1846c-171">Everything in BASIC plus the request and response headers REST calls</span></span>
| <span data-ttu-id="1846c-172">com.microsoft.rest.LogLevel.BODY_AND_HEADERS</span><span class="sxs-lookup"><span data-stu-id="1846c-172">com.microsoft.rest.LogLevel.BODY_AND_HEADERS</span></span> | <span data-ttu-id="1846c-173">BODY 中的所有項目，以及 HEADERS 記錄層級</span><span class="sxs-lookup"><span data-stu-id="1846c-173">Everything in both BODY and HEADERS log level</span></span>

<span data-ttu-id="1846c-174">若您需要將輸出記錄到 [Log4J 2](https://logging.apache.org/log4j/2.x/) 之類的記錄架構，請繫結 [SLF4J 記錄實作](https://www.slf4j.org/manual.html)。</span><span class="sxs-lookup"><span data-stu-id="1846c-174">Bind a [SLF4J logging implementation](https://www.slf4j.org/manual.html) if you need to log output to a logging framework like [Log4J 2](https://logging.apache.org/log4j/2.x/).</span></span>