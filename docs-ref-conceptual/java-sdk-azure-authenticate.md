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
ms.openlocfilehash: 88354039ad98050f5dee2e7eeadf7f399cd36014
ms.sourcegitcommit: 0f38ef9ad64cffdb7b2e9e966224dfd0af251b0f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2018
ms.locfileid: "42703381"
---
# <a name="authenticate-with-the-azure-libraries-for-java"></a><span data-ttu-id="d24ec-104">使用適用於 Java 的 Azure 程式庫來進行驗證</span><span class="sxs-lookup"><span data-stu-id="d24ec-104">Authenticate with the Azure libraries for Java</span></span> 

## <a name="connect-to-services-with-connection-strings"></a><span data-ttu-id="d24ec-105">使用連接字串來連線到服務</span><span class="sxs-lookup"><span data-stu-id="d24ec-105">Connect to services with connection strings</span></span>

<span data-ttu-id="d24ec-106">Azure 服務程式庫大多會使用連接字串或安全金鑰來進行驗證。</span><span class="sxs-lookup"><span data-stu-id="d24ec-106">Most Azure service libraries use a connection string or secure key for authentication.</span></span> <span data-ttu-id="d24ec-107">例如，SQL Database 會在 JDBC 連接字串中包含使用者名稱和密碼資訊：</span><span class="sxs-lookup"><span data-stu-id="d24ec-107">For example, SQL Database includes username and password information in the JDBC connection string:</span></span>

```java
String url = "jdbc:sqlserver://myazuredb.database.windows.net:1433;" + 
        "database=testjavadb;" + 
        "user=myazdbuser;" +
        "password=myazdbpass;" +
        "encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
        Connection conn = DriverManager.getConnection(url);
```

<span data-ttu-id="d24ec-108">Azure 儲存體會使用儲存體金鑰來授權應用程式：</span><span class="sxs-lookup"><span data-stu-id="d24ec-108">Azure Storage uses a storage key to authorize the application:</span></span>

```java
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storageName 
        + ";AccountKey=" + storageKey
        + ";EndpointSuffix=core.windows.net";
```

<span data-ttu-id="d24ec-109">服務的連接字串可用來向其他 Azure 服務 (例如 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService)、[Redis 快取](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started)和[服務匯流排](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues)) 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="d24ec-109">Service connection strings are used to authenticate to other Azure services like [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService), [Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started), and [Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues).</span></span> <span data-ttu-id="d24ec-110">您可以使用 Azure 入口網站或 CLI 來取得連接字串。</span><span class="sxs-lookup"><span data-stu-id="d24ec-110">You can get the connection strings using the Azure portal or the CLI.</span></span>  <span data-ttu-id="d24ec-111">您也可以使用適用於 Java 的 Azure 管理程式庫來查詢資源，從而在程式碼中建置連接字串。</span><span class="sxs-lookup"><span data-stu-id="d24ec-111">You can also use the Azure management libraries for Java to query resources to build connection strings in your code.</span></span> 

<span data-ttu-id="d24ec-112">例如，此程式碼會使用管理程式庫來建立儲存體帳戶的連接字串：</span><span class="sxs-lookup"><span data-stu-id="d24ec-112">For example, this code uses the management libraries to create a storage account connection string:</span></span>

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

<span data-ttu-id="d24ec-113">對於其他程式庫，應用程式就必須使用[服務主體](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)來執行，這個服務主體要授權應用程式使用獲授與的認證來執行。</span><span class="sxs-lookup"><span data-stu-id="d24ec-113">Other libraries require your application to run with a [service principal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) authorizing the application to run with granted credentials.</span></span> <span data-ttu-id="d24ec-114">這項設定很類似下面所列管理程式庫中的物件型驗證步驟。</span><span class="sxs-lookup"><span data-stu-id="d24ec-114">This configuration is similar to the object-based authentication steps for the management library listed below.</span></span>

<a name="mgmt-auth"></a>

##  <a name="authenticate-with-the-azure-management-libraries-for-java"></a><span data-ttu-id="d24ec-115">使用適用於 Java 的 Azure 管理程式庫來進行驗證</span><span class="sxs-lookup"><span data-stu-id="d24ec-115">Authenticate with the Azure management libraries for Java</span></span>

<span data-ttu-id="d24ec-116">在使用 Java 管理程式庫來建立和管理資源時，有兩個選項可供您向 Azure 驗證應用程式。</span><span class="sxs-lookup"><span data-stu-id="d24ec-116">Two options are available to authenticate your application with Azure when using the Java management libraries to create and manage resources.</span></span>

### <a name="authenticate-with-an-applicationtokencredentials-object"></a><span data-ttu-id="d24ec-117">使用 ApplicationTokenCredentials 物件進行驗證</span><span class="sxs-lookup"><span data-stu-id="d24ec-117">Authenticate with an ApplicationTokenCredentials object</span></span>

