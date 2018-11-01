---
title: 適用於 Java 的 Azure 儲存體程式庫
description: ''
keywords: Azure, Java, SDK, API, 儲存體
author: douge
ms.author: douge
manager: douge
ms.date: 10/19/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: fba48dfa04f223dce72a0ee54da967565ebd3687
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235237"
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="6bade-103">適用於 Java 的 Azure 儲存體程式庫</span><span class="sxs-lookup"><span data-stu-id="6bade-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="6bade-104">概觀</span><span class="sxs-lookup"><span data-stu-id="6bade-104">Overview</span></span>

<span data-ttu-id="6bade-105">使用 [Azure 儲存體](/azure/storage/storage-introduction)從 Java 應用程式讀取和寫入 blob (物件) 資料、檔案以及訊息。</span><span class="sxs-lookup"><span data-stu-id="6bade-105">Read and write blob (object) data, files, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="6bade-106">若要開始使用 Azure 儲存體，請參閱[如何使用 Java 的 Blob 儲存體](/azure/storage/blobs/storage-quickstart-blobs-java-v10)。</span><span class="sxs-lookup"><span data-stu-id="6bade-106">To get started with Azure Storage, see [How to use Blob storage from Java](/azure/storage/blobs/storage-quickstart-blobs-java-v10).</span></span>

## <a name="client-library"></a><span data-ttu-id="6bade-107">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="6bade-107">Client library</span></span>

<span data-ttu-id="6bade-108">使用共用金鑰、SAS 權杖或來自 Azure Active Directory 的 OAuth 權杖來授權使用 Azure 儲存體服務。</span><span class="sxs-lookup"><span data-stu-id="6bade-108">Use a Shared Key, SAS token or an OAuth token from the Azure Active Directory to authorize with Azure Storage services.</span></span> <span data-ttu-id="6bade-109">然後使用用戶端程式庫的類別和方法來使用 blob、檔案或佇列儲存體。</span><span class="sxs-lookup"><span data-stu-id="6bade-109">Then use the client libraries' classes and methods to work with blob, file, or queue storage.</span></span> 

<span data-ttu-id="6bade-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="6bade-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

<span data-ttu-id="6bade-111">**Blob 服務的相依性**：</span><span class="sxs-lookup"><span data-stu-id="6bade-111">**Dependency for Blob service**:</span></span>
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>10.1.0</version>
</dependency>
```

<span data-ttu-id="6bade-112">**佇列服務的相依性**：</span><span class="sxs-lookup"><span data-stu-id="6bade-112">**Dependency for Queue service**:</span></span>
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-queue</artifactId>
    <version>10.0.0-Preview</version>
</dependency>
```


### <a name="example"></a><span data-ttu-id="6bade-113">範例</span><span class="sxs-lookup"><span data-stu-id="6bade-113">Example</span></span>

<span data-ttu-id="6bade-114">將映像檔從本機檔案系統寫入到現有 Azure 儲存體 blob 容器的新 blob 中。</span><span class="sxs-lookup"><span data-stu-id="6bade-114">Write an image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


```java
// Retrieve the credentials and initialize SharedKeyCredentials
String accountName = System.getenv("AZURE_STORAGE_ACCOUNT");
String accountKey = System.getenv("AZURE_STORAGE_ACCESS_KEY");

// Create a BlockBlobURL to run operations on Block Blobs. Alternatively create a ServiceURL, or ContainerURL for operations on Blob service, and Blob containers
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);

// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final BlockBlobURL blobURL = new BlockBlobURL(
    new URL("https://" + accountName + ".blob.core.windows.net/mycontainer/myimage.jpg"), 
        StorageURL.createPipeline(creds, new PipelineOptions())
);

AsynchronousFileChannel fileChannel = AsynchronousFileChannel.open(Paths.get("myimage.jpg"));
TransferManager.uploadFileToBlockBlob(fileChannel, blobURL,0, null).blockingGet();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="6bade-115">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="6bade-115">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="6bade-116">管理 API</span><span class="sxs-lookup"><span data-stu-id="6bade-116">Management API</span></span>

<span data-ttu-id="6bade-117">使用管理 API 建立及管理 Azure 儲存體帳戶和連線金鑰。</span><span class="sxs-lookup"><span data-stu-id="6bade-117">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="6bade-118">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="6bade-118">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="6bade-119">範例</span><span class="sxs-lookup"><span data-stu-id="6bade-119">Example</span></span>

<span data-ttu-id="6bade-120">在訂用帳戶中建立新的 Azure 儲存體帳戶並擷取其存取金鑰。</span><span class="sxs-lookup"><span data-stu-id="6bade-120">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

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
> [<span data-ttu-id="6bade-121">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="6bade-121">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="6bade-122">範例</span><span class="sxs-lookup"><span data-stu-id="6bade-122">Samples</span></span>

<span data-ttu-id="6bade-123">[適用於 Java 的 Azure 儲存體 SDK](https://github.com/azure/azure-storage-java)
[讀取物件並寫入 blob 儲存體](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span><span class="sxs-lookup"><span data-stu-id="6bade-123">[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span></span>  
[<span data-ttu-id="6bade-124">使用佇列讀取及寫入訊息</span><span class="sxs-lookup"><span data-stu-id="6bade-124">Read and write messages with queues</span></span>](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
