---
title: "適用於 Java 的 Azure Batch 程式庫"
description: "Java Batch 程式庫的參考文件"
keywords: "Azure, Java, SDK, API, Batch, 處理, 排程, 長時間執行"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: batch
ms.openlocfilehash: ebf86e358ff541632e2d1f9503ebae593bdbdb3c
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2017
---
# <a name="azure-batch-libraries-for-java"></a>適用於 Java 的 Azure Batch 程式庫

## <a name="overview"></a>概觀

使用 [Azure Batch](/azure/batch/batch-technical-overview) 在雲端有效地執行大規模的平行和高效能計算應用程式。   

若要開始使用 Azure Batch，請參閱[使用 Azure 入口網站建立 Batch 帳戶](/azure/batch/batch-account-create-portal)。

## <a name="client-library"></a>用戶端程式庫

Azure Batch 用戶端程式庫可讓您設定計算節點和集區、定義工作並將其設定為在作業中執行，以及設定作業管理員以控制和監控作業的執行。 [深入了解](/azure/batch/batch-api-basics)如何使用這些物件以執行大規模的平行計算解決方案。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```   

### <a name="example"></a>範例

在 Batch 帳戶中設定 Linux 計算節點集區：

```java
// create the batch client for an account using its URI and keys
BatchClient client = BatchClient.open(new BatchSharedKeyCredentials("https://fabrikambatch.eastus.batch.azure.com", "fabrikambatch", batchKey));

// configure a pool of VMs to use 
VirtualMachineConfiguration configuration = new VirtualMachineConfiguration();
configuration.withNodeAgentSKUId("batch.node.ubuntu 16.04");
client.poolOperations().createPool(poolId, poolVMSize, configuration, poolVMCount);
```

> [!div class="nextstepaction"]
> [探索用戶端 API](/java/api/overview/azure/batch/clientlibrary)


## <a name="management-api"></a>管理 API

使用 Azure Batch 管理程式庫來建立和刪除 Batch 帳戶、讀取和重新產生 Batch 帳戶金鑰，以及管理 Batch 帳戶的儲存體。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-batch</artifactId>
    <version>1.2.1</version>
</dependency>
```

### <a name="example"></a>範例

建立 Azure Batch 帳戶，並為其設定新的應用程式和 Azure 儲存體帳戶。

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
> [探索管理 API](/java/api/overview/azure/batch/managementapi)


## <a name="samples"></a>範例

[管理 Batch 帳戶][1]   

深入探索可在應用程式中使用的 [Azure Batch Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=batch)。

[1]: https://github.com/Azure-Samples/batch-java-manage-batch-accounts