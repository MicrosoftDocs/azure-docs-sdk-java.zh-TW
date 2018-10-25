---
title: 適用於 Java 的 Azure Data Lake Store 程式庫
description: Java Data Lake Store 程式庫的參考文件
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
ms.openlocfilehash: ebbadff83e1e081153f0ae376fb71d6607e63f15
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799794"
---
# <a name="azure-data-lake-store-libraries-for-java"></a><span data-ttu-id="c3db7-104">適用於 Java 的 Azure Data Lake Store 程式庫</span><span class="sxs-lookup"><span data-stu-id="c3db7-104">Azure Data Lake Store libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="c3db7-105">概觀</span><span class="sxs-lookup"><span data-stu-id="c3db7-105">Overview</span></span>

<span data-ttu-id="c3db7-106">使用 [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview) 在單一位置擷取任何大小、類型和擷取速度的資料以供分析。</span><span class="sxs-lookup"><span data-stu-id="c3db7-106">Capture data of any size, type, and ingestion speed in a single place for analytics with [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview).</span></span>

<span data-ttu-id="c3db7-107">若要開始使用 Data Lake Store，請參閱[使用 Java 開始使用 Azure Data Lake Store](/azure/data-lake-store/data-lake-store-get-started-java-sdk)。</span><span class="sxs-lookup"><span data-stu-id="c3db7-107">To get started with Data Lake Store, see [Get started with Azure Data Lake Store using Java](/azure/data-lake-store/data-lake-store-get-started-java-sdk).</span></span>


## <a name="client-library"></a><span data-ttu-id="c3db7-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="c3db7-108">Client library</span></span>

<span data-ttu-id="c3db7-109">使用用戶端程式庫在 Data Lake Store 中讀取和寫入檔案、設定權限和中繼資料，以及管理檔案和目錄。</span><span class="sxs-lookup"><span data-stu-id="c3db7-109">Read and write files, set permissions and metadata, and manage files and directories in Data Lake Store with the client library.</span></span>

<span data-ttu-id="c3db7-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="c3db7-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="c3db7-111">範例</span><span class="sxs-lookup"><span data-stu-id="c3db7-111">Example</span></span>

<span data-ttu-id="c3db7-112">透過完整網域名稱和 OAuth2 存取權杖建立 Data Lake 用戶端，然後在 Data Lake 中建立檔案並將檔案寫入其中。</span><span class="sxs-lookup"><span data-stu-id="c3db7-112">Create a Data Lake client from a fully qualified domain name and OAuth2 access token, then create a file in Data Lake and write to it.</span></span>

```java
// AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);
ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

// create directory
client.createDirectory("/a/b/w");
        
// create file and write some content
String filename = "/a/b/c.txt";
OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
PrintStream out = new PrintStream(stream);
for (int i = 1; i <= 10; i++) {
    out.println("This is line #" + i);
    out.format("This is the same line (%d), but using formatted output. %n", i);
}
out.close();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="c3db7-113">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="c3db7-113">Explore the Client APIs</span></span>](/java/api/overview/azure/datalakestore/client)


## <a name="management-api"></a><span data-ttu-id="c3db7-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="c3db7-114">Management API</span></span>

<span data-ttu-id="c3db7-115">使用管理 API 來管理 Data Lake Store 帳戶、防火牆規則和受信任的識別提供者。</span><span class="sxs-lookup"><span data-stu-id="c3db7-115">Use the management API to manage Data Lake Store accounts, firewall rules, and trusted identity providers.</span></span>

<span data-ttu-id="c3db7-116">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="c3db7-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-store</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="c3db7-117">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="c3db7-117">Explore the Management APIs</span></span>](/java/api/overview/azure/datalakestore/management)

## <a name="samples"></a><span data-ttu-id="c3db7-118">範例</span><span class="sxs-lookup"><span data-stu-id="c3db7-118">Samples</span></span>

<span data-ttu-id="c3db7-119">[開始使用 Azure Data Lake][1]</span><span class="sxs-lookup"><span data-stu-id="c3db7-119">[Azure Data Lake Get Started][1]</span></span> 

[1]: https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started

<span data-ttu-id="c3db7-120">深入探索可在應用程式中使用的 [ Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=lake)。</span><span class="sxs-lookup"><span data-stu-id="c3db7-120">Explore more [sample Java code for Azure Data Lake Store](https://azure.microsoft.com/resources/samples/?platform=java&term=lake) you can use in your apps.</span></span>
