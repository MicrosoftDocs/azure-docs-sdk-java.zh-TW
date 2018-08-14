---
title: 使用 Java 管理 SQL 資料庫彈性集區 | Microsoft Docs
description: 使用適用於 Java 的 Azure SDK 來建立及設定 Azure SQL 資料庫的程式碼範例
author: rloutlaw
manager: douge
ms.assetid: 9b461de8-46bc-4650-8e9e-59531f4e2a53
ms.topic: article
ms.service: Azure
ms.devlang: java
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 9ec0cf3083b8659fa850b798ca0ecf18b2757234
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931114"
---
# <a name="manage-azure-sql-databases-in-elastic-pools-from-your-java-applications"></a>從 Java 應用程式管理彈性集區中的 Azure SQL 資料庫

[此範例](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)會使用[彈性集區](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)建立 SQL 資料庫伺服器，以在單一方案中對多個資料庫的資源進行管理和調整。

## <a name="run-the-sample"></a>執行範例

建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。 然後，執行：

```
git clone https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool
cd sql-database-java-manage-sql-dbs-in-elastic-pool
mvn clean compile exec:java
```

檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)

## <a name="authenticate-with-azure"></a>使用 Azure 進行驗證

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-sql-database-server-with-an-elastic-pool"></a>使用彈性集區建立 SQL 資料庫伺服器

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    // Use ElasticPoolEditions.STANDARD as the edition
                    // and create two databases
                    .withNewElasticPool(elasticPoolName, ElasticPoolEditions.STANDARD, 
                        database1Name, database2Name)
                    .create();
```

請參閱 [ElasticPoolEditions 類別參考](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions)以取得目前版本的值。 檢閱 [SQL 資料庫彈性集區文件](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)，以比較版本資源特性。 

## <a name="change-database-transaction-unit-dtu-settings-in-an-elastic-pool"></a>在彈性集區中變更資料庫交易單位 (DTU) 設定

```java
// Set an pool of 200 eDTUs to share between the databases
// and ensure that a database always has 10 DTUs available but never uses more than 50
elasticPool = elasticPool.update()
                    .withDtu(200)
                    .withDatabaseDtuMin(10)
                    .withDatabaseDtuMax(50)
                    .apply();
```

檢閱 [DTU 和 eDTU 文件](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu)，以深入了解如何對 SQL 資料庫配置資源。

## <a name="create-a-new-database-and-add-it-to-an-elastic-pool"></a>建立新的資料庫並將其新增至彈性集區

```java
// Add a newly created database to the elastic pool
SqlDatabase anotherDatabase = sqlServer.databases().define(anotherDatabaseName).create();
elasticPool.update().withExistingDatabase(anotherDatabase).apply();            
```

在第一個陳述式中，API 會在 [S0 層](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)建立 `anotherDatabase`。 將 `anotherDatabase` 移到彈性集區時，系統會根據集區設定指派資料庫資源。

## <a name="remove-a-database-from-an-elastic-pool"></a>從彈性集區移除資料庫
```java
// Assign an edition since the database resources are no longer managed in the pool 
anotherDatabase = anotherDatabase.update()
                     .withoutElasticPool()
                     .withEdition(DatabaseEditions.STANDARD)
                     .apply();
```

請參閱 [DatabaseEditions 類別參考](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions)，以獲得要傳遞至 `withEdition()` 的值。

## <a name="list-current-database-activities-in-an-elastic-pool"></a>列出彈性集區中目前的資料庫活動
```java
// get a list of in-flight elastic operations on databases in the pool and log them 
for (ElasticPoolDatabaseActivity databaseActivity : elasticPool.listDatabaseActivities()) {
    System.out.println("Database " + databaseActivity.databaseName() 
        + " performing operation " + databaseActivity.operation() + 
        " with objective " + databaseActivity.requestedServiceObjective());
}
```

資料庫活動包括在彈性集區中移入或移出資料庫，以及在彈性集區中更新資料庫。


## <a name="list-databases-in-an-elastic-pool"></a>列出彈性集區中的資料庫
```java
// Log a list of databases in the elastic pool 
for (SqlDatabase databaseInServer : elasticPool.listDatabases()) {
    System.out.println(databaseInServer.name());
}
```

檢閱 [com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) 中的方法，以對資料庫進行更詳細的查詢。

## <a name="delete-an-elastic-pool"></a>刪除彈性集區
```java
sqlServer.elasticPools().delete(elasticPoolName);
```

您必須先將彈性集區清空，才能加以刪除。

## <a name="sample-code-summary"></a>程式碼範例摘要。

此範例會建立 SQL Server，且其中具有兩個在單一彈性集區中受到管理的資料庫。 彈性集區資源的限制會加以更新，然後在集區中新增其他資料庫。 程式碼接著會讀取彈性集區的組態和狀態。 

此範例會刪除它所建立的所有資源，然後才結束。

| 範例中使用的類別 | 注意事項 |
|-------|-------|
| [SqlServer](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_server) | Azure 中由 `azure.sqlServers().define()...create()` Fluent 鏈結所建立的 SQL DB 伺服器。 提供方法來建立和使用彈性集區與資料庫。 
| [SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) | 代表 SQL 資料庫的用戶端物件。 透過 `sqlServer().define()...create()` 所建立。 
| [DatabaseEditions](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) | 在彈性集區外建立資料庫時或將資料庫移出彈性集區時，用來設定資料庫資源的常數靜態欄位  
| [SqlElasticPool](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_elastic_pool) | 透過 Fluent 鏈結的 `withNewElasticPool()` 區段所建立，並且會在 Azure 中建立 SqlServer。 提供方法來針對執行於彈性集區的資料庫以及彈性集區本身設定資源限制。 
| [ElasticPoolEditions](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) | 會定義可供彈性集區使用之資源的常數欄位類別。 如需層級詳細資料，請參閱 [SQL 資料庫彈性集區文件](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)。 
| [ElasticPoolDatabaseActivity](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_database_activity) | 擷取自 `SqlElasticPool.listDatabaseActivities()`。 此類型的每個物件皆代表一個在彈性集區的資料庫上執行的活動。
| [ElasticPoolActivity](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_activity) | 從 `SqlElasticPool.listActivities()` 的清單中來擷取。 此清單中的每個物件皆代表一個在彈性集區上 (而非彈性集區的資料庫上) 執行的活動。

## <a name="next-steps"></a>後續步驟

[!INCLUDE [next-steps](includes/java-next-steps.md)]