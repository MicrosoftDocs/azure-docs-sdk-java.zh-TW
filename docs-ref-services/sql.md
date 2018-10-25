---
title: 適用於 Java 的 Azure SQL Database 程式庫
description: 使用管理 API 透過 JDBC 驅動程式或管理 Azure SQL Database 執行個體來連線到 Azure SQL Database。
keywords: Azure, Java, SDK, API, SQL, 資料庫, JDBC
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/05/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: sql-database
ms.openlocfilehash: 75b31aa0ffd32707deb4b28f9912aa4c9d1e4d7c
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799784"
---
# <a name="azure-sql-database-libraries-for-java"></a><span data-ttu-id="f8e14-104">適用於 Java 的 Azure SQL Database 程式庫</span><span class="sxs-lookup"><span data-stu-id="f8e14-104">Azure SQL Database libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="f8e14-105">概觀</span><span class="sxs-lookup"><span data-stu-id="f8e14-105">Overview</span></span>

<span data-ttu-id="f8e14-106">[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) 是使用 Microsoft SQL Server 引擎的關聯式資料庫服務，可支援資料表、JSON、空間和 XML 資料。</span><span class="sxs-lookup"><span data-stu-id="f8e14-106">[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) is a relational database service using the Microsoft SQL Server engine that supports table, JSON, spatial, and XML data.</span></span> 

<span data-ttu-id="f8e14-107">若要開始使用 Azure SQL Database，請參閱 [Azure SQL Database：使用 Java 連線及查詢資料](/azure/sql-database/sql-database-connect-query-java)。</span><span class="sxs-lookup"><span data-stu-id="f8e14-107">To get started with Azure SQL Database, see [Azure SQL Database: Use Java to connect and query data](/azure/sql-database/sql-database-connect-query-java).</span></span>

## <a name="client-jdbc-driver"></a><span data-ttu-id="f8e14-108">用戶端 JDBC 驅動程式</span><span class="sxs-lookup"><span data-stu-id="f8e14-108">Client JDBC driver</span></span>

<span data-ttu-id="f8e14-109">使用 [SQL Database JDBC 驅動程式](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)從應用程式連線到 Azure SQL Database。</span><span class="sxs-lookup"><span data-stu-id="f8e14-109">Connect to Azure SQL Database from your applications using the [SQL Database JDBC driver](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server).</span></span> <span data-ttu-id="f8e14-110">您可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接與資料庫連線，或使用資料存取架構透過 JDBC (例如 [Hibernate](http://hibernate.org/)) 與資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="f8e14-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect with the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="f8e14-111">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端 JDBC 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="f8e14-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="f8e14-112">範例</span><span class="sxs-lookup"><span data-stu-id="f8e14-112">Example</span></span>

<span data-ttu-id="f8e14-113">使用 JDBC 連線到 SQL 資料庫並選取資料表中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="f8e14-113">Connect to SQL database and select all records in a table using JDBC.</span></span>

```java
String connectionString = "jdbc:sqlserver://fabrikam.database.windows.net:1433;database=fiber;user=raisa;password=testpass;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
try {
    Connection conn = DriverManager.getConnection(connectionString);
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery("SELECT * FROM SALES");
}  
```

## <a name="management-api"></a><span data-ttu-id="f8e14-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="f8e14-114">Management API</span></span>

<span data-ttu-id="f8e14-115">使用管理 API 在訂用帳戶中建立和管理 Azure SQL Database 資源。</span><span class="sxs-lookup"><span data-stu-id="f8e14-115">Create and manage Azure SQL Database resources in your subscription with the management API.</span></span>   

<span data-ttu-id="f8e14-116">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="f8e14-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-sql</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="f8e14-117">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="f8e14-117">Explore the Management APIs</span></span>](/java/api/overview/azure/sql/management)

### <a name="example"></a><span data-ttu-id="f8e14-118">範例</span><span class="sxs-lookup"><span data-stu-id="f8e14-118">Example</span></span>

<span data-ttu-id="f8e14-119">建立 SQL Database 資源，並使用防火牆規則限制只能存取某個 IP 位址範圍。</span><span class="sxs-lookup"><span data-stu-id="f8e14-119">Create a SQL Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

```java
SqlServer sqlServer = azure.sqlServers().define(sqlDbName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(resourceGroupName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    .withNewFirewallRule("172.16.0.0", "172.31.255.255")
                    .create();
```

## <a name="samples"></a><span data-ttu-id="f8e14-120">範例</span><span class="sxs-lookup"><span data-stu-id="f8e14-120">Samples</span></span>

[!INCLUDE [java-sql-samples](../docs-ref-conceptual/includes/sql.md)]

<span data-ttu-id="f8e14-121">深入探索可在應用程式中使用的 [Azure SQL Database Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL)。</span><span class="sxs-lookup"><span data-stu-id="f8e14-121">Explore more [sample Java code for Azure SQL Database](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL) you can use in your apps.</span></span>