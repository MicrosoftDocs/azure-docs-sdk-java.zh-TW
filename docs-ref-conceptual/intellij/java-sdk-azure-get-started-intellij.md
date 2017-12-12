---
title: "使用 Intellij 來對 Java 開始使用 Azure"
description: "使用自有 Azure 訂用帳戶開始進行適用於 Java 之 Azure 程式庫的基本使用。"
keywords: "Azure, Java, SDK, API, 驗證, 開始使用"
author: roygara
ms.author: v-rogara
manager: timlt
ms.date: 10/30/2017
ms.topic: get-started-article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.openlocfilehash: 1e10a7c5a46ed0e36143fd4a99decc037c04e1fe
ms.sourcegitcommit: fcf1189ede712ae30f8c7626bde50c9b8bb561bc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/01/2017
---
# <a name="get-started-with-the-azure-libraries-using-intellij"></a>使用 Intellij 來開始使用 Azure 程式庫

本指南會逐步引導您設定開發環境和使用適用於 Java 的 Azure 程式庫。 您會建立服務主體來使用 Azure 進行驗證，並執行某些程式碼範例以在訂用帳戶中建立及使用 Azure 資源。 在使用 Azure 開發 Java 時，您可以選擇使用 Intellij。 只要是具有 Maven 整合的 IDE 都能運作。 或者，如果您不想使用 IDE，則可以使用 Maven 從命令列執行程式碼。

## <a name="prerequisites"></a>必要條件

