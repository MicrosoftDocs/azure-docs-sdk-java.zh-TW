---
title: "適用於 Java 的 Azure CDN 程式庫"
description: "Java CDN 管理程式庫的參考文件"
keywords: "Azure, Java, SDK, API, 內容, 散發, 網路, CDN"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cdn
ms.openlocfilehash: db9fb8a9a8f9f21061f1e16b77e3145d1fb1b119
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2017
---
# <a name="azure-cdn-libraries-for-java"></a>適用於 Java 的 Azure CDN 程式庫

## <a name="overview"></a>概觀

在策略性放置的位置上快取靜態 Web 內容，以針對使用 [Azure 內容傳遞網路](/azure/cdn/cdn-overview) (CDN) 的使用者提供最大輸送量。

若要開始使用 Azure CDN，請參閱[開始使用 Azure CDN](/azure/cdn/cdn-create-new-endpoint)。

## <a name="management-api"></a>管理 API

使用管理 API 建立 CDN 設定檔、定義端點，並將內容新增至 CDN。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-cdn</artifactId>
    <version>1.2.1</version>
</dependency>
```   

### <a name="example"></a>範例

建立 CDN 設定檔、指派端點，以及將內容載入 CDN。

```java
CdnProfile profile = azure.cdnProfiles().define("testCDN")
        .withRegion(Region.US_EAST)
        .withNewResourceGroup()
        .withStandardVerizonSku()
        .withNewEndpoint("webAppHostname1")
        .withNewEndpoint("webAppHostname2")
        .create();

List<String> contentList = new ArrayList<String>();
contentList.add("/client.js");
contentList.add("/media/fabrikam_logo.png");

for (CdnEndpoint endpoint : profile.endpoints().values()) {
    endpoint.loadContent(contentList);
}
```

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/cdn/managementapi)

## <a name="samples"></a>範例

[使用 Java 管理 CDN](https://github.com/Azure-Samples/cdn-java-manage-cdn)

深入探索可在應用程式中使用的 [Azure CDN Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn)。