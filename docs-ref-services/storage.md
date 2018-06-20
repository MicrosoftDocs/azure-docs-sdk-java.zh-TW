---
title: 適用於 Java 的 Azure 儲存體程式庫
description: ''
keywords: Azure, Java, SDK, API, 儲存體
author: douge
ms.author: douge
manager: douge
ms.date: 02/07/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: ec06e79374176b5a4795d27c5fbbb2260e65cd8c
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823651"
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="e7fb7-103">適用於 Java 的 Azure 儲存體程式庫</span><span class="sxs-lookup"><span data-stu-id="e7fb7-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="e7fb7-104">概觀</span><span class="sxs-lookup"><span data-stu-id="e7fb7-104">Overview</span></span>

<span data-ttu-id="e7fb7-105">使用 [Azure 儲存體](/azure/storage/storage-introduction)從 Java 應用程式讀取和寫入檔案、blob (物件) 資料、機碼值組以及訊息。</span><span class="sxs-lookup"><span data-stu-id="e7fb7-105">Read and write files, blob (object) data, key-value pairs, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="e7fb7-106">若要開始使用 Azure 儲存體，請參閱[如何使用 Java 的 Blob 儲存體](/azure/storage/storage-java-how-to-use-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="e7fb7-106">To get started with Azure Storage, see [How to use Blob storage from Java](/azure/storage/storage-java-how-to-use-blob-storage).</span></span>

## <a name="client-library"></a><span data-ttu-id="e7fb7-107">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="e7fb7-107">Client library</span></span>

<span data-ttu-id="e7fb7-108">使用[連接字串](/azure/storage/storage-create-storage-account#manage-your-storage-account)來連線到 Azure 儲存體帳戶，然後使用用戶端程式庫的類別和方法來處理 blob、資料表、檔案或佇列儲存體。</span><span class="sxs-lookup"><span data-stu-id="e7fb7-108">Use [connection strings](/azure/storage/storage-create-storage-account#manage-your-storage-account) to connect to an Azure Storage account, then use the client libraries' classes and methods to work with blob, table, file, or queue storage.</span></span> 

<span data-ttu-id="e7fb7-109">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="e7fb7-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>7.0.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="e7fb7-110">範例</span><span class="sxs-lookup"><span data-stu-id="e7fb7-110">Example</span></span>

<span data-ttu-id="e7fb7-111">將映像檔從本機檔案系統寫入到現有 Azure 儲存體 blob 容器中的新 blob 中。</span><span class="sxs-lookup"><span data-stu-id="e7fb7-111">Write a image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


```java
String storageConnectionString = "DefaultEndpointsProtocol=https;" + 
"AccountName=fabrikamblobstorage;" + 
"AccountKey=keyvalue;EndpointSuffix=core.windows.net";

CloudStorageAccount account = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient serviceClient = account.createCloudBlobClient();
CloudBlobContainer container = serviceClient.getContainerReference(blobContainer);

// write a blob from a local filesystem path to the container as logo.png
CloudBlockBlob blob = container.getBlockBlobReference("logo.png");
blob.uploadFromFile("/Users/raisa/fabrikam.png");
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e7fb7-112">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="e7fb7-112">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="e7fb7-113">管理 API</span><span class="sxs-lookup"><span data-stu-id="e7fb7-113">Management API</span></span>

<span data-ttu-id="e7fb7-114">使用管理 API 建立及管理 Azure 儲存體帳戶和連線金鑰。</span><span class="sxs-lookup"><span data-stu-id="e7fb7-114">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="e7fb7-115">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="e7fb7-115">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="e7fb7-116">範例</span><span class="sxs-lookup"><span data-stu-id="e7fb7-116">Example</span></span>

<span data-ttu-id="e7fb7-117">在訂用帳戶中建立新的 Azure 儲存體帳戶並擷取其存取金鑰。</span><span class="sxs-lookup"><span data-stu-id="e7fb7-117">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

```java
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
        .withRegion(Region.US_EAST)
        .withNewResourceGroup(rgName)
        .create();

// get a list of storage account keys related to the account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e7fb7-118">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="e7fb7-118">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="e7fb7-119">範例</span><span class="sxs-lookup"><span data-stu-id="e7fb7-119">Samples</span></span>

<span data-ttu-id="e7fb7-120">[管理 Azure 儲存體帳戶](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)  </span><span class="sxs-lookup"><span data-stu-id="e7fb7-120">[Manage Azure Storage accounts](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)  </span></span>  
<span data-ttu-id="e7fb7-121">[在 blob 儲存體中讀取和寫入物件](https://github.com/Azure-Samples/storage-blob-java-getting-started) </span><span class="sxs-lookup"><span data-stu-id="e7fb7-121">[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blob-java-getting-started) </span></span>  
<span data-ttu-id="e7fb7-122">[使用佇列讀取和寫入訊息](https://github.com/Azure-Samples/storage-queue-java-getting-started) </span><span class="sxs-lookup"><span data-stu-id="e7fb7-122">[Read and write messages with queues](https://github.com/Azure-Samples/storage-queue-java-getting-started) </span></span>  
[<span data-ttu-id="e7fb7-123">從 Web 應用程式中的 blob 儲存體讀取檔案</span><span class="sxs-lookup"><span data-stu-id="e7fb7-123">Read files from blob storage in a web app</span></span>](https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps-on-linux)

<span data-ttu-id="e7fb7-124">深入探索可在應用程式中使用的 [Azure 儲存體 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=storage)。</span><span class="sxs-lookup"><span data-stu-id="e7fb7-124">Explore more [sample Java code for Azure Storage](https://azure.microsoft.com/resources/samples/?platform=java&term=storage) you can use in your apps.</span></span>
