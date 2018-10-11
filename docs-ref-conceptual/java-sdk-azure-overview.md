---
title: 適用於 Java 的 Azure 程式庫
description: 適用於 Java 的 Azure 管理和服務程式庫概觀
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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892729"
---
# <a name="azure-libraries-for-java"></a><span data-ttu-id="fef9d-104">適用於 Java 的 Azure 程式庫</span><span class="sxs-lookup"><span data-stu-id="fef9d-104">Azure libraries for Java</span></span>

<span data-ttu-id="fef9d-105">適用於 Java 的 Azure 程式庫可協助您管理 Azure 資源，並從應用程式程式碼連線至服務。</span><span class="sxs-lookup"><span data-stu-id="fef9d-105">The Azure libraries for Java help you manage Azure resources and connect to services from your application code.</span></span> <span data-ttu-id="fef9d-106">您可以將程式庫作為 [Maven 匯入](java-sdk-azure-install.md)以在 Java 專案中使用。</span><span class="sxs-lookup"><span data-stu-id="fef9d-106">The libraries are available as [Maven imports](java-sdk-azure-install.md) for use in your Java projects.</span></span> 

## <a name="manage-azure-resources"></a><span data-ttu-id="fef9d-107">管理 Azure 資源</span><span class="sxs-lookup"><span data-stu-id="fef9d-107">Manage Azure resources</span></span>

<span data-ttu-id="fef9d-108">使用[適用於 Java 的 Azure 管理程式庫](java-sdk-azure-get-started.md)以從 Java 應用程式建立和管理 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="fef9d-108">Create and manage Azure resources from your Java applications using the [Azure management libraries for Java](java-sdk-azure-get-started.md).</span></span> <span data-ttu-id="fef9d-109">使用這些程式庫可建置您自己的 Azure 自動化工具和服務。</span><span class="sxs-lookup"><span data-stu-id="fef9d-109">Use these libraries to build your own Azure automation tools and services.</span></span> 

<span data-ttu-id="fef9d-110">例如，若要建立 Linux VM，您需要撰寫下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="fef9d-110">For example, to create a Linux VM you would write the following code:</span></span>

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

<span data-ttu-id="fef9d-111">請檢閱[安裝指示](java-sdk-azure-install.md)以取得完整的程式庫清單，以及了解如何將程式庫匯入到專案中，然後閱讀[開始使用文章](java-sdk-azure-get-started.md)，以針對您自己的 Azure 訂用帳戶設定驗證和執行程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="fef9d-111">Review the [install instructions](java-sdk-azure-install.md) for a full list of the libraries and how to import them into your projects and then read the [get started article](java-sdk-azure-get-started.md) to set up your authentication and run sample code against your own Azure subscription.</span></span> 

## <a name="connect-to-azure-services"></a><span data-ttu-id="fef9d-112">連線到 Azure 服務</span><span class="sxs-lookup"><span data-stu-id="fef9d-112">Connect to Azure services</span></span>

<span data-ttu-id="fef9d-113">除了使用 Java 程式庫以在 Azure 中建立和管理資源外，您也可以使用 Java 程式庫來連線到應用程式中的資源並加以使用。</span><span class="sxs-lookup"><span data-stu-id="fef9d-113">In addition to using Java libraries to create and manage resources within Azure, you can also use Java libraries to connect  and use those resources in your apps.</span></span> <span data-ttu-id="fef9d-114">例如，您可以更新資料表 SQL Database，或在 Azure 儲存體中儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="fef9d-114">For example, you might update a table SQL Database or store files in Azure Storage.</span></span> <span data-ttu-id="fef9d-115">從[完整程式庫清單](java-sdk-azure-install.md)選取特定服務所需的程式庫，然後瀏覽 [Java 開發人員中心](https://azure.microsoft.com/develop/java/)以取得可協助您在應用程式中使用程式庫的教學課程和程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="fef9d-115">Select the library you need for a particular service from the [complete list of libraries](java-sdk-azure-install.md) and visit the [Java developer center](https://azure.microsoft.com/develop/java/) for tutorials and sample code for help using them in your apps.</span></span>

<span data-ttu-id="fef9d-116">例如，若要在 Azure 儲存體容器中列印出每個 blob 的內容：</span><span class="sxs-lookup"><span data-stu-id="fef9d-116">For example, to print out the contents of every blob in an Azure storage container:</span></span>

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

## <a name="sample-code-and-reference"></a><span data-ttu-id="fef9d-117">程式碼範例和參考</span><span class="sxs-lookup"><span data-stu-id="fef9d-117">Sample code and reference</span></span>

<span data-ttu-id="fef9d-118">下列範例涵蓋了可使用適用於 Java 的 Azure 管理程式庫來執行的常見自動化工作，而且這些範例中已備有程式碼可供您自己的應用程式使用：</span><span class="sxs-lookup"><span data-stu-id="fef9d-118">The following samples cover common automation tasks with the Azure management libraries for Java and have code ready to use in your own apps:</span></span>

- [<span data-ttu-id="fef9d-119">虛擬機器</span><span class="sxs-lookup"><span data-stu-id="fef9d-119">Virtual machines</span></span>](java-sdk-azure-virtual-machine-samples.md)
- [<span data-ttu-id="fef9d-120">Web Apps</span><span class="sxs-lookup"><span data-stu-id="fef9d-120">Web apps</span></span>](java-sdk-azure-web-apps-samples.md)
- [<span data-ttu-id="fef9d-121">SQL Database</span><span class="sxs-lookup"><span data-stu-id="fef9d-121">SQL Database</span></span>](java-sdk-azure-sql-database-samples.md)
   
<span data-ttu-id="fef9d-122">我們提供了一個適用於服務和管理程式庫中所有套件的[參考](https://docs.microsoft.com/java/api)。</span><span class="sxs-lookup"><span data-stu-id="fef9d-122">A [reference](https://docs.microsoft.com/java/api) is available for all packages in both the service and management libraries.</span></span> <span data-ttu-id="fef9d-123">新功能、重大變更以及從先前版本移轉的指示則會在[版本資訊](java-sdk-azure-release-notes.md)中提供。</span><span class="sxs-lookup"><span data-stu-id="fef9d-123">New features, breaking changes, and migration instructions from previous versions are available in the [release notes](java-sdk-azure-release-notes.md).</span></span>