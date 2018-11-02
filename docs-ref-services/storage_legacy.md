---
title: 適用於 Java 的 Azure 儲存體程式庫
description: ''
keywords: Azure, Java, SDK, API, 儲存體
author: douge
ms.author: seguler
manager: dineshm
ms.date: 10/29/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: 68a5fdbf8e2fbfe83f9ea09112d64488e0c11d08
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235231"
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="0354d-103">適用於 Java 的 Azure 儲存體程式庫</span><span class="sxs-lookup"><span data-stu-id="0354d-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="0354d-104">概觀</span><span class="sxs-lookup"><span data-stu-id="0354d-104">Overview</span></span>

<span data-ttu-id="0354d-105">使用 [Azure 儲存體](/azure/storage/storage-introduction)從 Java 應用程式讀取和寫入 blob (物件) 資料、檔案以及訊息。</span><span class="sxs-lookup"><span data-stu-id="0354d-105">Read and write blob (object) data, files, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="0354d-106">若要開始使用 Azure 儲存體，請參閱[如何使用 Java SDK v7 的 Blob 儲存體](/azure/storage/blobs/storage-quickstart-blobs-java)。</span><span class="sxs-lookup"><span data-stu-id="0354d-106">To get started with Azure Storage, see [How to use Blob storage from Java SDK v7](/azure/storage/blobs/storage-quickstart-blobs-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="0354d-107">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="0354d-107">Client library</span></span>

<span data-ttu-id="0354d-108">使用共用金鑰、SAS 權杖或來自 Azure Active Directory 的 OAuth 權杖來授權使用 Azure 儲存體服務。</span><span class="sxs-lookup"><span data-stu-id="0354d-108">Use a Shared Key, SAS token or an OAuth token from the Azure Active Directory to authorize with Azure Storage services.</span></span> <span data-ttu-id="0354d-109">然後使用用戶端程式庫的類別和方法來使用 blob、檔案或佇列儲存體。</span><span class="sxs-lookup"><span data-stu-id="0354d-109">Then use the client libraries' classes and methods to work with blob, file, or queue storage.</span></span> 

<span data-ttu-id="0354d-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="0354d-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

<span data-ttu-id="0354d-111">**舊版 Azure 儲存體 SDK v7 的相依性**：</span><span class="sxs-lookup"><span data-stu-id="0354d-111">**Dependency for the legacy Azure Storage SDK v7**:</span></span>
```XML
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>8.0.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="0354d-112">範例</span><span class="sxs-lookup"><span data-stu-id="0354d-112">Example</span></span>

<span data-ttu-id="0354d-113">將映像檔從本機檔案系統寫入到現有 Azure 儲存體 blob 容器的新 blob 中。</span><span class="sxs-lookup"><span data-stu-id="0354d-113">Write an image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


```java
// Parse the connection string and create a blob client to interact with Blob storage
CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient blobClient = storageAccount.createCloudBlobClient();
CloudBlobContainer container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());         
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="0354d-114">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="0354d-114">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="0354d-115">管理 API</span><span class="sxs-lookup"><span data-stu-id="0354d-115">Management API</span></span>

<span data-ttu-id="0354d-116">使用管理 API 建立及管理 Azure 儲存體帳戶和連線金鑰。</span><span class="sxs-lookup"><span data-stu-id="0354d-116">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="0354d-117">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="0354d-117">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="0354d-118">範例</span><span class="sxs-lookup"><span data-stu-id="0354d-118">Example</span></span>

<span data-ttu-id="0354d-119">在訂用帳戶中建立新的 Azure 儲存體帳戶並擷取其存取金鑰。</span><span class="sxs-lookup"><span data-stu-id="0354d-119">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

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
> [<span data-ttu-id="0354d-120">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="0354d-120">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="0354d-121">範例</span><span class="sxs-lookup"><span data-stu-id="0354d-121">Samples</span></span>

<span data-ttu-id="0354d-122">[適用於 Java 的 Azure 儲存體 SDK](https://github.com/azure/azure-storage-java)
[讀取物件並寫入 blob 儲存體](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span><span class="sxs-lookup"><span data-stu-id="0354d-122">[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span></span>  
[<span data-ttu-id="0354d-123">使用佇列讀取及寫入訊息</span><span class="sxs-lookup"><span data-stu-id="0354d-123">Read and write messages with queues</span></span>](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
