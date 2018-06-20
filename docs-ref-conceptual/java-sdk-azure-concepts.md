---
title: 適用於 Java 開發人員的 Azure 管理程式庫指南
description: 使用 Java 管理程式庫，以便讓 Java 可以管理 Azure 雲端資源的模式和概念。
keywords: Azure, Java, SDK, API, Maven, Gradle, 驗證, active directory, 服務主體
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
ms.openlocfilehash: 8b52981ddfaadb7227cea4c7df014011196339cb
ms.sourcegitcommit: 1f6a80e067a8bdbbb4b2da2e2145fda73d5fe65a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2017
ms.locfileid: "26184642"
---
# <a name="patterns-and-best-practices-for-development-with-the-azure-libraries-for-java"></a>使用適用於 Java 的 Azure 程式庫來進行開發的模式和最佳做法 

本文會列出一系列在專案中使用適用於 Java 的 Azure 程式庫的模式和最佳做法。 請使用這些模式和指導方針進行開發，以減少要維護的程式碼數量，並輕鬆地在未來的管理程式庫更新中新增或設定其他資源。

## <a name="build-resources-through-a-fluent-interface"></a>透過 Fluent 介面建置資源

Fluent 介面是一種模式，這種模式會使用能夠正確設定物件屬性的方法鏈結來建立物件。 例如，若要建立新的 Azure 儲存體帳戶

```java
StorageAccount storage = azure.storageAccounts().define(storageAccountName)
    .withRegion(region)
    .withNewResourceGroup(resourceGroup)
    .create();
```

當您在方法鏈結中逐步進行時，您的 IDE 會建議下一個要在 Fluent 交談中呼叫的方法。   

![透過 Fluent 鏈結完成工作之 IntelliJ 命令的 GIF 動圖](media/intelliJFluent.gif)

只要 IDE 所建議的方法對於您要定義的 Azure 資源有用，就請將這些方法鏈結起來。 如果您在鏈結中遺漏了必要方法，IDE 會將它反白顯示並指出錯誤。

## <a name="resource-collections"></a>資源集合

管理程式庫只有一個進入點，透過最上層的 `com.microsoft.azure.management.Azure` 物件進入即可建立和更新資源。 請使用 `Azure` 物件中定義的資源集合方法，來選取要處理哪種類型的資源。 例如，SQL Database：

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName)
    .withAdministratorLogin(administratorLogin)
    .withAdministratorPassword(administratorPassword)
    .create();
