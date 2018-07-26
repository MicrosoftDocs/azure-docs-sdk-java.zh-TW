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
ms.openlocfilehash: 37f7d3caf10e6b709cee2452c63a543d49e0ead8
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823711"
---
# <a name="azure-sql-database-libraries-for-java"></a>適用於 Java 的 Azure SQL Database 程式庫

## <a name="overview"></a>概觀

[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) 是使用 Microsoft SQL Server 引擎的關聯式資料庫服務，可支援資料表、JSON、空間和 XML 資料。 

若要開始使用 Azure SQL Database，請參閱 [Azure SQL Database：使用 Java 連線及查詢資料](/azure/sql-database/sql-database-connect-query-java)。

## <a name="client-jdbc-driver"></a>用戶端 JDBC 驅動程式

使用 [SQL Database JDBC 驅動程式](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)從應用程式連線到 Azure SQL Database。 您可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接與資料庫連線，或使用資料存取架構透過 JDBC (例如 [Hibernate](http://hibernate.org/)) 與資料庫互動。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端 JDBC 驅動程式。


```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```   

### <a name="example"></a>範例

使用 JDBC 連線到 SQL 資料庫並選取資料表中的所有記錄。

```java
String connectionString = "jdbc:sqlserver://fabrikam.database.windows.net:1433;database=fiber;user=raisa;password=testpass;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
try {
    Connection conn = DriverManager.getConnection(connectionString);
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery("SELECT * FROM SALES");
}  
```

## <a name="management-api"></a>管理 API

使用管理 API 在訂用帳戶中建立和管理 Azure SQL Database 資源。   

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-sql</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/sql/management)

### <a name="example"></a>範例

建立 SQL Database 資源，並使用防火牆規則限制只能存取某個 IP 位址範圍。

```java
SqlServer sqlServer = azure.sqlServers().define(sqlDbName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(resourceGroupName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    .withNewFirewallRule("172.16.0.0", "172.31.255.255")
                    .create();
```

## <a name="samples"></a>範例

[!INCLUDE [java-sql-samples](../docs-ref-conceptual/includes/sql.md)]

深入探索可在應用程式中使用的 [Azure SQL Database Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL)。