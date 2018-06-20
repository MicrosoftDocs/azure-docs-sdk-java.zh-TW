---
title: 適用於 Java 的 Azure Database for PostgreSQL 程式庫
description: 適用於 Azure Database for PostgreSQL 之 Java 用戶端程式庫的參考文件
keywords: Azure, Java, SDK, API, SQL, 資料庫, PostGres, PostgreSQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: postgresql
ms.openlocfilehash: d6fa2acb9a2f44b157ae52ba6e41f6777a43b574
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
ms.locfileid: "21930974"
---
# <a name="azure-database-for-postgresql-libraries-for-java"></a><span data-ttu-id="d06d6-104">適用於 Java 的 Azure Database for PostgreSQL 程式庫</span><span class="sxs-lookup"><span data-stu-id="d06d6-104">Azure Database for PostgreSQL libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="d06d6-105">概觀</span><span class="sxs-lookup"><span data-stu-id="d06d6-105">Overview</span></span>

<span data-ttu-id="d06d6-106">[Azure Database for PostgreSQL](/azure/sql-database/sql-database-technical-overview) 是 Azure 中以開放原始碼 [PostgreSQL](https://www.postgresql.org/) 資料庫引擎的社群版為基礎的關聯式資料庫服務，目標對象是開發人員。</span><span class="sxs-lookup"><span data-stu-id="d06d6-106">[Azure Database for PostgreSQL](/azure/sql-database/sql-database-technical-overview) is a relational database service in Azure built for developers based on the community version of open source [PostgreSQL](https://www.postgresql.org/) database engine.</span></span>

<span data-ttu-id="d06d6-107">若要開始使用 Azure Database for PostgreSQL，請參閱[使用 Java 連線及查詢資料](/azure/postgresql/connect-java)。</span><span class="sxs-lookup"><span data-stu-id="d06d6-107">To get started with Azure Database for PostgreSQL, see [Use Java to connect and query data](/azure/postgresql/connect-java).</span></span>

## <a name="client-jdbc-driver"></a><span data-ttu-id="d06d6-108">用戶端 JDBC 驅動程式</span><span class="sxs-lookup"><span data-stu-id="d06d6-108">Client JDBC driver</span></span>

<span data-ttu-id="d06d6-109">使用開放原始碼 [PostgreSQL JDBC 驅動程式](https://jdbc.postgresql.org/)從應用程式連線到 Azure Database for PostgreSQL。</span><span class="sxs-lookup"><span data-stu-id="d06d6-109">Connect to Azure Database for PostgreSQL from your applications using the open-source [PostgreSQL JDBC driver](https://jdbc.postgresql.org/).</span></span> <span data-ttu-id="d06d6-110">您可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接連線到資料庫，或使用資料存取架構透過 JDBC (例如 [Hibernate](http://hibernate.org/)) 與資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="d06d6-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect to the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="d06d6-111">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端 JDBC 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="d06d6-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>  

```XML
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.1.1</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="d06d6-112">範例</span><span class="sxs-lookup"><span data-stu-id="d06d6-112">Example</span></span>

<span data-ttu-id="d06d6-113">使用 JDBC 連線到 Azure Database for PostgreSQL 並選取銷售資料表中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="d06d6-113">Connect to Azure Database for PostgreSQL using JDBC and select all records in the sales table.</span></span> <span data-ttu-id="d06d6-114">您可以從 Azure 入口網站取得資料庫的 JDBC 連接字串。</span><span class="sxs-lookup"><span data-stu-id="d06d6-114">You can get the JDBC connection string for the database from the Azure Portal.</span></span>

```java
String url = String.format("jdbc:postgresql://mypostgresdb.postgres.database.azure.com:5432/mydb?user=frank@mypostgresdb&password=AbCdEfGhIjK&ssl=true");
Connection connection = null;
try {
    connection = DriverManager.getConnection(url);
    String selectSql = "SELECT * FROM SALES";
    Statement statement = connection.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a><span data-ttu-id="d06d6-115">範例</span><span class="sxs-lookup"><span data-stu-id="d06d6-115">Samples</span></span>

[<span data-ttu-id="d06d6-116">使用 Azure CLI 設計 PostgreSQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="d06d6-116">Design a PostgreSQL database using the Azure CLI</span></span>](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-cli) 

<span data-ttu-id="d06d6-117">深入探索可在應用程式中使用的 [Azure Database for PostgreSQL Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres)。</span><span class="sxs-lookup"><span data-stu-id="d06d6-117">Explore more [sample Java code for Azure Database for PostgreSQL](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres) you can use in your apps.</span></span>
