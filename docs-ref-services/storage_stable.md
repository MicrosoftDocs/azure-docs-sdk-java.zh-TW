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
ms.locfileid: "50235219"
---
# <a name="azure-storage-libraries-for-java"></a>適用於 Java 的 Azure 儲存體程式庫

## <a name="overview"></a>概觀

使用 [Azure 儲存體](/azure/storage/storage-introduction)從 Java 應用程式讀取和寫入 blob (物件) 資料、檔案以及訊息。

若要開始使用 Azure 儲存體，請參閱[如何使用 Java 的 Blob 儲存體](/azure/storage/blobs/storage-quickstart-blobs-java-v10)。

## <a name="client-library"></a>用戶端程式庫

使用共用金鑰、SAS 權杖或來自 Azure Active Directory 的 OAuth 權杖來授權使用 Azure 儲存體服務。 然後使用用戶端程式庫的類別和方法來使用 blob、檔案或佇列儲存體。 

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。   

**Blob 服務的相依性**：
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>10.1.0</version>
</dependency>
```

**佇列服務的相依性**：
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-queue</artifactId>
    <version>10.0.0-Preview</version>
</dependency>
```


### <a name="example"></a>範例

將映像檔從本機檔案系統寫入到現有 Azure 儲存體 blob 容器的新 blob 中。


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
> [探索用戶端 API](/java/api/overview/azure/storage/client)

## <a name="management-api"></a>管理 API

使用管理 API 建立及管理 Azure 儲存體帳戶和連線金鑰。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a>範例

在訂用帳戶中建立新的 Azure 儲存體帳戶並擷取其存取金鑰。

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
> [探索管理 API](/java/api/overview/azure/storage/management)


## <a name="samples"></a>範例

[適用於 Java 的 Azure 儲存體 SDK](https://github.com/azure/azure-storage-java)
[讀取物件並寫入 blob 儲存體](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart)   
[使用佇列讀取及寫入訊息](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
