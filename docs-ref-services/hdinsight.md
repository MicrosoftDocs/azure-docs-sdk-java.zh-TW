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
ms.openlocfilehash: 3827b5744a5d08c53cbbff1db29eca34194c1625
ms.sourcegitcommit: 1c1412ad5d8960975c3fc7fd3d1948152ef651ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57335431"
---
# <a name="hdinsight-java-management-sdk-preview"></a>HDInsight Java 管理 SDK (預覽)

## <a name="overview"></a>概觀

HDInsight Java SDK 提供可讓您管理 HDInsight 叢集的類別和方法。 它包含用來建立、刪除、更新、列出、調整大小、執行指令碼動作、監視、取得 HDInsight 叢集屬性的作業，和其他多種作業。

## <a name="prerequisites"></a>必要條件

* 一個 Azure 帳戶。 如果您沒有帳戶，請[取得免費試用帳戶](https://azure.microsoft.com/free/)。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* [Maven](https://maven.apache.org/install.html)

## <a name="sdk-installation"></a>SDK 安裝

您可以在[此處](https://mvnrepository.com/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight)透過 Maven 取得 HDInsight Java SDK。 將下列相依性新增至 pom.xml 中：

```
<dependency>
    <groupId>com.microsoft.azure.hdinsight.v2018_06_01_preview</groupId>
    <artifactId>azure-mgmt-hdinsight</artifactId>
    <version>1.0.0-beta-1</version>
</dependency>
```

您也需要將下列相依性新增至 pom.xml：

* [Azure 用戶端驗證程式庫：](https://mvnrepository.com/artifact/com.microsoft.azure/azure-client-authentication/1.6.2)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-client-authentication</artifactId>
    <version>1.6.2</version>
  </dependency>
  ```

* [適用於 ARM 的 Azure Java 用戶端執行階段：](https://mvnrepository.com/artifact/com.microsoft.azure/azure-arm-client-runtime/1.6.2)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-arm-client-runtime</artifactId>
    <version>1.6.2</version>
  </dependency>
  ```

## <a name="authentication"></a>Authentication

SDK 必須先使用您的 Azure 訂用帳戶進行驗證。  請依照下列範例建立服務主體，並使用它來驗證。 此動作完成後，您會有 `HDInsightManagementClientImpl` 的執行個體，其中包含許多可用來執行管理作業的方法 (分述於下列各節中)。

> [!NOTE]
> 除了下列範例以外，還有其他方式可進行驗證，可能更符合您的需求。 此處概述所有方法：[使用適用於 Java 的 Azure 管理程式庫來進行驗證](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)

### <a name="authentication-example-using-a-service-principal"></a>使用服務主體的驗證範例

首先，請登入 [Azure Cloud Shell](https://shell.azure.com/bash)。 確認您目前使用的訂用帳戶，是您希望建立服務主體的位置。 

```azurecli-interactive
az account show
```

您的訂用帳戶資訊會以 JSON 的形式顯示。

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

如果您未登入正確的訂用帳戶，請執行下列命令以選取正確的訂用帳戶： 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> 如果您尚未以其他方法註冊 HDInsight 資源提供者 (例如，透過 Azure 入口網站建立 HDInsight 叢集)，您必須立即執行此動作，才能進行驗證。 此動作也可以從 [Azure Cloud Shell](https://shell.azure.com/bash) 完成，只要執行下列命令即可：
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

接下來，請選擇服務主體的名稱，並使用下列命令加以建立：

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

服務主體資訊會顯示為 JSON。

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
複製下列程式碼片段，並且在 `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET` 和 `SUBSCRIPTION_ID` 中填入在執行建立服務主體的命令後傳回的 JSON 中所包含的字串。

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


## <a name="cluster-management"></a>叢集管理

> [!NOTE]
> 本節假設您已通過驗證並建構 `HDInsightManagementClientImpl` 執行個體，並且將其儲存在名為 `client` 的變數中。 驗證及取得 `HDInsightManagementClientImpl` 的指示可在先前的「驗證」一節中找到。

### <a name="create-a-cluster"></a>建立叢集

新叢集可藉由呼叫 `client.clusters().create()` 來建立。

#### <a name="example"></a>範例

此範例示範如何使用 2 個前端節點和 1 個背景工作節點來建立 Spark 叢集。

> [!NOTE]
> 您必須先建立資源群組和儲存體帳戶，說明如下。 如果您已建立這些項目，則可以略過這些步驟。

##### <a name="creating-a-resource-group"></a>建立資源群組

您可以使用 [Azure Cloud Shell](https://shell.azure.com/bash) 建立資源群組，只要執行下列命令即可
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a>建立儲存體帳戶

您可以使用 [Azure Cloud Shell](https://shell.azure.com/bash) 建立儲存體帳戶，只要執行下列命令即可：
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
現在請執行下列命令，以取得儲存體帳戶的金鑰 (您需要此金鑰才能建立叢集)：
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
下列 Java 程式碼片段會使用 2 個前端節點和 1 個背景工作節點來建立 Spark 叢集。 請依照註解中的說明填入空白變數，並依據您的特定需求變更其他參數。

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

### <a name="get-cluster-details"></a>取得叢集詳細資料

若要取得特定叢集的屬性：

```java
client.clusters.getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a>範例

您可以使用 `get` 來確認您已成功建立叢集。

```java
ClusterInner cluster = client.clusters().getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
System.out.println(cluster.name()); //Prints the name of the cluster
System.out.println(cluster.id()); //Prints the resource Id of the cluster
```

輸出應會顯示如下：

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a>列出叢集

#### <a name="list-clusters-under-the-subscription"></a>列出訂用帳戶下的叢集

```java
client.clusters.list();
```
#### <a name="list-clusters-by-resource-group"></a>依資源群組列出叢集

```java
client.clusters.listByResourceGroup("<Resource Group Name>");
```
> [!NOTE]
> `List()` 和 `ListByResourceGroup()` 都會傳回 `PagedList<ClusterInner>` 物件。 呼叫 `loadNext()` 會傳回該頁面上的叢集清單，並將 `ClusterPaged` 物件往前送到下一個頁面。 這可以重複到 `hasNextPage()` 傳回 `false` 為止，表示沒有更多的頁面。

#### <a name="example"></a>範例

下列範例會列印目前訂用帳戶所有叢集的屬性：

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

### <a name="delete-a-cluster"></a>刪除叢集

若要刪除叢集：

```java
client.clusters.delete("<Resource Group Name>", "<Cluster Name>");
```

### <a name="update-cluster-tags"></a>更新叢集標記

您可以更新指定叢集的標記，如下所示：

```java
client.clusters.update("<Resource Group Name>", "<Cluster Name>", <Map<String,String> of Tags>);
```

### <a name="resize-cluster"></a>調整叢集大小

您可以藉由指定新的大小來調整指定叢集的背景工作節點數目，如下所示：

```java
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", <Num of Worker Nodes (int)>)
```

## <a name="cluster-monitoring"></a>叢集監視

HDInsight 管理 SDK 也可用來透過 Operations Management Suite (OMS) 管理您對叢集的監視。

### <a name="enable-oms-monitoring"></a>啟用 OMS 監視

> [!NOTE]
> 若要啟用 OMS 監視，您必須擁有現有的 Log Analytics 工作區。 如果您尚未建立工作區，您可以在此了解如何建立：[在 Azure 入口網站中建立 Log Analytics 工作區](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)。

若要對您的叢集啟用 OMS 監視：

```java
client.extensions().enableMonitoring("<Resource Group Name", "<Cluster Name>", new ClusterMonitoringRequest().withWorkspaceId("<Workspace Id>"));
```

### <a name="view-status-of-oms-monitoring"></a>檢視 OMS 監視的狀態

若要取得叢集的 OMS 狀態：

```java
client.extensions().getMonitoringStatus("<Resource Group Name", "Cluster Name");
```

### <a name="disable-oms-monitoring"></a>停用 OMS 監視

若要對您的叢集停用 OMS：

```java
client.extensions().disableMonitoring("<Resource Group Name>", "<Cluster Name>");
```

## <a name="script-actions"></a>指令碼動作

HDInsight 提供名為指令碼動作的設定方法，此方法會叫用自訂指令碼來自訂叢集。
> [!NOTE]
> 您可以在此處找到如何使用指令碼動作的詳細資訊：[使用指令碼動作自訂 Linux 型 HDInsight 叢集](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)

### <a name="execute-script-actions"></a>執行指令碼動作

您可以對指定的叢集執行指令碼動作，如下所示：

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

### <a name="delete-script-action"></a>刪除指令碼動作

若要刪除對給定叢集指定的持續性指令碼動作：

```java
client.scriptActions().delete("<Resource Group Name>", "<Cluster Name>", "<Script Name>");
```

### <a name="list-persisted-script-actions"></a>列出持續性指令碼動作

> [!NOTE]
> `listByCluster()` 會傳回 `PagedList<RuntimeScriptActionDetailInner>` 物件。 呼叫 `currentPage().items()` 會傳回 `RuntimeScriptActionDetailInner` 清單，而 `loadNextPage()` 會前進到下一個頁面。 這可以重複到 `hasNextPage()` 傳回 `false` 為止，表示沒有更多的頁面。

若要列出指定叢集的所有持續性指令碼動作：
```java
client.scriptActions().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a>範例

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

### <a name="list-all-scripts-execution-history"></a>列出所有指令碼的執行歷程記錄

若要針對指定的叢集列出所有指令碼的執行歷程記錄：

```java
client.scriptExecutionHistorys().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a>範例

此範例會列印過去所有指令碼執行的所有詳細資料。

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
