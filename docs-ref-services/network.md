---
title: 適用於 Java 的 Azure 網路程式庫
description: Java Azure 網路管理程式庫的參考文件
keywords: Azure, Java, SDK, API, 網路, 負載平衡, vnet , 子網路
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: networking
ms.openlocfilehash: 4eb4f583da686508db9fb0c4346f2525502f669d
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593323"
---
# <a name="azure-network-libraries-for-java"></a><span data-ttu-id="28a4e-104">適用於 Java 的 Azure 網路程式庫</span><span class="sxs-lookup"><span data-stu-id="28a4e-104">Azure Network libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="28a4e-105">概觀</span><span class="sxs-lookup"><span data-stu-id="28a4e-105">Overview</span></span>

<span data-ttu-id="28a4e-106">使用 [Azure 網路](/azure/networking/networking-overview)連線到 Azure 資源、篩選和平衡流量，以及管理路由。</span><span class="sxs-lookup"><span data-stu-id="28a4e-106">Connect Azure resources, filter and balance traffic, and manage routing with [Azure Networking](/azure/networking/networking-overview).</span></span>

<span data-ttu-id="28a4e-107">若要開始使用 Azure 網路，請參閱[建立您的第一個虛擬網路](/azure/virtual-network/virtual-network-get-started-vnet-subnet)。</span><span class="sxs-lookup"><span data-stu-id="28a4e-107">To get started with Azure Networking, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-api"></a><span data-ttu-id="28a4e-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="28a4e-108">Management API</span></span>

<span data-ttu-id="28a4e-109">使用管理 API 來建立和管理 Azure [虛擬網路](/azure/virtual-network/virtual-networks-overview)、[ExpressRoutes](/azure/expressroute/) 和[應用程式閘道](/azure/application-gateway/)。</span><span class="sxs-lookup"><span data-stu-id="28a4e-109">Create and manage Azure [virtual networks](/azure/virtual-network/virtual-networks-overview) , [ExpressRoutes](/azure/expressroute/) , and [Application Gateways](/azure/application-gateway/) with the management API.</span></span>

<span data-ttu-id="28a4e-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="28a4e-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-network</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="28a4e-111">範例</span><span class="sxs-lookup"><span data-stu-id="28a4e-111">Example</span></span>

<span data-ttu-id="28a4e-112">建立具有單一子網路的新虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="28a4e-112">Create a new virtual network with a single subnet.</span></span>

```java
Network virtualNetwork1 = azure.networks().define(vnetName1)
        .withRegion(Region.US_EAST)
        .withExistingResourceGroup("myResourceGroup")
        .withAddressSpace("192.168.0.0/16")
        .defineSubnet("myVirtualNetwork")
            .withAddressPrefix("192.168.2.0/24")
            .attach()
        .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="28a4e-113">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="28a4e-113">Explore the Management APIs</span></span>](/java/api/overview/azure/networking/management)

## <a name="samples"></a><span data-ttu-id="28a4e-114">範例</span><span class="sxs-lookup"><span data-stu-id="28a4e-114">Samples</span></span>

<span data-ttu-id="28a4e-115">[管理虛擬網路](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span><span class="sxs-lookup"><span data-stu-id="28a4e-115">[Manage virtual networks](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span></span>  
<span data-ttu-id="28a4e-116">[管理網路介面](https://github.com/Azure-Samples/network-java-manage-network-interface) </span><span class="sxs-lookup"><span data-stu-id="28a4e-116">[Manage network interfaces](https://github.com/Azure-Samples/network-java-manage-network-interface) </span></span>  
<span data-ttu-id="28a4e-117">[管理應用程式閘道](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span><span class="sxs-lookup"><span data-stu-id="28a4e-117">[Manage Application Gateways](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span></span>  
[<span data-ttu-id="28a4e-118">管理網際網路對向負載平衡器</span><span class="sxs-lookup"><span data-stu-id="28a4e-118">Manage internet facing load balancers</span></span>](https://github.com/Azure-Samples/network-java-manage-internet-facing-load-balancers)   

<span data-ttu-id="28a4e-119">深入探索可在應用程式中使用的 [Azure 網路 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=network)。</span><span class="sxs-lookup"><span data-stu-id="28a4e-119">Explore more [sample Java code for Azure Networking](https://azure.microsoft.com/resources/samples/?platform=java&term=network) you can use in your apps.</span></span>
