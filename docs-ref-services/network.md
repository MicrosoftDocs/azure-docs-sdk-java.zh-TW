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
ms.openlocfilehash: bb74ccd8826df7b627e0b5f4e4ffd2da44b2642d
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823601"
---
# <a name="azure-network-libraries-for-java"></a>適用於 Java 的 Azure 網路程式庫

## <a name="overview"></a>概觀

使用 [Azure 網路](/azure/networking/networking-overview)連線到 Azure 資源、篩選和平衡流量，以及管理路由。

若要開始使用 Azure 網路，請參閱[建立您的第一個虛擬網路](/azure/virtual-network/virtual-network-get-started-vnet-subnet)。

## <a name="management-api"></a>管理 API

使用管理 API 來建立和管理 Azure [虛擬網路](/azure/virtual-network/virtual-networks-overview)、[ExpressRoutes](/azure/expressroute/) 和[應用程式閘道](/azure/application-gateway/)。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-network</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a>範例

建立具有單一子網路的新虛擬網路。

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
> [探索管理 API](/java/api/overview/azure/networking/management)

## <a name="samples"></a>範例

[管理虛擬網路](https://github.com/Azure-Samples/network-java-manage-virtual-network)   
[管理網路介面](https://github.com/Azure-Samples/network-java-manage-network-interface)   
[管理應用程式閘道](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways)   
[管理網際網路對向負載平衡器](https://github.com/Azure-Samples/network-java-manage-internet-facing-load-balancers)   

深入探索可在應用程式中使用的 [Azure 網路 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=network)。
