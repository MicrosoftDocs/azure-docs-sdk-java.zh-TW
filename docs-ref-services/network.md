---
title: "適用於 Java 的 Azure 網路程式庫"
description: "Java Azure 網路管理程式庫的參考文件"
keywords: "Azure, Java, SDK, API, 網路, 負載平衡, vnet , 子網路"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: networking
ms.openlocfilehash: 6eed6f45ee239db1286e94f210341febb189378d
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2017
---
# <a name="azure-network-libraries-for-java"></a><span data-ttu-id="41ca7-104">適用於 Java 的 Azure 網路程式庫</span><span class="sxs-lookup"><span data-stu-id="41ca7-104">Azure Network libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="41ca7-105">概觀</span><span class="sxs-lookup"><span data-stu-id="41ca7-105">Overview</span></span>

<span data-ttu-id="41ca7-106">使用 [Azure 網路](/azure/networking/networking-overview)連線到 Azure 資源、篩選和平衡流量，以及管理路由。</span><span class="sxs-lookup"><span data-stu-id="41ca7-106">Connect Azure resources, filter and balance traffic, and manage routing with [Azure Networking](/azure/networking/networking-overview).</span></span>

<span data-ttu-id="41ca7-107">若要開始使用 Azure 網路，請參閱[建立您的第一個虛擬網路](/azure/virtual-network/virtual-network-get-started-vnet-subnet)。</span><span class="sxs-lookup"><span data-stu-id="41ca7-107">To get started with Azure Networking, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-api"></a><span data-ttu-id="41ca7-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="41ca7-108">Management API</span></span>

<span data-ttu-id="41ca7-109">使用管理 API 來建立和管理 Azure [虛擬網路](/azure/virtual-network/virtual-networks-overview)、[ExpressRoutes](/azure/expressroute/) 和[應用程式閘道](/azure/application-gateway/)。</span><span class="sxs-lookup"><span data-stu-id="41ca7-109">Create and manage Azure [virtual networks](/azure/virtual-network/virtual-networks-overview) , [ExpressRoutes](/azure/expressroute/) , and [Application Gateways](/azure/application-gateway/) with the management API.</span></span>

<span data-ttu-id="41ca7-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="41ca7-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-network</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="41ca7-111">範例</span><span class="sxs-lookup"><span data-stu-id="41ca7-111">Example</span></span>

<span data-ttu-id="41ca7-112">建立具有單一子網路的新虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="41ca7-112">Create a new virtual network with a single subnet.</span></span>

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
> [<span data-ttu-id="41ca7-113">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="41ca7-113">Explore the Management APIs</span></span>](/java/api/overview/azure/networking/managementapi)

## <a name="samples"></a><span data-ttu-id="41ca7-114">範例</span><span class="sxs-lookup"><span data-stu-id="41ca7-114">Samples</span></span>

<span data-ttu-id="41ca7-115">[管理虛擬網路](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span><span class="sxs-lookup"><span data-stu-id="41ca7-115">[Manage virtual networks](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span></span>  
<span data-ttu-id="41ca7-116">[管理網路介面](https://github.com/Azure-Samples/network-java-manage-network-interface) </span><span class="sxs-lookup"><span data-stu-id="41ca7-116">[Manage network interfaces](https://github.com/Azure-Samples/network-java-manage-network-interface) </span></span>  
<span data-ttu-id="41ca7-117">[管理應用程式閘道](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span><span class="sxs-lookup"><span data-stu-id="41ca7-117">[Manage Application Gateways](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span></span>  
[<span data-ttu-id="41ca7-118">管理網際網路對向負載平衡器</span><span class="sxs-lookup"><span data-stu-id="41ca7-118">Manage internet facing load balancers</span></span>](https://github.com/Azure-Samples/network-java-manage-internet-facing-load-balancers)   

<span data-ttu-id="41ca7-119">深入探索可在應用程式中使用的 [Azure 網路 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=network)。</span><span class="sxs-lookup"><span data-stu-id="41ca7-119">Explore more [sample Java code for Azure Networking](https://azure.microsoft.com/resources/samples/?platform=java&term=network) you can use in your apps.</span></span>