<span data-ttu-id="d24ec-118">建立 `ApplicationTokenCredentials` 的執行個體從程式碼內對最上層的 `Azure` 物件提供服務主體認證。</span><span class="sxs-lookup"><span data-stu-id="d24ec-118">Create an instance of `ApplicationTokenCredentials` to supply the service principal credentials to the top-level `Azure` object from inside your code.</span></span>

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

<span data-ttu-id="d24ec-119">`client`、`tenant` 和 `key` 是用於[檔案型驗證](#mgmt-file)的相同服務主體值。</span><span class="sxs-lookup"><span data-stu-id="d24ec-119">The `client`, `tenant` and `key` are the same service principal values used with [file-based authentication](#mgmt-file).</span></span> <span data-ttu-id="d24ec-120">`AzureEnvironment.AZURE` 值會對 Azure 公用雲端建立認證。</span><span class="sxs-lookup"><span data-stu-id="d24ec-120">The `AzureEnvironment.AZURE` value creates credentials against the Azure public cloud.</span></span> <span data-ttu-id="d24ec-121">如果您需要存取其他雲端，請將此值變更為不同的值 (例如，`AzureEnvironment.AZURE_GERMANY`)。</span><span class="sxs-lookup"><span data-stu-id="d24ec-121">Change this to a different value if you need to access another cloud (for example, `AzureEnvironment.AZURE_GERMANY`).</span></span>  

 <span data-ttu-id="d24ec-122">從環境變數或祕密管理存放區 (如 [Key Vault](/azure/key-vault/key-vault-whatis)) 讀取服務主體值。</span><span class="sxs-lookup"><span data-stu-id="d24ec-122">Read the service principal values from environment variables or a secret management store like [Key Vault](/azure/key-vault/key-vault-whatis).</span></span> <span data-ttu-id="d24ec-123">請避免在程式碼中將這些值設定為純文字字串，以免不小心地在版本控制記錄中公開認證。</span><span class="sxs-lookup"><span data-stu-id="d24ec-123">Avoid setting these values as cleartext strings in your code to prevent accidentally exposing credentials in your version control history.</span></span>   

<a name="mgmt-file"></a>

### <a name="file-based-authentication-preview"></a><span data-ttu-id="d24ec-124">檔案型驗證 (預覽)</span><span class="sxs-lookup"><span data-stu-id="d24ec-124">File based authentication (Preview)</span></span>

<span data-ttu-id="d24ec-125">最簡單的驗證方式是建立屬性檔，在其中包含適用於 [Azure 服務主體](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)且使用下列格式的認證：</span><span class="sxs-lookup"><span data-stu-id="d24ec-125">The simplest way to authenticate is to create a properties file that contains credentials for an [Azure service principal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) using the following format:</span></span>

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

- <span data-ttu-id="d24ec-126">subscription：使用在 Azure CLI 2.0 中透過 `az account show` 所得到的 id 值。</span><span class="sxs-lookup"><span data-stu-id="d24ec-126">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="d24ec-127">client：使用從建立來執行應用程式之服務主體所擷取之輸出中得到的 appId 值。</span><span class="sxs-lookup"><span data-stu-id="d24ec-127">client: use the *appId* value from the output taken from a service principal created to run the application.</span></span> <span data-ttu-id="d24ec-128">如果您沒有適用於應用程式的服務主體，請[使用 Azure CLI 2.0 建立服務主體](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="d24ec-128">If you don't have a service principal for your app, [create one with the Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>
- <span data-ttu-id="d24ec-129">key：使用從服務主體建立 CLI 輸出所得到的 password 值</span><span class="sxs-lookup"><span data-stu-id="d24ec-129">key: use the *password* value from the service principal create CLI output</span></span> 
- <span data-ttu-id="d24ec-130">tenant：使用從服務主體建立 CLI 輸出所得到的 tenant 值</span><span class="sxs-lookup"><span data-stu-id="d24ec-130">tenant: use the *tenant* value from the service principal create CLI output</span></span>

<span data-ttu-id="d24ec-131">將此檔案儲存在系統上可供程式碼讀取且安全的位置。</span><span class="sxs-lookup"><span data-stu-id="d24ec-131">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="d24ec-132">在殼層中使用該檔案的完整路徑來設定環境變數：</span><span class="sxs-lookup"><span data-stu-id="d24ec-132">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="d24ec-133">建立進入點 `Azure` 物件來開始使用程式庫。</span><span class="sxs-lookup"><span data-stu-id="d24ec-133">Create the entry point `Azure` object to start working with the libraries.</span></span> <span data-ttu-id="d24ec-134">透過環境變數讀取屬性檔的位置。</span><span class="sxs-lookup"><span data-stu-id="d24ec-134">Read the location of the properties file through the environment variable.</span></span>

```java
// pull in the location of the authenticaiton properties file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```



