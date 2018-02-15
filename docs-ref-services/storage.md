---
title: "適用於 Java 的 Azure 儲存體程式庫"
description: 
keywords: "Azure, Java, SDK, API, 儲存體"
author: douge
ms.author: douge
manager: douge
ms.date: 02/07/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: 3c7a3a1fcf2e97202e7f38f8df5acb6637fb4b47
ms.sourcegitcommit: 2ae0d99c02f4ad7efa9e3d3fbd1db7e9de20c6e7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2018
---
# <a name="azure-storage-libraries-for-java"></a>適用於 Java 的 Azure 儲存體程式庫

## <a name="overview"></a>概觀

使用 [Azure 儲存體](/azure/storage/storage-introduction)從 Java 應用程式讀取和寫入檔案、blob (物件) 資料、機碼值組以及訊息。

若要開始使用 Azure 儲存體，請參閱[如何使用 Java 的 Blob 儲存體](/azure/storage/storage-java-how-to-use-blob-storage)。

## <a name="client-library"></a>用戶端程式庫

使用[連接字串](/azure/storage/storage-create-storage-account#manage-your-storage-account)來連線到 Azure 儲存體帳戶，然後使用用戶端程式庫的類別和方法來處理 blob、資料表、檔案或佇列儲存體。 

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>7.0.0</version>
</dependency>
```   

### <a name="example"></a>範例

將映像檔從本機檔案系統寫入到現有 Azure 儲存體 blob 容器中的新 blob 中。


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
> [探索用戶端 API](/java/api/overview/azure/storage/clientlibrary)

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
> [探索管理 API](/java/api/overview/azure/storage/managementapi)


## <a name="samples"></a>範例

[管理 Azure 儲存體帳戶](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)    
[在 blob 儲存體中讀取和寫入物件](https://github.com/Azure-Samples/storage-blob-java-getting-started)   
[使用佇列讀取和寫入訊息](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
[從 Web 應用程式中的 blob 儲存體讀取檔案](https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps-on-linux)

深入探索可在應用程式中使用的 [Azure 儲存體 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=storage)。