```

## <a name="lists-and-iterations"></a>清單和反覆執行

每個資源集合都會有 `list()` 方法以便傳回該資源在您目前的訂用帳戶中的每個執行個體。 例如，`azure.sqlServers().list()` 會傳回訂用帳戶中的所有 SQL 資料庫。

使用 `listByResourceGroup(String groupname)` 方法可將傳回的清單侷限在特定的 [Azure 資源群組](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)。  

請依照您對一般的 `List<T>` 所進行的操作方式，來對傳回的 `PagedList<T>` 集合進行搜尋並反覆執行：

```java
PagedList<VirtualMachine> vms = azure.virtualMachines().list();
for (VirtualMachine vm : vms) {
    System.out.println("Found virtual machine with ID " + vm.id());
}
```   

## <a name="collections-returned-from-queries"></a>查詢所傳回的集合

管理程式庫會根據結果的結構，從查詢中傳回特定的集合類型。

- `List<T>`：容易搜尋並反覆執行的未排序資料。
- `Map<T>`：對應是指一對索引鍵/值，索引鍵是唯一的，值則不一定唯一。 舉例來說，App Service Web 應用程式的應用程式設定就是對應。
- `Set<T>`：集合具有唯一的索引鍵和值。 集合的範例則是連結至虛擬機器的網路，這個集合會有唯一的識別碼 (索引鍵) 和唯一的網路組態 (值)。

## <a name="actionable-verbs"></a>可採取動作的動詞命令

其名稱中有動詞命令的方法會在 Azure 中立即採取動作。 這些方法會同步運作，而且在完成之前會封鎖目前執行緒的執行。 

| 指令動詞   |  範例用法 |
|--------|---------------|
| create | `azure.virtualMachines().create(listOfVMCreatables)` |
| apply  | `virtualMachineScaleSet.update().withCapacity(6).apply()` |
| delete | `azure.disks().deleteById(id)` | 
| list   | `azure.sqlServers().list()` | 
| get    | `VirtualMachine vm  = azure.virtualMachines().getByResourceGroup(group, vmName)` |

>[!NOTE]
> `define()` 和 `update()` 是動詞命令，但除非後面接著 `create()` 或 `apply()`，否則不會封鎖。
 
這些方法中有一部分可透過使用[回應式擴充功能](https://github.com/ReactiveX/RxJava)而具有非同步版本，非同步版本的方法會有 `Async` 後置詞。 

某些物件具有其他方法，從而會在 Azure 中變更資源的狀態。 例如，`VirtualMachine` 上的 `restart()`。

```java
VirtualMachine vmToRestart = azure.getVirtualMachines().getById(id);
vmToRestart.restart();
```
這些方法不一定會有非同步版本，而且在完成之前會封鎖其執行緒的執行。

<a name="Creatables"></a>

## <a name="lazy-resource-creation"></a>資源建立緩慢

在建立 Azure 資源時，如果新的資源需要仰賴另一項尚不存在的資源時，就會引發一項挑戰。 針對這個情形，我們以「在建立新的虛擬機器時保留公用 IP 位址並設定磁碟」為例。 您不想要確認是否保留位址或建立磁碟，您只想要確保虛擬機器於建立時具有這些資源。

`Creatable<T>` 物件可讓您定義要在程式碼中使用的 Azure 資源，而不必等待訂用帳戶建立這些資源。 管理程式庫會將 `Creatable<T>` 物件的建立工作推遲，直到要用到時再加以建立。

透過 `define()` 動詞命令為 Azure 資源產生 `Creatable<T>` 物件：

```java
Creatable<PublicIPAddress> publicIPAddressCreatable = azure.publicIPAddresses().define(publicIPAddressName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName);
```

在此程式碼執行當下，此範例中由 `Creatable<PublicIPAddress>` 所定義的 Azure 資源尚未存在於訂用帳戶中。  請使用 `publicIPAddressCreatable` 物件來建立其他具有此 IP 位址的 Azure 資源。 

```java
Creatable<VirtualMachine> vmCreatable = azure.virtualMachines().define("creatableVM")
    .withNewPrimaryPublicIPAddress(publicIPAddressCreatable)
```

當您使用 `create()` 在 Azure 中建置任何以此物件定義的資源時，訂用帳戶中就會產生 `Creatable<T>` 資源。 繼續進行 IP 位址及虛擬機器的範例：

```java
CreatedResources<VirtualMachine> virtualMachine = azure.virtualMachines().create(vmCreatable);
```

將 `Creatable<T>` 傳遞至 `create()` 呼叫會傳回 `CreatedResources` 物件，而非傳回單一資源物件。  `CreatedResources<T>` 物件可讓您存取 `create()` 呼叫所建立的所有資源，而不只是存取呼叫中具類型的資源。 若要存取在 Azure 中為上述範例所建立之虛擬機器而建立的公用 IP 位址：

```java
PublicIPAddress pip = (PublicIPAddress) virtualMachine.createdRelatedResource(publicIPAddressCreatable.key());
```    

## <a name="exception-handling"></a>例外狀況處理

管理程式庫的例外狀況類別可擴充 `com.microsoft.rest.RestException`。 在相關的 `try` 陳述式之後加上 `catch (RestException exception)` 區塊，即可攔截管理程式庫所產生的例外狀況。

## <a name="logs-and-trace"></a>記錄和追蹤

在使用 `withLogLevel()` 建置進入點 `Azure` 物件時，可從管理程式庫設定記錄數量。 可用的追蹤層級如下：

| 追蹤層級 | 啟用記錄 
| ------------ | ---------------
| com.microsoft.rest.LogLevel.NONE | 不輸出
| com.microsoft.rest.LogLevel.BASIC | 記錄基礎 REST 呼叫的 URL、回應碼和時間
| com.microsoft.rest.LogLevel.BODY | BASIC 中的所有項目，以及 REST 呼叫的要求和回應內文
| com.microsoft.rest.LogLevel.HEADERS | BASIC 中的所有項目，以及 REST 呼叫的要求和回應標頭
| com.microsoft.rest.LogLevel.BODY_AND_HEADERS | BODY 中的所有項目，以及 HEADERS 記錄層級

若您需要將輸出記錄到 [Log4J 2](https://logging.apache.org/log4j/2.x/) 之類的記錄架構，請繫結 [SLF4J 記錄實作](https://www.slf4j.org/manual.html)。