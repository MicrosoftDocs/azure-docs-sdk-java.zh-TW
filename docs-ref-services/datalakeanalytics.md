---
title: "適用於 Java 的 Azure Data Lake Analytics 程式庫"
description: "Java Data Lake Analytics 程式庫的參考文件"
keywords: "Azure, Java, SDK, API, 巨量資料, data lake"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: 70cfe1417d460172df0cb753d2b719a635978ca8
ms.sourcegitcommit: 4b63ecd2c92a9115dfae018618e4e4046b061b3e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2017
---
# <a name="azure-data-lake-analytics-libraries-for-java"></a><span data-ttu-id="39511-104">適用於 Java 的 Azure Data Lake Analytics 程式庫</span><span class="sxs-lookup"><span data-stu-id="39511-104">Azure Data Lake Analytics libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="39511-105">概觀</span><span class="sxs-lookup"><span data-stu-id="39511-105">Overview</span></span>

<span data-ttu-id="39511-106">使用 [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) 來執行擴充至大規模資料集的巨量資料分析作業。</span><span class="sxs-lookup"><span data-stu-id="39511-106">Run big data analysis jobs that scale to massive data sets with [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview).</span></span>

<span data-ttu-id="39511-107">若要開始使用 Azure Data Lake Analytics，請參閱[透過 Java SDK 開始使用 Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk)。</span><span class="sxs-lookup"><span data-stu-id="39511-107">To get started with Azure Data Lake Analytics, see [Get started with Azure Data Lake Analytics using Java SDK](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk).</span></span>

## <a name="management-api"></a><span data-ttu-id="39511-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="39511-108">Management API</span></span>

<span data-ttu-id="39511-109">使用管理 API 來管理 Data Lake Analytics 帳戶、作業、原則和目錄。</span><span class="sxs-lookup"><span data-stu-id="39511-109">Use the management API to manage Data Lake Analytics accounts, jobs, policies, and catalogs.</span></span>

<span data-ttu-id="39511-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="39511-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-analytics</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="39511-111">範例</span><span class="sxs-lookup"><span data-stu-id="39511-111">Example</span></span>

<span data-ttu-id="39511-112">提交新的 U-SQL 作業到 Data Lake Analytics。</span><span class="sxs-lookup"><span data-stu-id="39511-112">Submit a new U-SQL job to Data Lake Analytics.</span></span>

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
> [<span data-ttu-id="39511-113">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="39511-113">Explore the Client APIs</span></span>](/java/api/overview/azure/datalakeanalytics/managementapi)

## <a name="samples"></a><span data-ttu-id="39511-114">範例</span><span class="sxs-lookup"><span data-stu-id="39511-114">Samples</span></span>

<span data-ttu-id="39511-115">[使用 Java SDK 的 Azure Data Lake Analytics][1]</span><span class="sxs-lookup"><span data-stu-id="39511-115">[Azure Data Lake Analytics using Java SDK][1]</span></span> 

[1]: https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk

<span data-ttu-id="39511-116">檢視 Azure Data Lake Analytics 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics)。</span><span class="sxs-lookup"><span data-stu-id="39511-116">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics) of Azure Data Lake Analytics samples.</span></span>
