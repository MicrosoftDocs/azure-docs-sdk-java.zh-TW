---
title: 適用於 Java 的 Azure Database for MySQL 程式庫
description: 適用於 Azure Database for MySQL 之 Java 用戶端程式庫的參考文件
keywords: Azure, Java, SDK, API, SQL, 資料庫, PostGres, MySQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: mysql
ms.openlocfilehash: 72c94ef4bdad7adeae63da2efda93b52a9afef53
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931014"
---
# <a name="azure-database-for-mysql-libraries-for-java"></a>適用於 Java 的 Azure Database for MySQL 程式庫

## <a name="overview"></a>概觀

[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) 是以開放原始碼 [MySQL](https://www.mysql.com/) 伺服器引擎為基礎的關聯式資料庫服務。 

若要開始使用 Azure Database for MySQL，請參閱[使用 Java 連線及查詢資料](/azure/mysql/connect-java)。

## <a name="client-jbdc-driver"></a>用戶端 JBDC 驅動程式

使用開放原始碼 [MySQL JDBC 驅動程式](https://dev.mysql.com/downloads/connector/j/)從應用程式連線到 Azure Database for MySQL。 您可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接連線到資料庫，或使用資料存取架構透過 JDBC (例如 [Hibernate](http://hibernate.org/)) 與資料庫互動。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端 JDBC 驅動程式。  

```XML
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.42</version>
</dependency>
```   

## <a name="example"></a>範例

使用 JDBC 連線到 Azure Database for MySQL 並選取銷售資料表中的所有記錄。 您可以從 Azure 入口網站取得資料庫的 JDBC 連接字串。

```java
String url = String.format("jdbc:mysql://fabrikamysql.mysql.database.azure.com:3306/fabrikamdb?verifyServerCertificate=true&useSSL=true&requireSSL=false");
try {
    Connection conn = DriverManager.getConnection(url, "frank@fabrikamysql", "aBcDeFgHiJkL");
    String selectSql = "SELECT * FROM SALES";
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a>範例

[建置 Java 和 MySQL Web 應用程式](/azure/app-service-web/app-service-web-tutorial-java-mysql)   
[使用 Azure CLI 設計 MySQL 資料庫](/azure/mysql/tutorial-design-database-using-cli)   

深入探索可在應用程式中使用的 [Azure Database for MySQL Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql)。
