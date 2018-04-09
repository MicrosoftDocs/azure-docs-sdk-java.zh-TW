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
# <a name="azure-cosmos-db-libraries-for-java"></a>適用於 Java 的 Azure Cosmos DB 程式庫

## <a name="overview"></a>概觀

使用 [Azure Cosmos DB](/azure/cosmos-db/introduction) 在分散世界各地的資料庫中儲存和查詢金鑰值、JSON 文件、圖形和單欄式資料 。

若要開始使用 Azure Cosmos DB，請參閱 [Azure Cosmos DB：使用 Java 和 Azure 入口網站建置 API 應用程式](/azure/cosmos-db/create-sql-api-java)。

## <a name="client-library"></a>用戶端程式庫

使用 [SQL API](/azure/cosmos-db/sql-api-introduction) 用戶端程式庫連線到 Azure Cosmos DB 來透過 [SQL 查詢語法](/azure/cosmos-db/sql-api-sql-query)處理 JSON 資料。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用 Cosmos DB 用戶端程式庫。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

### <a name="example"></a>範例

使用 SQL 查詢語法在 Cosmos DB 中選取相符的 JSON 文件。

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
> [探索用戶端 API](/java/api/overview/azure/cosmosdb/clientlibrary)


## <a name="samples"></a>範例

[使用 Azure Cosmos DB MongoDB API 開發 Java 應用程式][2]   
[使用 Azure Cosmos DB Graph API 開發 Java 應用程式][3]   
[使用 Azure Cosmos DB SQL API 開發 Java 應用程式][4]        

深入探索可在應用程式中使用的 [Azure Cosmos DB Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos)。

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started
