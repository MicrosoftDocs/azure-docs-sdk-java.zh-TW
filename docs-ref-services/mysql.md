---
title: 適用於 Java 的 Azure Database for MySQL 程式庫
description: 適用於 Azure Database for MySQL 之 Java 用戶端程式庫的參考文件
keywords: Azure, Java, SDK, API, SQL, 資料庫, PostGres, MySQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.devlang: java
ms.service: mysql
ms.openlocfilehash: f3c0b84a8c6577e5a844c4084b3d9cfaf3a52323
ms.sourcegitcommit: 1b22376e4ceb3d2f2734c8fc80823a44cc5fe8fb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2018
ms.locfileid: "42703367"
---
# <a name="azure-database-for-mysql-libraries-for-java"></a><span data-ttu-id="d4abf-104">適用於 Java 的 Azure Database for MySQL 程式庫</span><span class="sxs-lookup"><span data-stu-id="d4abf-104">Azure Database for MySQL libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="d4abf-105">概觀</span><span class="sxs-lookup"><span data-stu-id="d4abf-105">Overview</span></span>

<span data-ttu-id="d4abf-106">[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) 是以開放原始碼 [MySQL](https://www.mysql.com/) 伺服器引擎為基礎的關聯式資料庫服務。</span><span class="sxs-lookup"><span data-stu-id="d4abf-106">[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) is a relational database service based on the open source [MySQL](https://www.mysql.com/) Server engine.</span></span> 

<span data-ttu-id="d4abf-107">若要開始使用 Azure Database for MySQL，請參閱[使用 Java 連線及查詢資料](/azure/mysql/connect-java)。</span><span class="sxs-lookup"><span data-stu-id="d4abf-107">To get started with Azure Database for MySQL, see [Use Java to connect and query data](/azure/mysql/connect-java).</span></span>

## <a name="client-jbdc-driver"></a><span data-ttu-id="d4abf-108">用戶端 JBDC 驅動程式</span><span class="sxs-lookup"><span data-stu-id="d4abf-108">Client JBDC driver</span></span>

<span data-ttu-id="d4abf-109">使用開放原始碼 [MySQL JDBC 驅動程式](https://dev.mysql.com/downloads/connector/j/)從應用程式連線到 Azure Database for MySQL。</span><span class="sxs-lookup"><span data-stu-id="d4abf-109">Connect to Azure Database for MySQL from your applications using the open-source [MySQL JDBC driver](https://dev.mysql.com/downloads/connector/j/).</span></span> <span data-ttu-id="d4abf-110">您可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接連線到資料庫，或使用資料存取架構透過 JDBC (例如 [Hibernate](http://hibernate.org/)) 與資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="d4abf-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect to the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="d4abf-111">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端 JDBC 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="d4abf-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>  

```XML
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.42</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="d4abf-112">範例</span><span class="sxs-lookup"><span data-stu-id="d4abf-112">Example</span></span>

<span data-ttu-id="d4abf-113">使用 JDBC 連線到 Azure Database for MySQL 並選取銷售資料表中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="d4abf-113">Connect to Azure Database for MySQL using JDBC and select all records in the sales table.</span></span> <span data-ttu-id="d4abf-114">您可以從 Azure 入口網站取得資料庫的 JDBC 連接字串。</span><span class="sxs-lookup"><span data-stu-id="d4abf-114">You can get the JDBC connection string for the database from the Azure Portal.</span></span>

```java
String url = String.format("jdbc:mysql://fabrikamysql.mysql.database.azure.com:3306/fabrikamdb?verifyServerCertificate=true&useSSL=true&requireSSL=false");
try {
    Connection conn = DriverManager.getConnection(url, "frank@fabrikamysql", "aBcDeFgHiJkL");
    String selectSql = "SELECT * FROM SALES";
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a><span data-ttu-id="d4abf-115">範例</span><span class="sxs-lookup"><span data-stu-id="d4abf-115">Samples</span></span>

<span data-ttu-id="d4abf-116">[建置 Java 和 MySQL Web 應用程式](/azure/app-service-web/app-service-web-tutorial-java-mysql) </span><span class="sxs-lookup"><span data-stu-id="d4abf-116">[Build a Java and MySQL web app](/azure/app-service-web/app-service-web-tutorial-java-mysql) </span></span>  
[<span data-ttu-id="d4abf-117">使用 Azure CLI 設計 MySQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="d4abf-117">Design a MySQL database using the Azure CLI</span></span>](/azure/mysql/tutorial-design-database-using-cli)   

<span data-ttu-id="d4abf-118">深入探索可在應用程式中使用的 [Azure Database for MySQL Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql)。</span><span class="sxs-lookup"><span data-stu-id="d4abf-118">Explore more [sample Java code for Azure Database for MySQL](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql) you can use in your apps.</span></span>
