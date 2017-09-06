---
title: "適用於 Java 的 Azure 程式庫"
description: "適用於 Java 的 Azure 管理和服務程式庫概觀"
keywords: Azure, Java, SDK, API
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 9aaf22a2-382a-4b13-a8e3-0e467d607207
ms.openlocfilehash: 22a337e928085475e905a271f991147729d97671
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="azure-libraries-for-java"></a>適用於 Java 的 Azure 程式庫

適用於 Java 的 Azure 程式庫可協助您管理 Azure 資源，並從應用程式程式碼連線至服務。 您可以將程式庫作為 [Maven 匯入](java-sdk-azure-install.md)以在 Java 專案中使用。 

## <a name="manage-azure-resources"></a>管理 Azure 資源

使用[適用於 Java 的 Azure 管理程式庫](java-sdk-azure-get-started.md)以從 Java 應用程式建立和管理 Azure 資源。 使用這些程式庫可建置您自己的 Azure 自動化工具和服務。 

例如，若要建立 Linux VM，您需要撰寫下列程式碼：

```java
VirtualMachine linuxVM = azure.virtualMachines().define("myAzureVM")
           .withRegion(region)
           .withExistingResourceGroup("sampleResourceGroup")
           .withNewPrimaryNetwork("10.0.0.0/28")
           .withPrimaryPrivateIpAddressDynamic()
           .withoutPrimaryPublicIpAddress()
           .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
           .withRootUsername(userName)
           .withSsh(key)
           .withUnmanagedStorage()
           .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
           .create();
 ```

請檢閱[安裝指示](java-sdk-azure-install.md)以取得完整的程式庫清單，以及了解如何將程式庫匯入到專案中，然後閱讀[開始使用文章](java-sdk-azure-get-started.md)，以針對您自己的 Azure 訂用帳戶設定驗證和執行程式碼範例。 

## <a name="connect-to-azure-services"></a>連線到 Azure 服務

除了使用 Java 程式庫以在 Azure 中建立和管理資源外，您也可以使用 Java 程式庫來連線到應用程式中的資源並加以使用。 例如，您可以更新資料表 SQL Database，或在 Azure 儲存體中儲存檔案。 從[完整程式庫清單](java-sdk-azure-install.md)選取特定服務所需的程式庫，然後瀏覽 [Java 開發人員中心](https://azure.microsoft.com/develop/java/)以取得可協助您在應用程式中使用程式庫的教學課程和程式碼範例。

例如，若要在 Azure 儲存體容器中列印出每個 blob 的內容：

```java
// get the container from the blob client
CloudBlobContainer container = blobClient.getContainerReference("blobcontainer");

// For each item in the container
for (ListBlobItem blobItem : container.listBlobs()) {
    // If the item is a blob, not a virtual directory
    if (blobItem instanceof CloudBlockBlob) {
        // Download the text
        CloudBlockBlob retrievedBlob = (CloudBlockBlob) blobItem;
        System.out.println(retrievedBlob.downloadText());
    }
}
```

## <a name="sample-code-and-reference"></a>程式碼範例和參考

下列範例涵蓋了可使用適用於 Java 的 Azure 管理程式庫來執行的常見自動化工作，而且這些範例中已備有程式碼可供您自己的應用程式使用：

- [虛擬機器](java-sdk-azure-virtual-machine-samples.md)
- [Web Apps](java-sdk-azure-web-apps-samples.md)
- [SQL Database](java-sdk-azure-sql-database-samples.md)
   
我們提供了一個適用於服務和管理程式庫中所有套件的[參考](https://docs.microsoft.com/java/api)。 新功能、重大變更以及從先前版本移轉的指示則會在[版本資訊](java-sdk-azure-release-notes.md)中提供。