- 一個 Azure 帳戶。 如果您沒有帳戶，請[取得免費試用帳戶](https://azure.microsoft.com/free/)
- [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) 或 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。
- 最新的 [Intellij](https://www.jetbrains.com/idea/) 穩定版

## <a name="set-up-authentication"></a>設定驗證

Java 應用程式必須有 Azure 訂用帳戶的讀取和建立權限，才能在此教學課程中執行程式碼範例。 請建立服務主體，並將應用程式設定為使用其認證來執行。 服務主體可讓您建立與身分識別相關聯的非互動式帳戶，而且對於此身分識別，您只賦予它應用程式執行時所需的權限。

[建立服務主體](/cli/azure/create-an-azure-service-principal-azure-cli)來對程式碼授與權限，讓它不必直接使用帳戶認證，就能在訂用帳戶中建立和更新資源。 請務必要擷取輸出。 在 password 引數中提供[安全的密碼](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy)，而不是提供 `MY_SECURE_PASSWORD`。 密碼必須介於 8 到 16 個字元，而且至少符合下列 4 個準則中的其中 3 個：

* 包含小寫字元
* 包含大寫字元
* 包含數字
* 包含下列其中一個符號：@ # $ % ^ & * - _ ! + = [ ] { } | \ : ‘ , . ? / ` ~ “ ( ) ;


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

這會提供您下列格式的回覆：

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

接下來，將下列內容複製到您系統上的文字檔：

```text
# sample management library properties file
subscription=ssssssss-ssss-ssss-ssss-ssssssssssss
client=cccccccc-cccc-cccc-cccc-cccccccccccc
key=kkkkkkkkkkkkkkkk
tenant=tttttttt-tttt-tttt-tttt-tttttttttttt
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

將前四個值替換為以下內容：

- subscription：使用在 Azure CLI 2.0 中透過 `az account show` 所得到的 id 值。
- client：使用從服務主體輸出所擷取之輸出中得到的 appId 值。
- key：使用從服務主體輸出所得到的 password 值。
- tenant：使用從服務主體輸出所得到的 tenant 值。

將此檔案儲存在系統上可供程式碼讀取且安全的位置。 您可以將此檔案用於日後撰寫的程式碼，因此建議您將其儲存在本文應用程式以外的位置。 

在殼層中使用驗證檔案的完整路徑來設定環境變數 `AZURE_AUTH_LOCATION`。  

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

如果您是在 Windows 環境中工作，請在系統屬性中新增該變數。 開啟 PowerShell，並在以檔案路徑取代第二個變數之後，輸入下列命令：

```powershell
[Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\<fullpath>\azureauth.properties", "Machine")
```

## <a name="create-a-new-maven-project"></a>建立新的 Maven 專案

> [!NOTE]
> 本指南使用 Maven 建置工具來建置及執行程式碼範例，但 Gradle 等其他建置工具也能與適用於 Java 的 Azure 程式庫搭配運作。 

開啟 Intellij，選取 [檔案] > [新增] > [專案]...然後繼續進行下一個畫面。

輸入 "com.fabrikam" 作為 groupID，並輸入您選擇的 artifactID。

繼續進行最後一個畫面，並完成專案的建立。

現在，開啟 pom.xml 檔案。 然後新增下列程式碼：

```XML
<dependencies>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.0.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
</dependencies>
```

儲存 pom.xml。
   
## <a name="install-the-azure-toolkit-for-intellij"></a>安裝 Azure Toolkit for Intellij

如果您要以程式設計方式部署 Web 應用程式或 API，就必須要有 [Azure 工具組](azure-toolkit-for-intellij-installation.md)，但此工具組目前並未用於任何其他種類的開發。 以下是安裝程序的摘要。 如需詳細步驟，請瀏覽[安裝 Azure Toolkit for Intellij](azure-toolkit-for-intellij-installation.md)。

選取 [檔案] 功能表，然後選取 [設定...]。 

選取 [瀏覽存放庫...] 並搜尋 "Azure"，然後安裝 **Azure Toolkit for Intellij**。

重新啟動 Intellij。

## <a name="create-a-linux-virtual-machine"></a>建立 Linux 虛擬機器

在專案的 `src/main/java` 目錄中建立名為 `AzureApp.java` 的新檔案，然後貼上以下程式碼區塊。 使用機器的實際值來更新 `userName` 和 `sshKey` 變數。 此程式碼會在美國東部 Azure 區域執行的 `sampleResourceGroup` 資源群組中，建立名為 `testLinuxVM` 的新 Linux VM。

若要建立 `sshkey`，請開啟 Azure Cloud Shell，然後輸入 `ssh-keygen -t rsa -b 2048`。 輸入檔案名稱，然後存取 .public 檔案以取得金鑰 (您會在下列程式碼中用到)，將其全數複製並貼到您的變數 `sshKey` 中。

```java

import com.microsoft.azure.management.Azure;
import com.microsoft.azure.management.compute.VirtualMachine;
import com.microsoft.azure.management.compute.KnownLinuxVirtualMachineImage;
import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
import com.microsoft.azure.management.appservice.PricingTier;
import com.microsoft.azure.management.appservice.WebApp;
import com.microsoft.azure.management.storage.StorageAccount;
import com.microsoft.azure.management.storage.SkuName;
import com.microsoft.azure.management.storage.StorageAccountKey;
import com.microsoft.azure.management.sql.SqlDatabase;
import com.microsoft.azure.management.sql.SqlServer;
import com.microsoft.azure.management.resources.fluentcore.arm.Region;
import com.microsoft.azure.management.resources.fluentcore.utils.SdkContext;

import com.microsoft.rest.LogLevel;

import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

import java.io.File;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.List;

public class AzureApp {

    public static void main(String[] args) {

        final String userName = "YOUR_VM_USERNAME";
        final String sshKey = "YOUR_PUBLIC_SSH_KEY";

        try {

            // use the properties file with the service principal information to authenticate
            // change the name of the environment variable if you used a different name in the previous step
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));    
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();
           
            // create a Ubuntu virtual machine in a new resource group 
            VirtualMachine linuxVM = azure.virtualMachines().define("testLinuxVM")
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleVmResourceGroup")
                    .withNewPrimaryNetwork("10.0.0.0/24")
                    .withPrimaryPrivateIPAddressDynamic()
                    .withoutPrimaryPublicIPAddress()
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withUnmanagedDisks()
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();   

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```


您會在主控台中看到某些 REST 要求和回應，因為 SDK 會對 Azure REST API 進行基礎呼叫，以設定虛擬機器和其資源。 當程式完成時，請使用 Azure CLI 2.0 在訂用帳戶中確認虛擬機器：

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

在確認程式碼有效後，請使用 CLI 來刪除 VM 和其資源。

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>從 GitHub 存放庫部署 Web 應用程式

將 `AzureApp.java` 中的主要方法替換為下列內容，並將 `appName` 變數更新為唯一值再執行程式碼。 此程式碼會將公用 GitHub 存放庫中 `master` 分支內的 Web 應用程式，部署至執行於免費定價層的新的 [Azure App Service Web 應用程式](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview)。

```java
    public static void main(String[] args) {
        try {

            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            final String appName = "YOUR_APP_NAME";

            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            WebApp app = azure.webApps().define(appName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleWebResourceGroup")
                    .withNewWindowsPlan(PricingTier.FREE_F1)
                    .defineSourceControl()
                    .withPublicGitRepository(
                            "https://github.com/Azure-Samples/app-service-web-java-get-started")
                    .withBranch("master")
                    .attach()
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

使用 Maven 如往常一樣地執行程式碼：

使用 CLI 開啟瀏覽器並讓其指向該應用程式：

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```
確認過部署後，請從訂用帳戶中移除 Web 應用程式和方案。

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a>連線到 Azure SQL Database

使用下列程式碼取代 `AzureApp.java` 中目前的主要方法，並為 `dbPassword` 變數設定實際的值。
此程式碼會使用允許遠端存取的防火牆規則來建立新的 SQL 資料庫，然後使用 SQL Database JBDC 驅動程式來與該資料庫連線。 

```java

    public static void main(String args[])
    {
        // create the db using the management libraries
        try {
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            final String adminUser = SdkContext.randomResourceName("db",8);
            final String sqlServerName = SdkContext.randomResourceName("sql",10);
            final String sqlDbName = SdkContext.randomResourceName("dbname",8);
            final String dbPassword = "YOUR_PASSWORD_HERE";


            SqlServer sampleSQLServer = azure.sqlServers().define(sqlServerName)
                            .withRegion(Region.US_EAST)
                            .withNewResourceGroup("sampleSqlResourceGroup")
                            .withAdministratorLogin(adminUser)
                            .withAdministratorPassword(dbPassword)
                            .withNewFirewallRule("0.0.0.0","255.255.255.255")
                            .create();

            SqlDatabase sampleSQLDb = sampleSQLServer.databases().define(sqlDbName).create();

            // assemble the connection string to the database
            final String domain = sampleSQLServer.fullyQualifiedDomainName();
            String url = "jdbc:sqlserver://"+ domain + ":1433;" +
                    "database=" + sqlDbName +";" +
                    "user=" + adminUser+ "@" + sqlServerName + ";" +
                    "password=" + dbPassword + ";" +
                    "encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";

            // connect to the database, create a table and insert a entry into it
            Connection conn = DriverManager.getConnection(url);

            String createTable = "CREATE TABLE CLOUD ( name varchar(255), code int);";
            String insertValues = "INSERT INTO CLOUD (name, code ) VALUES ('Azure', 1);";
            String selectValues = "SELECT * FROM CLOUD";
            Statement createStatement = conn.createStatement();
            createStatement.execute(createTable);
            Statement insertStatement = conn.createStatement();
            insertStatement.execute(insertValues);
            Statement selectStatement = conn.createStatement();
            ResultSet rst = selectStatement.executeQuery(selectValues);

            while (rst.next()) {
                System.out.println(rst.getString(1) + " "
                        + rst.getString(2));
            }


        } catch (Exception e) {
            System.out.println(e.getMessage());
            System.out.println(e.getStackTrace().toString());
        }
    }
```
從命令列執行範例：

```
mvn clean compile exec:java
```

然後使用 CLI 清除資源：

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a>將 blob 寫入到新的儲存體帳戶

使用下列程式碼取代 `AzureApp.java` 中目前的主要方法。 此程式碼會建立 [Azure 儲存體帳戶](https://docs.microsoft.com/azure/storage/storage-introduction)，然後使用適用於 Java 的 Azure 儲存體程式庫在雲端建立新的文字檔。

```java
public static void main(String[] args) {

    try {

        // use the properties file with the service principal information to authenticate
        // change the name of the environment variable if you used a different name in the previous step
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
                .withLogLevel(LogLevel.BASIC)
                .authenticate(credFile)
                .withDefaultSubscription();

        // create a new storage account
        String storageAccountName = SdkContext.randomResourceName("st",8);
        StorageAccount storage = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleStorageResourceGroup")
                    .create();

        // create a storage container to hold the file
        List<StorageAccountKey> keys = storage.getKeys();
        final String storageConnection = "DefaultEndpointsProtocol=https;"
                + "AccountName=" + storage.name()
                + ";AccountKey=" + keys.get(0).value()
                + ";EndpointSuffix=core.windows.net";

        CloudStorageAccount account = CloudStorageAccount.parse(storageConnection);
        CloudBlobClient serviceClient = account.createCloudBlobClient();

        // Container name must be lower case.
        CloudBlobContainer container = serviceClient.getContainerReference("helloazure");
        container.createIfNotExists();

        // Make the container public
        BlobContainerPermissions containerPermissions = new BlobContainerPermissions();
        containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
        container.uploadPermissions(containerPermissions);

        // write a blob to the container
        CloudBlockBlob blob = container.getBlockBlobReference("helloazure.txt");
        blob.uploadText("hello Azure");

    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }
    }
}
```

從命令列執行範例：

您可以透過 Azure 入口網站或 [Azure 儲存體總管](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) 來瀏覽儲存體帳戶中的 `helloazure.txt` 檔案。

使用 CLI 清除儲存體帳戶：

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>探索更多範例

若要深入了解如何使用適用於 Java 的 Azure 管理程式庫來管理資源和自動執行工作，請參閱我們針對[虛擬機器](../java-sdk-azure-virtual-machine-samples.md)、[Web 應用程式](../java-sdk-azure-web-apps-samples.md)和 [SQL 資料庫](../java-sdk-azure-sql-database-samples.md)所提供的程式碼範例。

## <a name="reference-and-release-notes"></a>參考資料和版本資訊

我們針對所有套件提供了[參考資料](http://docs.microsoft.com/java/api)。

## <a name="get-help-and-give-feedback"></a>獲得協助及提供意見

您可以在 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java) 的社群中張貼問題。 若要針對適用於 Java 的 Azure 程式庫回報錯誤和建立問題，請至 [GitHub 專案](https://github.com/Azure/azure-sdk-for-java)。
