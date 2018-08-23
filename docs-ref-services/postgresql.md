---
title: 適用於 Java 的 Azure Database for PostgreSQL 程式庫
description: 適用於 Azure Database for PostgreSQL 之 Java 用戶端程式庫的參考文件
keywords: Azure, Java, SDK, API, SQL, 資料庫, PostGres, PostgreSQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.devlang: java
ms.service: postgresql
ms.openlocfilehash: 0f93d26a07de9acf72a46792793b573dba878fba
ms.sourcegitcommit: 1b22376e4ceb3d2f2734c8fc80823a44cc5fe8fb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2018
ms.locfileid: "42703378"
---
# <a name="azure-database-for-postgresql-libraries-for-java"></a>適用於 Java 的 Azure Database for PostgreSQL 程式庫

## <a name="overview"></a>概觀

[Azure Database for PostgreSQL](/azure/sql-database/sql-database-technical-overview) 是 Azure 中以開放原始碼 [PostgreSQL](https://www.postgresql.org/) 資料庫引擎的社群版為基礎的關聯式資料庫服務，目標對象是開發人員。

若要開始使用 Azure Database for PostgreSQL，請參閱[使用 Java 連線及查詢資料](/azure/postgresql/connect-java)。

## <a name="client-jdbc-driver"></a>用戶端 JDBC 驅動程式

使用開放原始碼 [PostgreSQL JDBC 驅動程式](https://jdbc.postgresql.org/)從應用程式連線到 Azure Database for PostgreSQL。 您可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接連線到資料庫，或使用資料存取架構透過 JDBC (例如 [Hibernate](http://hibernate.org/)) 與資料庫互動。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端 JDBC 驅動程式。  

```XML
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.1.1</version>
</dependency>
```   

## <a name="example"></a>範例

使用 JDBC 連線到 Azure Database for PostgreSQL 並選取銷售資料表中的所有記錄。 您可以從 Azure 入口網站取得資料庫的 JDBC 連接字串。

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

## <a name="samples"></a>範例

[使用 Azure CLI 設計 PostgreSQL 資料庫](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-cli) 

深入探索可在應用程式中使用的 [Azure Database for PostgreSQL Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres)。
