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
# <a name="azure-dns-libraries-for-java"></a>適用於 Java 的 Azure DNS 程式庫

## <a name="overview"></a>概觀

透過 [Azure DNS](/azure/dns/dns-overview)，使用和其他 Azure 服務一樣的認證、API、工具和計費方式來提供網域名稱解析和管理 DNS 記錄。

若要開始使用 Azure DNS，請參閱[利用 Azure CLI 2.0 開始使用 Azure DNS](/azure/dns/dns-getstarted-cli)。

## <a name="management-api"></a>管理 API

使用管理 API 建立 DNS 區域，並將記錄新增至區域。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-dns</artifactId>
    <version>1.22.0</version>
</dependency>
```   

### <a name="example"></a>範例

建立根 DNS 區域並在現有資源群組中新增 `www` CNAME 記錄。

```java
DnsZone rootDnsZone = azure.dnsZones().define("contoso.com")
        .withExistingResourceGroup("myResourceGroup")
        .create();
rootDnsZone = rootDnsZone.update()
        .withCNameRecordSet("www", "172.30.241.20")
        .apply();
```

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/dns/management)

## <a name="samples"></a>範例

[使用 Azure DNS 裝載和管理網域](https://github.com/Azure-Samples/dns-java-host-and-manage-your-domains)

深入探索可在應用程式中使用的 [Azure DNS Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=dns)。

<!---Loc Comment: Please, refer to conversation section to check the issue. Thanks.--->
