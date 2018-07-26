---
title: 適用於 Java 的 Azure Data Lake Analytics 程式庫
description: Java Data Lake Analytics 程式庫的參考文件
keywords: Azure, Java, SDK, API, 巨量資料, data lake
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: c14c89f961951d114362adee4fec6239e78cffb3
ms.sourcegitcommit: 33c70f921f1524c8bdb69ad5a1a3c1b4b1de97ba
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026770"
---
# <a name="azure-data-lake-analytics-libraries-for-java"></a>適用於 Java 的 Azure Data Lake Analytics 程式庫

## <a name="overview"></a>概觀

使用 [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) 來執行擴充至大規模資料集的巨量資料分析作業。

若要開始使用 Azure Data Lake Analytics，請參閱[透過 Java SDK 開始使用 Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk)。

## <a name="management-api"></a>管理 API

使用管理 API 來管理 Data Lake Analytics 帳戶、作業、原則和目錄。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-analytics</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

## <a name="example"></a>範例

提交新的 U-SQL 作業到 Data Lake Analytics。

```java
// authenticate with service principal credentials
ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
DataLakeAnalyticsJobManagementClient adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);

// set up job parameters
UUID jobId = java.util.UUID.randomUUID();
USqlJobProperties properties = new USqlJobProperties();
properties.setScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input TO @\"/output1.csv\" USING Outputters.Csv();");
JobInformation parameters = new JobInformation();
parameters.setName("testJob");
parameters.setJobId(jobId);
parameters.setType(JobType.USQL);
parameters.setProperties(properties);

// create the job
JobInformation jobInfo = adlaJobClient.getJobOperations().create(accountName, jobId, parameters).getBody();

```

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/datalakeanalytics/management)

## <a name="samples"></a>範例

[使用 Java SDK 的 Azure Data Lake Analytics][1] 

[1]: https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk

檢視 Azure Data Lake Analytics 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics)。
