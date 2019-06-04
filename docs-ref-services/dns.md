---
title: 適用於 Java 的 Azure DNS 程式庫
description: Azure DNS Java 管理程式庫的參考文件
keywords: Azure, Java, SDK, API, 網域, DNS, 名稱, 服務, 網域名稱服務
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: dns
ms.openlocfilehash: 90751d2134b218e16415effeb336a62c6c737cb3
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65626062"
---
# <a name="azure-dns-libraries-for-java"></a><span data-ttu-id="0966c-104">適用於 Java 的 Azure DNS 程式庫</span><span class="sxs-lookup"><span data-stu-id="0966c-104">Azure DNS libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="0966c-105">概觀</span><span class="sxs-lookup"><span data-stu-id="0966c-105">Overview</span></span>

<span data-ttu-id="0966c-106">透過 [Azure DNS](/azure/dns/dns-overview)，使用和其他 Azure 服務一樣的認證、API、工具和計費方式來提供網域名稱解析和管理 DNS 記錄。</span><span class="sxs-lookup"><span data-stu-id="0966c-106">Provide domain name resolution and manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services with [Azure DNS](/azure/dns/dns-overview).</span></span>

<span data-ttu-id="0966c-107">若要開始使用 Azure DNS，請參閱[利用 Azure CLI 2.0 開始使用 Azure DNS](/azure/dns/dns-getstarted-cli)。</span><span class="sxs-lookup"><span data-stu-id="0966c-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure CLI 2.0](/azure/dns/dns-getstarted-cli).</span></span>

## <a name="management-api"></a><span data-ttu-id="0966c-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="0966c-108">Management API</span></span>

<span data-ttu-id="0966c-109">使用管理 API 建立 DNS 區域，並將記錄新增至區域。</span><span class="sxs-lookup"><span data-stu-id="0966c-109">Create DNS zones and add records to zones with the management API.</span></span>

<span data-ttu-id="0966c-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="0966c-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-dns</artifactId>
    <version>1.22.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="0966c-111">範例</span><span class="sxs-lookup"><span data-stu-id="0966c-111">Example</span></span>

<span data-ttu-id="0966c-112">建立根 DNS 區域並在現有資源群組中新增 `www` CNAME 記錄。</span><span class="sxs-lookup"><span data-stu-id="0966c-112">Create a root DNS zone and add a `www` CNAME record in an existing resource group.</span></span>

```java
DnsZone rootDnsZone = azure.dnsZones().define("contoso.com")
        .withExistingResourceGroup("myResourceGroup")
        .create();
rootDnsZone = rootDnsZone.update()
        .withCNameRecordSet("www", "172.30.241.20")
        .apply();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="0966c-113">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="0966c-113">Explore the Management APIs</span></span>](/java/api/overview/azure/dns/management)

## <a name="samples"></a><span data-ttu-id="0966c-114">範例</span><span class="sxs-lookup"><span data-stu-id="0966c-114">Samples</span></span>

[<span data-ttu-id="0966c-115">使用 Azure DNS 裝載和管理網域</span><span class="sxs-lookup"><span data-stu-id="0966c-115">Host and manage your domains with Azure DNS</span></span>](https://github.com/Azure-Samples/dns-java-host-and-manage-your-domains)

<span data-ttu-id="0966c-116">深入探索可在應用程式中使用的 [Azure DNS Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=dns)。</span><span class="sxs-lookup"><span data-stu-id="0966c-116">Explore more [sample Java code for Azure DNS](https://azure.microsoft.com/resources/samples/?platform=java&term=dns) you can use in your apps.</span></span>

<!---Loc Comment: Please, refer to conversation section to check the issue. Thanks.--->
