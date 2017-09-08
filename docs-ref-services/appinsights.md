---
title: "適用於 Java 的 Azure Application Insights 程式庫"
description: "適用於 Azure Appplication Insights 之 Java 管理 API 的參考文件"
keywords: "Azure, Java, SDK, API, AppInsights, 遙測, 診斷, 追蹤, 記錄, 效能"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/21/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: appinsights
ms.openlocfilehash: 9f943dc87d9e9b3e015407eea4dfd2900040da37
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="azure-application-insights-libraries-for-java"></a><span data-ttu-id="3102c-104">適用於 Java 的 Azure Application Insights 程式庫</span><span class="sxs-lookup"><span data-stu-id="3102c-104">Azure Application Insights libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="3102c-105">概觀</span><span class="sxs-lookup"><span data-stu-id="3102c-105">Overview</span></span>

<span data-ttu-id="3102c-106">使用 [Application Insights](/azure/application-insights/app-insights-overview) 偵測、分級和診斷 Web Apps 和服務的問題。</span><span class="sxs-lookup"><span data-stu-id="3102c-106">Detect, triage, and diagnose issues in your web apps and services with [Application Insights](/azure/application-insights/app-insights-overview).</span></span>

<span data-ttu-id="3102c-107">若要開始使用 Application Insights，請參閱[在 Java Web 專案中開始使用 Application Insights](/azure/application-insights/app-insights-java-get-started)。</span><span class="sxs-lookup"><span data-stu-id="3102c-107">To get started with Application Insights, see [Get started with Application Insights in a Java web project](/azure/application-insights/app-insights-java-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="3102c-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="3102c-108">Client library</span></span>

<span data-ttu-id="3102c-109">使用 Application Insights 用戶端程式庫來新增遙測，以追蹤應用程式中的事件、例外狀況和使用者計量。</span><span class="sxs-lookup"><span data-stu-id="3102c-109">Add telemetry to track events, exceptions, and user metrics in your apps with the Application Insights client library.</span></span>

<span data-ttu-id="3102c-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="3102c-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>   
    <version>1.0.8</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="3102c-111">範例</span><span class="sxs-lookup"><span data-stu-id="3102c-111">Example</span></span>

<span data-ttu-id="3102c-112">建立新的計量項目並記錄其值。</span><span class="sxs-lookup"><span data-stu-id="3102c-112">Create a new metric entry and record a value for it.</span></span>

```java
    MetricTelemetry sample = new MetricTelemetry();
    sample.setName("metric name");
    sample.setValue(42.3);
    telemetryClient.TrackMetric(sample);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="3102c-113">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="3102c-113">Explore the Client APIs</span></span>](/java/api/overview/azure/appinsights/clientlibrary)

## <a name="samples"></a><span data-ttu-id="3102c-114">範例</span><span class="sxs-lookup"><span data-stu-id="3102c-114">Samples</span></span>

<span data-ttu-id="3102c-115">深入探索可在應用程式中使用的 [Application Insights Java 程式碼範例](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java)。</span><span class="sxs-lookup"><span data-stu-id="3102c-115">Explore more [sample Java code for Application Insights](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java) you can use in your apps.</span></span>