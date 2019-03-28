---
title: Azure HDInsight Java SDK
description: Azure HDInsight Java SDK 的參考。 HDInsight Java SDK 提供可讓您管理 HDInsight 叢集的類別和方法。
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: reference
ms.devlang: java
ms.date: 11/21/2018
ms.openlocfilehash: 0ae8d78a0618c4dbcc5e734fce311f7c2e5684bd
ms.sourcegitcommit: a108a82414bd35be896e3c4e7047f5eb7b1518cb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489646"
---
# <a name="hdinsight-java-management-sdk-preview"></a><span data-ttu-id="395cf-104">HDInsight Java 管理 SDK (預覽)</span><span class="sxs-lookup"><span data-stu-id="395cf-104">HDInsight Java Management SDK (Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="395cf-105">概觀</span><span class="sxs-lookup"><span data-stu-id="395cf-105">Overview</span></span>

<span data-ttu-id="395cf-106">HDInsight Java SDK 提供可讓您管理 HDInsight 叢集的類別和方法。</span><span class="sxs-lookup"><span data-stu-id="395cf-106">The HDInsight Java SDK provides classes and methods that allow you to manage your HDInsight clusters.</span></span> <span data-ttu-id="395cf-107">它包含用來建立、刪除、更新、列出、調整大小、執行指令碼動作、監視、取得 HDInsight 叢集屬性的作業，和其他多種作業。</span><span class="sxs-lookup"><span data-stu-id="395cf-107">It includes operations to create, delete, update, list, resize, execute script actions, monitor, get properties of HDInsight clusters, and more.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="395cf-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="395cf-108">Prerequisites</span></span>

