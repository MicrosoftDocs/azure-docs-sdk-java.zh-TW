---
title: 適用於 Java 的 Azure CDN 程式庫
description: Java CDN 管理程式庫的參考文件
keywords: Azure, Java, SDK, API, 內容, 散發, 網路, CDN
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cdn
ms.openlocfilehash: 199e9b4b2b2431e23954d24e4adeb4326eb4741c
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893069"
---
# <a name="azure-cdn-libraries-for-java"></a><span data-ttu-id="d1697-104">適用於 Java 的 Azure CDN 程式庫</span><span class="sxs-lookup"><span data-stu-id="d1697-104">Azure CDN libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="d1697-105">概觀</span><span class="sxs-lookup"><span data-stu-id="d1697-105">Overview</span></span>

<span data-ttu-id="d1697-106">在策略性放置的位置上快取靜態 Web 內容，以針對使用 [Azure 內容傳遞網路](/azure/cdn/cdn-overview) (CDN) 的使用者提供最大輸送量。</span><span class="sxs-lookup"><span data-stu-id="d1697-106">Cache static web content at strategically placed locations to provide maximum throughput for users with [Azure Content Delivery Network](/azure/cdn/cdn-overview) (CDN).</span></span>

<span data-ttu-id="d1697-107">若要開始使用 Azure CDN，請參閱[開始使用 Azure CDN](/azure/cdn/cdn-create-new-endpoint)。</span><span class="sxs-lookup"><span data-stu-id="d1697-107">To get started with Azure CDN, see [Getting started with Azure CDN](/azure/cdn/cdn-create-new-endpoint).</span></span>

## <a name="management-api"></a><span data-ttu-id="d1697-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="d1697-108">Management API</span></span>

<span data-ttu-id="d1697-109">使用管理 API 建立 CDN 設定檔、定義端點，並將內容新增至 CDN。</span><span class="sxs-lookup"><span data-stu-id="d1697-109">Create CDN profiles, define endpoints, and add content to the CDN using the management API.</span></span>

<span data-ttu-id="d1697-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="d1697-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-cdn</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="d1697-111">範例</span><span class="sxs-lookup"><span data-stu-id="d1697-111">Example</span></span>

<span data-ttu-id="d1697-112">建立 CDN 設定檔、指派端點，以及將內容載入 CDN。</span><span class="sxs-lookup"><span data-stu-id="d1697-112">Create a CDN profile, assign endpoints, and load content into the CDN.</span></span>

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
> [<span data-ttu-id="d1697-113">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="d1697-113">Explore the Management APIs</span></span>](/java/api/overview/azure/cdn/management)

## <a name="samples"></a><span data-ttu-id="d1697-114">範例</span><span class="sxs-lookup"><span data-stu-id="d1697-114">Samples</span></span>

[<span data-ttu-id="d1697-115">使用 Java 管理 CDN</span><span class="sxs-lookup"><span data-stu-id="d1697-115">Manage CDNs with Java</span></span>](https://github.com/Azure-Samples/cdn-java-manage-cdn)

<span data-ttu-id="d1697-116">深入探索可在應用程式中使用的 [Azure CDN Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn)。</span><span class="sxs-lookup"><span data-stu-id="d1697-116">Explore more [sample Java code for Azure CDN](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn) you can use in your apps.</span></span>