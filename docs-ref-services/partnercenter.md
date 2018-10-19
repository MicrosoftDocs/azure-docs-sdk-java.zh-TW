---
title: 合作夥伴中心 Java SDK
description: 合作夥伴中心 Java SDK 的參考文件
keywords: API, CSP, 雲端解決方案提供者, Java, 合作夥伴中心, SDK
author: iswillia
ms.author: iswillia
manager: lesliep
ms.date: 10/09/2018
ms.topic: reference
ms.prod: partnercenter
ms.technology: partnercenter
ms.devlang: java
ms.openlocfilehash: 2adfbdbd37d6eb91e48ee37091f951b3f7d59eb9
ms.sourcegitcommit: 4da7d2f470331feb7d76a1658f5825f365cdba9a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2018
ms.locfileid: "49121639"
---
# <a name="partner-center-libraries-for-java"></a>適用於 Java 的合作夥伴中心程式庫

## <a name="overview"></a>概觀

適用於 Java 的合作夥伴中心程式庫是一個 SDK，可與 Microsoft 合作夥伴中心服務互動。 這可讓合作夥伴以程式設計方式執行合作夥伴中心作業。 這是 REST APIs 和 .NET SDK 現有組合的最新增加項目。

## <a name="client-library"></a>用戶端程式庫

使用用戶端程式庫，在合作夥伴中心中管理資源。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。

```xml
<dependency>
    <groupId>com.microsoft.store</groupId>
    <artifactId>partnercenter</artifactId>
    <version>1.8.0</version>
</dependency>
```   

### <a name="example"></a>範例

擷取與已驗證轉銷商相關聯的完整客戶清單。

```java
IPartnerCredentials appCredentials = PartnerCredentials.getInstance().generateByApplicationCredentials('YOUR_APP_ID', 'YOUR_APP_SECRET', 'YOUR_TENANT_ID');
IAggregatePartner partnerOperations = PartnerService.getInstance().createPartnerOperations(appCredentials);

// Query the customers
SeekBasedResourceCollection<Customer> customersPage = partnerOperations.getCustomers().query(QueryFactory.getInstance().buildIndexedQuery(100));
this.getContext().getConsoleHelper().stopProgress();

// Create a customer enumerator which will aid us in traversing the customer pages
IResourceCollectionEnumerator<SeekBasedResourceCollection<Customer>> customersEnumerator =
    partnerOperations.getEnumerators().getCustomers().create( customersPage );

int pageNumber = 1;

while ( customersEnumerator.hasValue() )
{
    /*
     * Perform some operation with the current page of customers using 
     * customersEnumerator.getCurrent()  
     */

    // Get the next page of customers
    customersEnumerator.next();
}
```

## <a name="samples"></a>範例

[支援案例](https://docs.microsoft.com/partner-center/develop/scenarios)
[範例主控台應用程式](https://github.com/Microsoft/Partner-Center-Java-Samples)  