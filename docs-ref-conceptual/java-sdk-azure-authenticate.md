---
title: 使用適用於 Java 的 Azure 管理程式庫來進行驗證
description: 使用用來進入適用於 Java 之 Azure 管理程式庫的服務主體來進行驗證
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
ms.assetid: 10f457e3-578b-4655-8cd1-51339226ee7d
ms.openlocfilehash: 1d556955fcc5b73f1ba099a0b846b571ba64ccff
ms.sourcegitcommit: 107c3c5ed8c6991c751f95bcaf3757220940df9e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2018
ms.locfileid: "34050725"
---
# <a name="authenticate-with-the-azure-libraries-for-java"></a>使用適用於 Java 的 Azure 程式庫來進行驗證 

## <a name="connect-to-services-with-connection-strings"></a>使用連接字串來連線到服務

Azure 服務程式庫大多會使用連接字串或安全金鑰來進行驗證。 例如，SQL Database 會在 JDBC 連接字串中包含使用者名稱和密碼資訊：

```java
String url = "jdbc:sqlserver://myazuredb.database.windows.net:1433;" + 
        "database=testjavadb;" + 
        "user=myazdbuser;" +
        "password=myazdbpass;" +
        "encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
        Connection conn = DriverManager.getConnection(url);
```

Azure 儲存體會使用儲存體金鑰來授權應用程式：

```java
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storageName 
        + ";AccountKey=" + storageKey
        + ";EndpointSuffix=core.windows.net";
```

服務的連接字串可用來向其他 Azure 服務 (例如 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService)、[Redis 快取](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started)和[服務匯流排](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues)) 進行驗證。 您可以使用 Azure 入口網站或 CLI 來取得連接字串。  您也可以使用適用於 Java 的 Azure 管理程式庫來查詢資源，從而在程式碼中建置連接字串。 

例如，此程式碼會使用管理程式庫來建立儲存體帳戶的連接字串：

```java
// create a new storage account
StorageAccount storage = azure.storageAccounts().getByResourceGroup("myResourceGroup","myStorageAccount");

// create a storage container to hold the file
List<StorageAccountKey> keys = storage.getKeys();
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storage.name()
        + ";AccountKey=" + keys.get(0).value()
        + ";EndpointSuffix=core.windows.net";
```

對於其他程式庫，應用程式就必須使用[服務主體](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)來執行，這個服務主體要授權應用程式使用獲授與的認證來執行。 這項設定很類似下面所列管理程式庫中的物件型驗證步驟。

<a name="mgmt-auth"></a>

##  <a name="authenticate-with-the-azure-management-libraries-for-java"></a>使用適用於 Java 的 Azure 管理程式庫來進行驗證

在使用 Java 管理程式庫來建立和管理資源時，有兩個選項可供您向 Azure 驗證應用程式。

### <a name="authenticate-with-an-applicationtokencredentials-object"></a>使用 ApplicationTokenCredentials 物件進行驗證

建立 `ApplicationTokenCredentials` 的執行個體從程式碼內對最上層的 `Azure` 物件提供服務主體認證。

```java
import com.microsoft.azure.credentials.ApplicationTokenCredentials;
import com.microsoft.azure.AzureEnvironment;

// ...

ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(client, 
        tenant,
        key, 
        AzureEnvironment.AZURE);
        
Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credentials)
        .withDefaultSubscription();
```

`client`、`tenant` 和 `key` 是用於[檔案型驗證](#mgmt-file)的相同服務主體值。 `AzureEnvironment.AZURE` 值會對 Azure 公用雲端建立認證。 如果您需要存取其他雲端，請將此值變更為不同的值 (例如，`AzureEnvironment.AZURE_GERMANY`)。  

 從環境變數或祕密管理存放區 (如 [Key Vault](/azure/key-vault/key-vault-whatis)) 讀取服務主體值。 請避免在程式碼中將這些值設定為純文字字串，以免不小心地在版本控制記錄中公開認證。   

<a name="mgmt-file"></a>

### <a name="file-based-authentication-preview"></a>檔案型驗證 (預覽)

最簡單的驗證方式是建立屬性檔，在其中包含適用於 [Azure 服務主體](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)且使用下列格式的認證：

```text
# sample management library properties file
subscription=########-####-####-####-############
client=########-####-####-####-############
key=XXXXXXXXXXXXXXXX
tenant=########-####-####-####-############
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

- subscription：使用在 Azure CLI 2.0 中透過 `az account show` 所得到的 id 值。
- client：使用從建立來執行應用程式之服務主體所擷取之輸出中得到的 appId 值。 如果您沒有適用於應用程式的服務主體，請[使用 Azure CLI 2.0 建立服務主體](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)。
- key：使用從服務主體建立 CLI 輸出所得到的 password 值 
- tenant：使用從服務主體建立 CLI 輸出所得到的 tenant 值

將此檔案儲存在系統上可供程式碼讀取且安全的位置。 在殼層中使用該檔案的完整路徑來設定環境變數：

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

建立進入點 `Azure` 物件來開始使用程式庫。 透過環境變數讀取屬性檔的位置。

```java
// pull in the location of the authenticaiton properties file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```



