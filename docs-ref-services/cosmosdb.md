---
title: 適用於 Java 的 Azure Cosmos DB 程式庫
description: 適用於 Azure Cosmos DB 之 Java 用戶端程式庫的參考文件
keywords: Azure, Java, SDK, API, SQL, 資料庫, MongoDB, Cosmos DB, NoSQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/10/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cosmosdb
ms.openlocfilehash: 845106b773de03aba8dd5edb9a18c6b036cf3215
ms.sourcegitcommit: 61030d025614b084e897809e603b2ec79900ec8d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2018
---
# <a name="azure-cosmos-db-libraries-for-java"></a><span data-ttu-id="65c73-104">適用於 Java 的 Azure Cosmos DB 程式庫</span><span class="sxs-lookup"><span data-stu-id="65c73-104">Azure Cosmos DB libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="65c73-105">概觀</span><span class="sxs-lookup"><span data-stu-id="65c73-105">Overview</span></span>

<span data-ttu-id="65c73-106">使用 [Azure Cosmos DB](/azure/cosmos-db/introduction) 在分散世界各地的資料庫中儲存和查詢金鑰值、JSON 文件、圖形和單欄式資料 。</span><span class="sxs-lookup"><span data-stu-id="65c73-106">Store and query key-value, JSON document, graph, and columnar data in a globally distributed database with [Azure Cosmos DB](/azure/cosmos-db/introduction).</span></span>

<span data-ttu-id="65c73-107">若要開始使用 Azure Cosmos DB，請參閱 [Azure Cosmos DB：使用 Java 和 Azure 入口網站建置 API 應用程式](/azure/cosmos-db/create-sql-api-java)。</span><span class="sxs-lookup"><span data-stu-id="65c73-107">To get started with Azure Cosmos DB, see [Azure Cosmos DB: Build an API app with Java and the Azure portal](/azure/cosmos-db/create-sql-api-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="65c73-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="65c73-108">Client library</span></span>

<span data-ttu-id="65c73-109">使用 [SQL API](/azure/cosmos-db/sql-api-introduction) 用戶端程式庫連線到 Azure Cosmos DB 來透過 [SQL 查詢語法](/azure/cosmos-db/sql-api-sql-query)處理 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="65c73-109">Connect to Azure Cosmos DB using the [SQL API](/azure/cosmos-db/sql-api-introduction) client library to work with JSON data with [SQL query syntax](/azure/cosmos-db/sql-api-sql-query).</span></span>

<span data-ttu-id="65c73-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用 Cosmos DB 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="65c73-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the Cosmos DB client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="65c73-111">範例</span><span class="sxs-lookup"><span data-stu-id="65c73-111">Example</span></span>

<span data-ttu-id="65c73-112">使用 SQL 查詢語法在 Cosmos DB 中選取相符的 JSON 文件。</span><span class="sxs-lookup"><span data-stu-id="65c73-112">Select matching JSON documents in Cosmos DB using SQL query syntax.</span></span>

```java
DocumentClient client = new DocumentClient("https://contoso.documents.azure.com:443",
                "contosoCosmosDBKey", 
                new ConnectionPolicy(),
                ConsistencyLevel.Session);

List<Document> results = client.queryDocuments("dbs/" + DATABASE_ID + "/colls/" + COLLECTION_ID,
        "SELECT * FROM myCollection WHERE myCollection.email = 'allen [at] contoso.com'",
        null)
    .getQueryIterable()
    .toList();

```

> [!div class="nextstepaction"]
> [<span data-ttu-id="65c73-113">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="65c73-113">Explore the Client APIs</span></span>](/java/api/overview/azure/cosmosdb/clientlibrary)


## <a name="samples"></a><span data-ttu-id="65c73-114">範例</span><span class="sxs-lookup"><span data-stu-id="65c73-114">Samples</span></span>

<span data-ttu-id="65c73-115">[使用 Azure Cosmos DB MongoDB API 開發 Java 應用程式][2] </span><span class="sxs-lookup"><span data-stu-id="65c73-115">[Develop a Java app using Azure Cosmos DB MongoDB API][2] </span></span>  
<span data-ttu-id="65c73-116">[使用 Azure Cosmos DB Graph API 開發 Java 應用程式][3] </span><span class="sxs-lookup"><span data-stu-id="65c73-116">[Develop a Java app using Azure Cosmos DB Graph API][3] </span></span>  
<span data-ttu-id="65c73-117">[使用 Azure Cosmos DB SQL API 開發 Java 應用程式][4]</span><span class="sxs-lookup"><span data-stu-id="65c73-117">[Develop a Java app using Azure Cosmos DB SQL API][4]</span></span>        

<span data-ttu-id="65c73-118">深入探索可在應用程式中使用的 [Azure Cosmos DB Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos)。</span><span class="sxs-lookup"><span data-stu-id="65c73-118">Explore more [sample Java code for Azure Cosmos DB](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos) you can use in your apps.</span></span>

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started
