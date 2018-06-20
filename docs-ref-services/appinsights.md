---
title: 適用於 Java 的 Azure Application Insights 程式庫
description: 適用於 Azure Appplication Insights 之 Java 管理 API 的參考文件
keywords: Azure, Java, SDK, API, AppInsights, 遙測, 診斷, 追蹤, 記錄, 效能
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/21/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: appinsights
ms.openlocfilehash: d881ff66ad806e13f7d2cbafff6ce85c23240304
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823761"
---
# <a name="azure-application-insights-libraries-for-java"></a>適用於 Java 的 Azure Application Insights 程式庫

## <a name="overview"></a>概觀

使用 [Application Insights](/azure/application-insights/app-insights-overview) 偵測、分級和診斷 Web Apps 和服務的問題。

若要開始使用 Application Insights，請參閱[在 Java Web 專案中開始使用 Application Insights](/azure/application-insights/app-insights-java-get-started)。

## <a name="client-library"></a>用戶端程式庫

使用 Application Insights 用戶端程式庫來新增遙測，以追蹤應用程式中的事件、例外狀況和使用者計量。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>   
    <version>1.0.8</version>
</dependency>
```   

### <a name="example"></a>範例

建立新的計量項目並記錄其值。

```java
    MetricTelemetry sample = new MetricTelemetry();
    sample.setName("metric name");
    sample.setValue(42.3);
    telemetryClient.TrackMetric(sample);
```

> [!div class="nextstepaction"]
> [探索用戶端 API](/java/api/overview/azure/appinsights/client)

## <a name="samples"></a>範例

深入探索可在應用程式中使用的 [Application Insights Java 程式碼範例](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java)。
