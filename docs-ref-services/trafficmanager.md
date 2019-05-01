---
title: 適用於 Java 的 Azure 流量管理員程式庫
description: Java 流量管理員管理程式庫的參考文件
keywords: Azure, Java, SDK, API, 負載平衡, 負載分散, 網路, 流量管理員
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: traffic-manager
ms.openlocfilehash: c9dc6bedfd8f7a79494a8a547983b2771e51a494
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593243"
---
# <a name="azure-traffic-manager-libraries-for-java"></a>適用於 Java 的 Azure 流量管理員程式庫

## <a name="overview"></a>概觀

使用 [Azure 流量管理員](/azure/traffic-manager/traffic-manager-overview)控制不同資料中心之服務端點的使用者流量散佈情況。

若要開始使用 Azure 流量管理員，請參閱[建立流量管理員設定檔](/azure/traffic-manager/traffic-manager-create-profile)。

## <a name="management-api"></a>管理 API

使用管理 API 建立流量管理員設定檔、定義端點，並變更路由方法。 

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.3.0</version>
</dependency>
```   

## <a name="example"></a>範例

建立流量管理員設定檔，並指派單一端點。

```java
TrafficManagerProfile tmProfile = azure.trafficManagerProfiles().define("testTMProfile")
        .withNewResourceGroup(Region.US_EAST)
        .withLeafDomainLabel("testTMProfile")
        .withPriorityBasedRouting()
        .defineAzureTargetEndpoint("endpoint")
        .toResourceId(webAppId)
        .withRoutingPriority(1)
        .attach()
        .create();
```

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/trafficmanager/management)

## <a name="samples"></a>範例

[跨多個區域平衡 Web 應用程式流量](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

深入探索可在應用程式中使用的 [Azure 流量管理員 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic)。
