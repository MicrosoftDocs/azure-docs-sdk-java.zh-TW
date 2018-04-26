---
title: 適用於 Java 的 Azure Resource Manager 程式庫
description: Java Resource Manager 程式庫的參考文件
keywords: Azure, Java, SDK, API, 資源群組, arm, resource manager
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: 326357e5b4667cc06a6058cb29e9685428174dee
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
---
# <a name="azure-resource-manager-libraries-for-java"></a><span data-ttu-id="47ebc-104">適用於 Java 的 Azure Resource Manager 程式庫</span><span class="sxs-lookup"><span data-stu-id="47ebc-104">Azure Resource Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="47ebc-105">概觀</span><span class="sxs-lookup"><span data-stu-id="47ebc-105">Overview</span></span>

<span data-ttu-id="47ebc-106">使用 [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) 部署、監視和管理群組中的資源。</span><span class="sxs-lookup"><span data-stu-id="47ebc-106">Deploy, monitor, and manage resources in groups with [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).</span></span>

## <a name="management-api"></a><span data-ttu-id="47ebc-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="47ebc-107">Management API</span></span>

<span data-ttu-id="47ebc-108">使用管理 API 來透過範本建立資源群組及部署資源。</span><span class="sxs-lookup"><span data-stu-id="47ebc-108">Use the management API to create resource groups and deploy resources from templates.</span></span>

<span data-ttu-id="47ebc-109">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="47ebc-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-resources</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="47ebc-110">範例</span><span class="sxs-lookup"><span data-stu-id="47ebc-110">Example</span></span>

<span data-ttu-id="47ebc-111">在 Azure 的美國東部區域建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="47ebc-111">Create a new resource group in the Azure Eastern US region.</span></span>

```java
ResourceGroup resourceGroup = azure.resourceGroups().define("myResourceGroup")
            .withRegion(Region.US_EAST)
            .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="47ebc-112">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="47ebc-112">Explore the Management APIs</span></span>](/java/api/overview/azure/resources/management)

## <a name="samples"></a><span data-ttu-id="47ebc-113">範例</span><span class="sxs-lookup"><span data-stu-id="47ebc-113">Samples</span></span>

<span data-ttu-id="47ebc-114">[使用 Java 管理 Azure 資源群組][1] 
[使用 ARM 範本部署資源][2]</span><span class="sxs-lookup"><span data-stu-id="47ebc-114">[Manage Azure Resource Groups with Java][1] 
[Deploy resources using an ARM template][2]</span></span>

[1]: https://github.com/Azure-Samples/resources-java-manage-resource-group
[2]: https://github.com/Azure-Samples/resources-java-deploy-using-arm-template

<span data-ttu-id="47ebc-115">檢視 Azure Resource Manager 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=java&term=resource)。</span><span class="sxs-lookup"><span data-stu-id="47ebc-115">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=java&term=resource) of Azure Resource Manager samples.</span></span>
