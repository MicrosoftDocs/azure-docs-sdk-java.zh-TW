---
title: 適用於 Java 的 Azure Batch 程式庫
description: Java Batch 程式庫的參考文件
keywords: Azure, Java, SDK, API, Batch, 處理, 排程, 長時間執行
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: batch
ms.openlocfilehash: 67381d68d23f98579a472aefbebaa929af622b8d
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823591"
---
# <a name="azure-batch-libraries-for-java"></a><span data-ttu-id="a47a7-104">適用於 Java 的 Azure Batch 程式庫</span><span class="sxs-lookup"><span data-stu-id="a47a7-104">Azure Batch libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="a47a7-105">概觀</span><span class="sxs-lookup"><span data-stu-id="a47a7-105">Overview</span></span>

<span data-ttu-id="a47a7-106">使用 [Azure Batch](/azure/batch/batch-technical-overview) 在雲端有效地執行大規模的平行和高效能計算應用程式。</span><span class="sxs-lookup"><span data-stu-id="a47a7-106">Run large-scale parallel and high-performance computing applications efficiently in the cloud with [Azure Batch](/azure/batch/batch-technical-overview).</span></span>   

<span data-ttu-id="a47a7-107">若要開始使用 Azure Batch，請參閱[使用 Azure 入口網站建立 Batch 帳戶](/azure/batch/batch-account-create-portal)。</span><span class="sxs-lookup"><span data-stu-id="a47a7-107">To get started with Azure Batch, see [Create a Batch account with the Azure portal](/azure/batch/batch-account-create-portal).</span></span>

## <a name="client-library"></a><span data-ttu-id="a47a7-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="a47a7-108">Client library</span></span>

<span data-ttu-id="a47a7-109">Azure Batch 用戶端程式庫可讓您設定計算節點和集區、定義工作並將其設定為在作業中執行，以及設定作業管理員以控制和監控作業的執行。</span><span class="sxs-lookup"><span data-stu-id="a47a7-109">The Azure Batch client libraries let you configure compute nodes and pools, define tasks and configure them to run in jobs, and set up a job manager to control and monitor job execution.</span></span> <span data-ttu-id="a47a7-110">[深入了解](/azure/batch/batch-api-basics)如何使用這些物件以執行大規模的平行計算解決方案。</span><span class="sxs-lookup"><span data-stu-id="a47a7-110">[Learn more](/azure/batch/batch-api-basics) about using these objects to run large-scale parallel compute solutions.</span></span>

<span data-ttu-id="a47a7-111">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="a47a7-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="a47a7-112">範例</span><span class="sxs-lookup"><span data-stu-id="a47a7-112">Example</span></span>

<span data-ttu-id="a47a7-113">在 Batch 帳戶中設定 Linux 計算節點集區：</span><span class="sxs-lookup"><span data-stu-id="a47a7-113">Set up a pool of Linux compute nodes in a batch account:</span></span>

```java
// create the batch client for an account using its URI and keys
BatchClient client = BatchClient.open(new BatchSharedKeyCredentials("https://fabrikambatch.eastus.batch.azure.com", "fabrikambatch", batchKey));

// configure a pool of VMs to use 
VirtualMachineConfiguration configuration = new VirtualMachineConfiguration();
configuration.withNodeAgentSKUId("batch.node.ubuntu 16.04");
client.poolOperations().createPool(poolId, poolVMSize, configuration, poolVMCount);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="a47a7-114">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="a47a7-114">Explore the Client APIs</span></span>](/java/api/overview/azure/batch/client)


## <a name="management-api"></a><span data-ttu-id="a47a7-115">管理 API</span><span class="sxs-lookup"><span data-stu-id="a47a7-115">Management API</span></span>

<span data-ttu-id="a47a7-116">使用 Azure Batch 管理程式庫來建立和刪除 Batch 帳戶、讀取和重新產生 Batch 帳戶金鑰，以及管理 Batch 帳戶的儲存體。</span><span class="sxs-lookup"><span data-stu-id="a47a7-116">Use the Azure Batch management libraries to create and delete batch accounts, read and regenerate batch account keys, and manage batch account storage.</span></span>

<span data-ttu-id="a47a7-117">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="a47a7-117">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-batch</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="a47a7-118">範例</span><span class="sxs-lookup"><span data-stu-id="a47a7-118">Example</span></span>

<span data-ttu-id="a47a7-119">建立 Azure Batch 帳戶，並為其設定新的應用程式和 Azure 儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="a47a7-119">Create an Azure Batch account and configure a new application and Azure storage account for it.</span></span>

```java
BatchAccount batchAccount = azure.batchAccounts().define("newBatchAcct")
    .withRegion(Region.US_EAST)
    .withNewResourceGroup("myResourceGroup")
    .defineNewApplication("batchAppName")
        .defineNewApplicationPackage(applicationPackageName)
        .withAllowUpdates(true)
        .withDisplayName(applicationDisplayName)
        .attach()
    .withNewStorageAccount("batchStorageAcct")
    .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="a47a7-120">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="a47a7-120">Explore the Management APIs</span></span>](/java/api/overview/azure/batch/management)


## <a name="samples"></a><span data-ttu-id="a47a7-121">範例</span><span class="sxs-lookup"><span data-stu-id="a47a7-121">Samples</span></span>

<span data-ttu-id="a47a7-122">[管理 Batch 帳戶][1]</span><span class="sxs-lookup"><span data-stu-id="a47a7-122">[Manage Batch accounts][1]</span></span>   

<span data-ttu-id="a47a7-123">深入探索可在應用程式中使用的 [Azure Batch Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=batch)。</span><span class="sxs-lookup"><span data-stu-id="a47a7-123">Explore more [sample Java code for Azure Batch](https://azure.microsoft.com/resources/samples/?platform=java&term=batch) you can use in your apps.</span></span>

[1]: https://github.com/Azure-Samples/batch-java-manage-batch-accounts