* <span data-ttu-id="395cf-109">一個 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="395cf-109">An Azure account.</span></span> <span data-ttu-id="395cf-110">如果您沒有帳戶，請[取得免費試用帳戶](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="395cf-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="395cf-111">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="395cf-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="395cf-112">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="395cf-112">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* [<span data-ttu-id="395cf-113">Maven</span><span class="sxs-lookup"><span data-stu-id="395cf-113">Maven</span></span>](https://maven.apache.org/download.cgi)

## <a name="sdk-installation"></a><span data-ttu-id="395cf-114">SDK 安裝</span><span class="sxs-lookup"><span data-stu-id="395cf-114">SDK Installation</span></span>

<span data-ttu-id="395cf-115">您可以在[此處](https://mvnrepository.com/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight)透過 Maven 取得 HDInsight Java SDK。</span><span class="sxs-lookup"><span data-stu-id="395cf-115">The HDInsight Java SDK is available through Maven [here](https://mvnrepository.com/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight).</span></span> <span data-ttu-id="395cf-116">將下列相依性新增至 pom.xml 中：</span><span class="sxs-lookup"><span data-stu-id="395cf-116">Add the following dependency to your pom.xml:</span></span>

```
<dependency>
    <groupId>com.microsoft.azure.hdinsight.v2018_06_01_preview</groupId>
    <artifactId>azure-mgmt-hdinsight</artifactId>
    <version>1.0.0-beta-1</version>
</dependency>
```

<span data-ttu-id="395cf-117">您也需要將下列相依性新增至 pom.xml：</span><span class="sxs-lookup"><span data-stu-id="395cf-117">You will also need to add the following dependencies to your pom.xml:</span></span>

* [<span data-ttu-id="395cf-118">Azure 用戶端驗證程式庫：</span><span class="sxs-lookup"><span data-stu-id="395cf-118">Azure Client Authentication Library:</span></span>](https://mvnrepository.com/artifact/com.microsoft.azure/azure-client-authentication/1.6.2)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-client-authentication</artifactId>
    <version>1.6.2</version>
  </dependency>
  ```

* [<span data-ttu-id="395cf-119">適用於 ARM 的 Azure Java 用戶端執行階段：</span><span class="sxs-lookup"><span data-stu-id="395cf-119">Azure Java Client Runtime For ARM:</span></span>](https://mvnrepository.com/artifact/com.microsoft.azure/azure-arm-client-runtime/1.6.2)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-arm-client-runtime</artifactId>
    <version>1.6.2</version>
  </dependency>
  ```

## <a name="authentication"></a><span data-ttu-id="395cf-120">Authentication</span><span class="sxs-lookup"><span data-stu-id="395cf-120">Authentication</span></span>

<span data-ttu-id="395cf-121">SDK 必須先使用您的 Azure 訂用帳戶進行驗證。</span><span class="sxs-lookup"><span data-stu-id="395cf-121">The SDK first needs to be authenticated with your Azure subscription.</span></span>  <span data-ttu-id="395cf-122">請依照下列範例建立服務主體，並使用它來驗證。</span><span class="sxs-lookup"><span data-stu-id="395cf-122">Follow the example below to create a service principal and use it to authenticate.</span></span> <span data-ttu-id="395cf-123">此動作完成後，您會有 `HDInsightManagementClientImpl` 的執行個體，其中包含許多可用來執行管理作業的方法 (分述於下列各節中)。</span><span class="sxs-lookup"><span data-stu-id="395cf-123">After this is done, you will have an instance of an `HDInsightManagementClientImpl`, which contains many methods (outlined in below sections) that can be used to perform management operations.</span></span>

> [!NOTE]
> <span data-ttu-id="395cf-124">除了下列範例以外，還有其他方式可進行驗證，可能更符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="395cf-124">There are other ways to authenticate besides the below example that could potentially be better suited for your needs.</span></span> <span data-ttu-id="395cf-125">此處概述所有方法：[使用適用於 Java 的 Azure 管理程式庫來進行驗證](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)</span><span class="sxs-lookup"><span data-stu-id="395cf-125">All methods are outlined here: [Authenticate with the Azure management libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)</span></span>

### <a name="authentication-example-using-a-service-principal"></a><span data-ttu-id="395cf-126">使用服務主體的驗證範例</span><span class="sxs-lookup"><span data-stu-id="395cf-126">Authentication Example Using a Service Principal</span></span>

<span data-ttu-id="395cf-127">首先，請登入 [Azure Cloud Shell](https://shell.azure.com/bash)。</span><span class="sxs-lookup"><span data-stu-id="395cf-127">First, login to [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="395cf-128">確認您目前使用的訂用帳戶，是您希望建立服務主體的位置。</span><span class="sxs-lookup"><span data-stu-id="395cf-128">Verify you are currently using the subscription in which you want the service principal created.</span></span> 

```azurecli-interactive
az account show
```

<span data-ttu-id="395cf-129">您的訂用帳戶資訊會以 JSON 的形式顯示。</span><span class="sxs-lookup"><span data-stu-id="395cf-129">Your subscription information is displayed as JSON.</span></span>

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

<span data-ttu-id="395cf-130">如果您未登入正確的訂用帳戶，請執行下列命令以選取正確的訂用帳戶：</span><span class="sxs-lookup"><span data-stu-id="395cf-130">If you're not logged into the correct subscription, select the correct one by running:</span></span> 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> <span data-ttu-id="395cf-131">如果您尚未以其他方法註冊 HDInsight 資源提供者 (例如，透過 Azure 入口網站建立 HDInsight 叢集)，您必須立即執行此動作，才能進行驗證。</span><span class="sxs-lookup"><span data-stu-id="395cf-131">If you have not already registered the HDInsight Resource Provider by another method (such as by creating an HDInsight Cluster through the Azure Portal), you need to do this once before you can authenticate.</span></span> <span data-ttu-id="395cf-132">此動作也可以從 [Azure Cloud Shell](https://shell.azure.com/bash) 完成，只要執行下列命令即可：</span><span class="sxs-lookup"><span data-stu-id="395cf-132">This can be done from the [Azure Cloud Shell](https://shell.azure.com/bash) by running the following command:</span></span>
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

<span data-ttu-id="395cf-133">接下來，請選擇服務主體的名稱，並使用下列命令加以建立：</span><span class="sxs-lookup"><span data-stu-id="395cf-133">Next, choose a name for your service principal and create it with the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

<span data-ttu-id="395cf-134">服務主體資訊會顯示為 JSON。</span><span class="sxs-lookup"><span data-stu-id="395cf-134">The service principal information is displayed as JSON.</span></span>

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
<span data-ttu-id="395cf-135">複製下列程式碼片段，並且在 `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET` 和 `SUBSCRIPTION_ID` 中填入在執行建立服務主體的命令後傳回的 JSON 中所包含的字串。</span><span class="sxs-lookup"><span data-stu-id="395cf-135">Copy the below snippet and fill in `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, and `SUBSCRIPTION_ID` with the strings from the JSON that was returned after running the command to create the service principal.</span></span>

```java
import com.microsoft.azure.management.hdinsight.v2018_06_01_preview.*;
import com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.HDInsightManagementClientImpl;

public class Main {
    public static void main (String[] args) {

        // Tenant ID for your Azure Subscription
        String TENANT_ID = "";
        // Your Service Principal App Client ID
        String CLIENT_ID = "";
        // Your Service Principal Client Secret
        String CLIENT_SECRET = "";
        // Azure Subscription ID
        String SUBSCRIPTION_ID = "";

        ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(
                CLIENT_ID,
                TENANT_ID,
                CLIENT_SECRET,
                AzureEnvironment.AZURE);

        HDInsightManagementClientImpl client = new HDInsightManagementClientImpl(credentials)
                .withSubscriptionId(SUBSCRIPTION_ID);
```


## <a name="cluster-management"></a><span data-ttu-id="395cf-136">叢集管理</span><span class="sxs-lookup"><span data-stu-id="395cf-136">Cluster Management</span></span>

> [!NOTE]
> <span data-ttu-id="395cf-137">本節假設您已通過驗證並建構 `HDInsightManagementClientImpl` 執行個體，並且將其儲存在名為 `client` 的變數中。</span><span class="sxs-lookup"><span data-stu-id="395cf-137">This section assumes you have already authenticated and constructed an `HDInsightManagementClientImpl` instance and store it in a variable called `client`.</span></span> <span data-ttu-id="395cf-138">驗證及取得 `HDInsightManagementClientImpl` 的指示可在先前的「驗證」一節中找到。</span><span class="sxs-lookup"><span data-stu-id="395cf-138">Instructions for authenticating and obtaining an `HDInsightManagementClientImpl` can be found in the Authentication section above.</span></span>

### <a name="create-a-cluster"></a><span data-ttu-id="395cf-139">建立叢集</span><span class="sxs-lookup"><span data-stu-id="395cf-139">Create a Cluster</span></span>

<span data-ttu-id="395cf-140">新叢集可藉由呼叫 `client.clusters().create()` 來建立。</span><span class="sxs-lookup"><span data-stu-id="395cf-140">A new cluster can be created by calling `client.clusters().create()`.</span></span>

#### <a name="example"></a><span data-ttu-id="395cf-141">範例</span><span class="sxs-lookup"><span data-stu-id="395cf-141">Example</span></span>

<span data-ttu-id="395cf-142">此範例示範如何使用 2 個前端節點和 1 個背景工作節點來建立 Spark 叢集。</span><span class="sxs-lookup"><span data-stu-id="395cf-142">This example demonstrates how to create a Spark cluster with 2 head nodes and 1 worker node.</span></span>

> [!NOTE]
> <span data-ttu-id="395cf-143">您必須先建立資源群組和儲存體帳戶，說明如下。</span><span class="sxs-lookup"><span data-stu-id="395cf-143">You first need to create a Resource Group and Storage Account, as explained below.</span></span> <span data-ttu-id="395cf-144">如果您已建立這些項目，則可以略過這些步驟。</span><span class="sxs-lookup"><span data-stu-id="395cf-144">If you have already created these, you can skip these steps.</span></span>

##### <a name="creating-a-resource-group"></a><span data-ttu-id="395cf-145">建立資源群組</span><span class="sxs-lookup"><span data-stu-id="395cf-145">Creating a Resource Group</span></span>

<span data-ttu-id="395cf-146">您可以使用 [Azure Cloud Shell](https://shell.azure.com/bash) 建立資源群組，只要執行下列命令即可</span><span class="sxs-lookup"><span data-stu-id="395cf-146">You can create a resource group using the [Azure Cloud Shell](https://shell.azure.com/bash) by running</span></span>
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a><span data-ttu-id="395cf-147">建立儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="395cf-147">Creating a Storage Account</span></span>

<span data-ttu-id="395cf-148">您可以使用 [Azure Cloud Shell](https://shell.azure.com/bash) 建立儲存體帳戶，只要執行下列命令即可：</span><span class="sxs-lookup"><span data-stu-id="395cf-148">You can create a storage account using the [Azure Cloud Shell](https://shell.azure.com/bash) by running:</span></span>
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
<span data-ttu-id="395cf-149">現在請執行下列命令，以取得儲存體帳戶的金鑰 (您需要此金鑰才能建立叢集)：</span><span class="sxs-lookup"><span data-stu-id="395cf-149">Now run the following command to get the key for your storage account (you will need this to create a cluster):</span></span>
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
<span data-ttu-id="395cf-150">下列 Java 程式碼片段會使用 2 個前端節點和 1 個背景工作節點來建立 Spark 叢集。</span><span class="sxs-lookup"><span data-stu-id="395cf-150">The below Java snippet creates a Spark cluster with 2 head nodes and 1 worker node.</span></span> <span data-ttu-id="395cf-151">請依照註解中的說明填入空白變數，並依據您的特定需求變更其他參數。</span><span class="sxs-lookup"><span data-stu-id="395cf-151">Fill in the blank variables as explained in the comments and feel free to change other parameters to suit your specific needs.</span></span>

```java
// The name for the cluster you are creating
String clusterName = "";
// The name of your existing Resource Group
String resourceGroupName = "";
// Choose a username
String username = "";
// Choose a password
String password = "";
// Replace <> with the name of your storage account
String storageAccount = "<>.blob.core.windows.net";
// Storage account key you obtained above
String storageAccountKey = "";
// Choose a region
String location = "";
String container = "default";

HashMap<String, HashMap<String, String>> configurations = new HashMap<String, HashMap<String, String>>();
        HashMap<String, String> gateway = new HashMap<String, String>();
        gateway.put("restAuthCredential.enabled_credential", "True");
        gateway.put("restAuthCredential.username", username);
        gateway.put("restAuthCredential.password", password);
        configurations.put("gateway", gateway);

        ClusterCreateParametersExtended parameters = new ClusterCreateParametersExtended()
            .withLocation(location)
            .withTags(Collections.EMPTY_MAP)
            .withProperties(
                new ClusterCreateProperties()
                    .withClusterVersion("3.6")
                    .withOsType(OSType.LINUX)
                    .withClusterDefinition(new ClusterDefinition()
                            .withKind("spark")
                            .withConfigurations(configurations)
                    )
                    .withTier(Tier.STANDARD)
                    .withComputeProfile(new ComputeProfile()
                        .withRoles(List.of(
                            new Role()
                                .withName("headnode")
                                .withTargetInstanceCount(2)
                                .withHardwareProfile(new HardwareProfile()
                                    .withVmSize("Large")
                                )
                                .withOsProfile(new OsProfile()
                                    .withLinuxOperatingSystemProfile(new LinuxOperatingSystemProfile()
                                            .withUsername(username)
                                            .withPassword(password)
                                    )
                                ),
                            new Role()
                                    .withName("workernode")
                                    .withTargetInstanceCount(1)
                                    .withHardwareProfile(new HardwareProfile()
                                        .withVmSize("Large")
                                    )
                                    .withOsProfile(new OsProfile()
                                        .withLinuxOperatingSystemProfile(new LinuxOperatingSystemProfile()
                                            .withUsername(username)
                                            .withPassword(password)
                                        )
                                    )
                        ))
                    )
                    .withStorageProfile(new StorageProfile()
                        .withStorageaccounts(List.of(
                                new StorageAccount()
                                    .withName(storageAccount)
                                    .withKey(storageAccountKey)
                                    .withContainer(container)
                                    .withIsDefault(true)
                        ))
                    )
            );
        client.clusters().create(resourceGroupName, clusterName, parameters);
```

### <a name="get-cluster-details"></a><span data-ttu-id="395cf-152">取得叢集詳細資料</span><span class="sxs-lookup"><span data-stu-id="395cf-152">Get Cluster Details</span></span>

<span data-ttu-id="395cf-153">若要取得特定叢集的屬性：</span><span class="sxs-lookup"><span data-stu-id="395cf-153">To get properties for a given cluster:</span></span>

```java
client.clusters.getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="395cf-154">範例</span><span class="sxs-lookup"><span data-stu-id="395cf-154">Example</span></span>

<span data-ttu-id="395cf-155">您可以使用 `get` 來確認您已成功建立叢集。</span><span class="sxs-lookup"><span data-stu-id="395cf-155">You can use `get` to confirm that you have successfully created your cluster.</span></span>

```java
ClusterInner cluster = client.clusters().getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
System.out.println(cluster.name()); //Prints the name of the cluster
System.out.println(cluster.id()); //Prints the resource Id of the cluster
```

<span data-ttu-id="395cf-156">輸出應會顯示如下：</span><span class="sxs-lookup"><span data-stu-id="395cf-156">The output should look like:</span></span>

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a><span data-ttu-id="395cf-157">列出叢集</span><span class="sxs-lookup"><span data-stu-id="395cf-157">List Clusters</span></span>

#### <a name="list-clusters-under-the-subscription"></a><span data-ttu-id="395cf-158">列出訂用帳戶下的叢集</span><span class="sxs-lookup"><span data-stu-id="395cf-158">List Clusters Under The Subscription</span></span>

```java
client.clusters.list();
```
#### <a name="list-clusters-by-resource-group"></a><span data-ttu-id="395cf-159">依資源群組列出叢集</span><span class="sxs-lookup"><span data-stu-id="395cf-159">List Clusters By Resource Group</span></span>

```java
client.clusters.listByResourceGroup("<Resource Group Name>");
```
> [!NOTE]
> <span data-ttu-id="395cf-160">`List()` 和 `ListByResourceGroup()` 都會傳回 `PagedList<ClusterInner>` 物件。</span><span class="sxs-lookup"><span data-stu-id="395cf-160">Both `List()` and `ListByResourceGroup()` return a `PagedList<ClusterInner>` object.</span></span> <span data-ttu-id="395cf-161">呼叫 `loadNext()` 會傳回該頁面上的叢集清單，並將 `ClusterPaged` 物件往前送到下一個頁面。</span><span class="sxs-lookup"><span data-stu-id="395cf-161">Calling `loadNext()` returns a list of clusters on that page and advances the `ClusterPaged` object to the next page.</span></span> <span data-ttu-id="395cf-162">這可以重複到 `hasNextPage()` 傳回 `false` 為止，表示沒有更多的頁面。</span><span class="sxs-lookup"><span data-stu-id="395cf-162">This can be repeated until `hasNextPage()` return `false`, indicating that there are no more pages.</span></span>

#### <a name="example"></a><span data-ttu-id="395cf-163">範例</span><span class="sxs-lookup"><span data-stu-id="395cf-163">Example</span></span>

<span data-ttu-id="395cf-164">下列範例會列印目前訂用帳戶所有叢集的屬性：</span><span class="sxs-lookup"><span data-stu-id="395cf-164">The following example prints the properties of all clusters for the current subscription:</span></span>

```java
PagedList<ClusterInner> clusterPages = client.clusters().list();
while (true) {
    for (ClusterInner cluster : clusterPages.currentPage().items()) {
        System.out.println(cluster.name());
    }
    if (clusterPages.hasNextPage()) {
        clusterPages.loadNextPage();
    } else {
        break;
    }
}
```

### <a name="delete-a-cluster"></a><span data-ttu-id="395cf-165">刪除叢集</span><span class="sxs-lookup"><span data-stu-id="395cf-165">Delete a Cluster</span></span>

<span data-ttu-id="395cf-166">若要刪除叢集：</span><span class="sxs-lookup"><span data-stu-id="395cf-166">To delete a cluster:</span></span>

```java
client.clusters.delete("<Resource Group Name>", "<Cluster Name>");
```

### <a name="update-cluster-tags"></a><span data-ttu-id="395cf-167">更新叢集標記</span><span class="sxs-lookup"><span data-stu-id="395cf-167">Update Cluster Tags</span></span>

<span data-ttu-id="395cf-168">您可以更新指定叢集的標記，如下所示：</span><span class="sxs-lookup"><span data-stu-id="395cf-168">You can update the tags of a given cluster like so:</span></span>

```java
client.clusters.update("<Resource Group Name>", "<Cluster Name>", <Map<String,String> of Tags>);
```

### <a name="resize-cluster"></a><span data-ttu-id="395cf-169">調整叢集大小</span><span class="sxs-lookup"><span data-stu-id="395cf-169">Resize Cluster</span></span>

<span data-ttu-id="395cf-170">您可以藉由指定新的大小來調整指定叢集的背景工作節點數目，如下所示：</span><span class="sxs-lookup"><span data-stu-id="395cf-170">You can resize a given cluster's number of worker nodes by specifying a new size like so:</span></span>

```java
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", <Num of Worker Nodes (int)>)
```

## <a name="cluster-monitoring"></a><span data-ttu-id="395cf-171">叢集監視</span><span class="sxs-lookup"><span data-stu-id="395cf-171">Cluster Monitoring</span></span>

<span data-ttu-id="395cf-172">HDInsight 管理 SDK 也可用來透過 Operations Management Suite (OMS) 管理您對叢集的監視。</span><span class="sxs-lookup"><span data-stu-id="395cf-172">The HDInsight Management SDK can also be used to manage monitoring on your clusters via the Operations Management Suite (OMS).</span></span>

### <a name="enable-oms-monitoring"></a><span data-ttu-id="395cf-173">啟用 OMS 監視</span><span class="sxs-lookup"><span data-stu-id="395cf-173">Enable OMS Monitoring</span></span>

> [!NOTE]
> <span data-ttu-id="395cf-174">若要啟用 OMS 監視，您必須擁有現有的 Log Analytics 工作區。</span><span class="sxs-lookup"><span data-stu-id="395cf-174">To enable OMS Monitoring, you must have an existing Log Analytics workspace.</span></span> <span data-ttu-id="395cf-175">如果您尚未建立工作區，您可以在此了解如何建立：[在 Azure 入口網站中建立 Log Analytics 工作區](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)。</span><span class="sxs-lookup"><span data-stu-id="395cf-175">If you have not already created one, you can learn how to do that here: [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span></span>

<span data-ttu-id="395cf-176">若要對您的叢集啟用 OMS 監視：</span><span class="sxs-lookup"><span data-stu-id="395cf-176">To enable OMS Monitoring on your cluster:</span></span>

```java
client.extensions().enableMonitoring("<Resource Group Name", "<Cluster Name>", new ClusterMonitoringRequest().withWorkspaceId("<Workspace Id>"));
```

### <a name="view-status-of-oms-monitoring"></a><span data-ttu-id="395cf-177">檢視 OMS 監視的狀態</span><span class="sxs-lookup"><span data-stu-id="395cf-177">View Status Of OMS Monitoring</span></span>

<span data-ttu-id="395cf-178">若要取得叢集的 OMS 狀態：</span><span class="sxs-lookup"><span data-stu-id="395cf-178">To get the status of OMS on your cluster:</span></span>

```java
client.extensions().getMonitoringStatus("<Resource Group Name", "Cluster Name");
```

### <a name="disable-oms-monitoring"></a><span data-ttu-id="395cf-179">停用 OMS 監視</span><span class="sxs-lookup"><span data-stu-id="395cf-179">Disable OMS Monitoring</span></span>

<span data-ttu-id="395cf-180">若要對您的叢集停用 OMS：</span><span class="sxs-lookup"><span data-stu-id="395cf-180">To disable OMS on your cluster:</span></span>

```java
client.extensions().disableMonitoring("<Resource Group Name>", "<Cluster Name>");
```

## <a name="script-actions"></a><span data-ttu-id="395cf-181">指令碼動作</span><span class="sxs-lookup"><span data-stu-id="395cf-181">Script Actions</span></span>

<span data-ttu-id="395cf-182">HDInsight 提供名為指令碼動作的設定方法，此方法會叫用自訂指令碼來自訂叢集。</span><span class="sxs-lookup"><span data-stu-id="395cf-182">HDInsight provides a configuration method called script actions that invokes custom scripts to customize the cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="395cf-183">您可以在此處找到如何使用指令碼動作的詳細資訊：[使用指令碼動作自訂 Linux 型 HDInsight 叢集](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span><span class="sxs-lookup"><span data-stu-id="395cf-183">More information on how to use script actions can be found here: [Customize Linux-based HDInsight clusters using script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span></span>

### <a name="execute-script-actions"></a><span data-ttu-id="395cf-184">執行指令碼動作</span><span class="sxs-lookup"><span data-stu-id="395cf-184">Execute Script Actions</span></span>

<span data-ttu-id="395cf-185">您可以對指定的叢集執行指令碼動作，如下所示：</span><span class="sxs-lookup"><span data-stu-id="395cf-185">You can execute script actions on a given cluster like so:</span></span>

```java
RuntimeScriptAction scriptAction1 = new RuntimeScriptAction()
    .withName("<Script Name>")
    .withUri("<URL To Script>")
    .withRoles(<List<String> of roles>);
client.clusters().executeScriptActions(
    resourceGroupName, 
    clusterName, 
    new ExecuteScriptActionParameters().withPersistOnSuccess(false).withScriptActions(new LinkedList<>(Arrays.asList(scriptAction1)))); //add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a><span data-ttu-id="395cf-186">刪除指令碼動作</span><span class="sxs-lookup"><span data-stu-id="395cf-186">Delete Script Action</span></span>

<span data-ttu-id="395cf-187">若要刪除對給定叢集指定的持續性指令碼動作：</span><span class="sxs-lookup"><span data-stu-id="395cf-187">To delete a specified persisted script action on a given cluster:</span></span>

```java
client.scriptActions().delete("<Resource Group Name>", "<Cluster Name>", "<Script Name>");
```

### <a name="list-persisted-script-actions"></a><span data-ttu-id="395cf-188">列出持續性指令碼動作</span><span class="sxs-lookup"><span data-stu-id="395cf-188">List Persisted Script Actions</span></span>

> [!NOTE]
> <span data-ttu-id="395cf-189">`listByCluster()` 會傳回 `PagedList<RuntimeScriptActionDetailInner>` 物件。</span><span class="sxs-lookup"><span data-stu-id="395cf-189">Both `listByCluster()` returns a `PagedList<RuntimeScriptActionDetailInner>` object.</span></span> <span data-ttu-id="395cf-190">呼叫 `currentPage().items()` 會傳回 `RuntimeScriptActionDetailInner` 清單，而 `loadNextPage()` 會前進到下一個頁面。</span><span class="sxs-lookup"><span data-stu-id="395cf-190">Calling `currentPage().items()` returns a list of `RuntimeScriptActionDetailInner`, and `loadNextPage()` advances to the next page.</span></span> <span data-ttu-id="395cf-191">這可以重複到 `hasNextPage()` 傳回 `false` 為止，表示沒有更多的頁面。</span><span class="sxs-lookup"><span data-stu-id="395cf-191">This can be repeated until `hasNextPage()` returns `false`, indicating that there are no more pages.</span></span>

<span data-ttu-id="395cf-192">若要列出指定叢集的所有持續性指令碼動作：</span><span class="sxs-lookup"><span data-stu-id="395cf-192">To list all persisted script actions for the specified cluster:</span></span>
```java
client.scriptActions().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="395cf-193">範例</span><span class="sxs-lookup"><span data-stu-id="395cf-193">Example</span></span>

```java
PagedList<RuntimeScriptActionDetailInner> scriptsPaged = client.scriptActions().listByCluster(resourceGroupName, clusterName);
while (true) {
    for (RuntimeScriptActionDetailInner script : scriptsPaged.currentPage().items()) {
        System.out.println(script.name()); //There are methods to get other properties of RuntimeScriptActionDetail besides name(), such as status(), operation(), startTime(), endTime(), etc. See reference documentation.
    }
    if (scriptsPaged.hasNextPage()) {
        scriptsPaged.loadNextPage();
    } else {
        break;
    }
}
```

### <a name="list-all-scripts-execution-history"></a><span data-ttu-id="395cf-194">列出所有指令碼的執行歷程記錄</span><span class="sxs-lookup"><span data-stu-id="395cf-194">List All Scripts' Execution History</span></span>

<span data-ttu-id="395cf-195">若要針對指定的叢集列出所有指令碼的執行歷程記錄：</span><span class="sxs-lookup"><span data-stu-id="395cf-195">To list all scripts' execution history for the specified cluster:</span></span>

```java
client.scriptExecutionHistorys().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="395cf-196">範例</span><span class="sxs-lookup"><span data-stu-id="395cf-196">Example</span></span>

<span data-ttu-id="395cf-197">此範例會列印過去所有指令碼執行的所有詳細資料。</span><span class="sxs-lookup"><span data-stu-id="395cf-197">This example prints all the details for all past script executions.</span></span>

```java
PagedList<RuntimeScriptActionDetailInner> scriptExecutionsPaged = client.scriptExecutionHistorys().listByCluster(resourceGroupName, clusterName);
while (true) {
    for (RuntimeScriptActionDetailInner script : scriptExecutionsPaged.currentPage().items()) {
        System.out.println(script.name()); //There are methods to get other properties of RuntimeScriptActionDetail besides name(), such as status(), operation(), startTime(), endTime(), etc. See reference documentation.
    }
    if (scriptExecutionsPaged.hasNextPage()) {
        scriptExecutionsPaged.loadNextPage();
    } else {
        break;
    }
}
```
