---
title: "適用於 Java 的 Azure Resource Manager 程式庫"
description: "Java Resource Manager 程式庫的參考文件"
keywords: "Azure, Java, SDK, API, 資源群組, arm, resource manager"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: 56199b87fa64e9cbf0a14716a58c01f11f0e433b
ms.sourcegitcommit: 4b63ecd2c92a9115dfae018618e4e4046b061b3e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2017
---
# <a name="azure-resource-manager-libraries-for-java"></a>適用於 Java 的 Azure Resource Manager 程式庫

## <a name="overview"></a>概觀

使用 [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) 部署、監視和管理群組中的資源。

## <a name="management-api"></a>管理 API

使用管理 API 來透過範本建立資源群組及部署資源。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-resources</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a>範例

在 Azure 的美國東部區域建立新的資源群組。

```java
ResourceGroup resourceGroup = azure.resourceGroups().define("myResourceGroup")
            .withRegion(Region.US_EAST)
            .create();
```

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/resources/managementapi)

## <a name="samples"></a>範例

[使用 Java 管理 Azure 資源群組][1] 
[使用 ARM 範本部署資源][2]

[1]: https://github.com/Azure-Samples/resources-java-manage-resource-group
[2]: https://github.com/Azure-Samples/resources-java-deploy-using-arm-template

檢視 Azure Resource Manager 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=java&term=resource)。
