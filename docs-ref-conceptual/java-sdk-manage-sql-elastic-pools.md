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
# <a name="manage-azure-sql-databases-in-elastic-pools-from-your-java-applications"></a><span data-ttu-id="60ad1-103">從 Java 應用程式管理彈性集區中的 Azure SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="60ad1-103">Manage Azure SQL databases in elastic pools from your Java applications</span></span>

<span data-ttu-id="60ad1-104">[此範例](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)會使用[彈性集區](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)建立 SQL 資料庫伺服器，以在單一方案中對多個資料庫的資源進行管理和調整。</span><span class="sxs-lookup"><span data-stu-id="60ad1-104">[This sample](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool) creates a SQL database server with an [elastic pool](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) to manage and scale resources for mulitple databases in a single plan.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="60ad1-105">執行範例</span><span class="sxs-lookup"><span data-stu-id="60ad1-105">Run the sample</span></span>

<span data-ttu-id="60ad1-106">建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="60ad1-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="60ad1-107">然後，執行：</span><span class="sxs-lookup"><span data-stu-id="60ad1-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool
cd sql-database-java-manage-sql-dbs-in-elastic-pool
mvn clean compile exec:java
```

<span data-ttu-id="60ad1-108">檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)</span><span class="sxs-lookup"><span data-stu-id="60ad1-108">[View the complete code sample on GitHub](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="60ad1-109">使用 Azure 進行驗證</span><span class="sxs-lookup"><span data-stu-id="60ad1-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-sql-database-server-with-an-elastic-pool"></a><span data-ttu-id="60ad1-110">使用彈性集區建立 SQL 資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="60ad1-110">Create a SQL database server with an elastic pool</span></span>

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

<span data-ttu-id="60ad1-111">請參閱 [ElasticPoolEditions 類別參考](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions)以取得目前版本的值。</span><span class="sxs-lookup"><span data-stu-id="60ad1-111">See the [ElasticPoolEditions class reference](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) for current edition values.</span></span> <span data-ttu-id="60ad1-112">檢閱 [SQL 資料庫彈性集區文件](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)，以比較版本資源特性。</span><span class="sxs-lookup"><span data-stu-id="60ad1-112">Review the [SQL database elastic pool documentation](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) to compare edition resource characteristics.</span></span> 

## <a name="change-database-transaction-unit-dtu-settings-in-an-elastic-pool"></a><span data-ttu-id="60ad1-113">在彈性集區中變更資料庫交易單位 (DTU) 設定</span><span class="sxs-lookup"><span data-stu-id="60ad1-113">Change Database Transaction Unit (DTU) settings in an elastic pool</span></span>

```java
// Set an pool of 200 eDTUs to share between the databases
// and ensure that a database always has 10 DTUs available but never uses more than 50
elasticPool = elasticPool.update()
                    .withDtu(200)
                    .withDatabaseDtuMin(10)
                    .withDatabaseDtuMax(50)
                    .apply();
```

<span data-ttu-id="60ad1-114">檢閱 [DTU 和 eDTU 文件](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu)，以深入了解如何對 SQL 資料庫配置資源。</span><span class="sxs-lookup"><span data-stu-id="60ad1-114">Review the [DTUs and eDTUs documentation](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu) to learn more about allocating resources to SQL databases.</span></span>

## <a name="create-a-new-database-and-add-it-to-an-elastic-pool"></a><span data-ttu-id="60ad1-115">建立新的資料庫並將其新增至彈性集區</span><span class="sxs-lookup"><span data-stu-id="60ad1-115">Create a new database and add it to an elastic pool</span></span>

```java
// Add a newly created database to the elastic pool
SqlDatabase anotherDatabase = sqlServer.databases().define(anotherDatabaseName).create();
elasticPool.update().withExistingDatabase(anotherDatabase).apply();            
```

<span data-ttu-id="60ad1-116">在第一個陳述式中，API 會在 [S0 層](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)建立 `anotherDatabase`。</span><span class="sxs-lookup"><span data-stu-id="60ad1-116">The API creates `anotherDatabase` at [S0 tier](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) in the first statement.</span></span> <span data-ttu-id="60ad1-117">將 `anotherDatabase` 移到彈性集區時，系統會根據集區設定指派資料庫資源。</span><span class="sxs-lookup"><span data-stu-id="60ad1-117">Moving `anotherDatabase` to the elastic pool assigns the database resources based on the pool settings.</span></span>

## <a name="remove-a-database-from-an-elastic-pool"></a><span data-ttu-id="60ad1-118">從彈性集區移除資料庫</span><span class="sxs-lookup"><span data-stu-id="60ad1-118">Remove a database from an elastic pool</span></span>
```java
// Assign an edition since the database resources are no longer managed in the pool 
anotherDatabase = anotherDatabase.update()
                     .withoutElasticPool()
                     .withEdition(DatabaseEditions.STANDARD)
                     .apply();
```

<span data-ttu-id="60ad1-119">請參閱 [DatabaseEditions 類別參考](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions)，以獲得要傳遞至 `withEdition()` 的值。</span><span class="sxs-lookup"><span data-stu-id="60ad1-119">See the [DatabaseEditions class reference](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) for values to pass to `withEdition()`.</span></span>

## <a name="list-current-database-activities-in-an-elastic-pool"></a><span data-ttu-id="60ad1-120">列出彈性集區中目前的資料庫活動</span><span class="sxs-lookup"><span data-stu-id="60ad1-120">List current database activities in an elastic pool</span></span>
```java
// get a list of in-flight elastic operations on databases in the pool and log them 
for (ElasticPoolDatabaseActivity databaseActivity : elasticPool.listDatabaseActivities()) {
    System.out.println("Database " + databaseActivity.databaseName() 
        + " performing operation " + databaseActivity.operation() + 
        " with objective " + databaseActivity.requestedServiceObjective());
}
```

<span data-ttu-id="60ad1-121">資料庫活動包括在彈性集區中移入或移出資料庫，以及在彈性集區中更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="60ad1-121">Database activities include moving a database in or out of an elastic pool and updating databases in an elastic pool.</span></span>


## <a name="list-databases-in-an-elastic-pool"></a><span data-ttu-id="60ad1-122">列出彈性集區中的資料庫</span><span class="sxs-lookup"><span data-stu-id="60ad1-122">List databases in an elastic pool</span></span>
```java
// Log a list of databases in the elastic pool 
for (SqlDatabase databaseInServer : elasticPool.listDatabases()) {
    System.out.println(databaseInServer.name());
}
```

<span data-ttu-id="60ad1-123">檢閱 [com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) 中的方法，以對資料庫進行更詳細的查詢。</span><span class="sxs-lookup"><span data-stu-id="60ad1-123">Review the methods in [com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) to query the databases in more detail.</span></span>

## <a name="delete-an-elastic-pool"></a><span data-ttu-id="60ad1-124">刪除彈性集區</span><span class="sxs-lookup"><span data-stu-id="60ad1-124">Delete an elastic pool</span></span>
```java
sqlServer.elasticPools().delete(elasticPoolName);
```

<span data-ttu-id="60ad1-125">您必須先將彈性集區清空，才能加以刪除。</span><span class="sxs-lookup"><span data-stu-id="60ad1-125">The elastic pool must be empty before deleting it.</span></span>

## <a name="sample-code-summary"></a><span data-ttu-id="60ad1-126">程式碼範例摘要。</span><span class="sxs-lookup"><span data-stu-id="60ad1-126">Sample code summary.</span></span>

<span data-ttu-id="60ad1-127">此範例會建立 SQL Server，且其中具有兩個在單一彈性集區中受到管理的資料庫。</span><span class="sxs-lookup"><span data-stu-id="60ad1-127">The sample creates a SQL server with two databases managed in a single elasic pool.</span></span> <span data-ttu-id="60ad1-128">彈性集區資源的限制會加以更新，然後在集區中新增其他資料庫。</span><span class="sxs-lookup"><span data-stu-id="60ad1-128">The elastic pool resource limits are updated, then additional databases are added to the pool.</span></span> <span data-ttu-id="60ad1-129">程式碼接著會讀取彈性集區的組態和狀態。</span><span class="sxs-lookup"><span data-stu-id="60ad1-129">The code then reads the elastic pool's configuration and state.</span></span> 

<span data-ttu-id="60ad1-130">此範例會刪除它所建立的所有資源，然後才結束。</span><span class="sxs-lookup"><span data-stu-id="60ad1-130">The sample deletes all resources it created before exiting.</span></span>

| <span data-ttu-id="60ad1-131">範例中使用的類別</span><span class="sxs-lookup"><span data-stu-id="60ad1-131">Class used in sample</span></span> | <span data-ttu-id="60ad1-132">注意事項</span><span class="sxs-lookup"><span data-stu-id="60ad1-132">Notes</span></span> |
|-------|-------|
| [<span data-ttu-id="60ad1-133">SqlServer</span><span class="sxs-lookup"><span data-stu-id="60ad1-133">SqlServer</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_server) | <span data-ttu-id="60ad1-134">Azure 中由 `azure.sqlServers().define()...create()` Fluent 鏈結所建立的 SQL DB 伺服器。</span><span class="sxs-lookup"><span data-stu-id="60ad1-134">SQL DB server in Azure created by `azure.sqlServers().define()...create()` fluent chain.</span></span> <span data-ttu-id="60ad1-135">提供方法來建立和使用彈性集區與資料庫。</span><span class="sxs-lookup"><span data-stu-id="60ad1-135">Provides methods to create and work with elastic pools and databases.</span></span> 
| [<span data-ttu-id="60ad1-136">SqlDatabase</span><span class="sxs-lookup"><span data-stu-id="60ad1-136">SqlDatabase</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) | <span data-ttu-id="60ad1-137">代表 SQL 資料庫的用戶端物件。</span><span class="sxs-lookup"><span data-stu-id="60ad1-137">Client side object representing a SQL database.</span></span> <span data-ttu-id="60ad1-138">透過 `sqlServer().define()...create()` 所建立。</span><span class="sxs-lookup"><span data-stu-id="60ad1-138">Created through `sqlServer().define()...create()`.</span></span> 
| [<span data-ttu-id="60ad1-139">DatabaseEditions</span><span class="sxs-lookup"><span data-stu-id="60ad1-139">DatabaseEditions</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) | <span data-ttu-id="60ad1-140">在彈性集區外建立資料庫時或將資料庫移出彈性集區時，用來設定資料庫資源的常數靜態欄位</span><span class="sxs-lookup"><span data-stu-id="60ad1-140">Constant static fields used to set database resources when creating a database outside of an elastic pool or when moving a database out of an elastic pool</span></span>  
| [<span data-ttu-id="60ad1-141">SqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="60ad1-141">SqlElasticPool</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_elastic_pool) | <span data-ttu-id="60ad1-142">透過 Fluent 鏈結的 `withNewElasticPool()` 區段所建立，並且會在 Azure 中建立 SqlServer。</span><span class="sxs-lookup"><span data-stu-id="60ad1-142">Created from the `withNewElasticPool()` section of the fluent chain that created the SqlServer in Azure.</span></span> <span data-ttu-id="60ad1-143">提供方法來針對執行於彈性集區的資料庫以及彈性集區本身設定資源限制。</span><span class="sxs-lookup"><span data-stu-id="60ad1-143">Provides methods to set resource limits for databases running in the elastic pool and for the elastic pool itself.</span></span> 
| [<span data-ttu-id="60ad1-144">ElasticPoolEditions</span><span class="sxs-lookup"><span data-stu-id="60ad1-144">ElasticPoolEditions</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) | <span data-ttu-id="60ad1-145">會定義可供彈性集區使用之資源的常數欄位類別。</span><span class="sxs-lookup"><span data-stu-id="60ad1-145">Class of constant fields defining the resources available to an elastic pool.</span></span> <span data-ttu-id="60ad1-146">如需層級詳細資料，請參閱 [SQL 資料庫彈性集區文件](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)。</span><span class="sxs-lookup"><span data-stu-id="60ad1-146">See [SQL database elastic pool documentation](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) for tier details.</span></span> 
| [<span data-ttu-id="60ad1-147">ElasticPoolDatabaseActivity</span><span class="sxs-lookup"><span data-stu-id="60ad1-147">ElasticPoolDatabaseActivity</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_database_activity) | <span data-ttu-id="60ad1-148">擷取自 `SqlElasticPool.listDatabaseActivities()`。</span><span class="sxs-lookup"><span data-stu-id="60ad1-148">Retreived from `SqlElasticPool.listDatabaseActivities()`.</span></span> <span data-ttu-id="60ad1-149">此類型的每個物件皆代表一個在彈性集區的資料庫上執行的活動。</span><span class="sxs-lookup"><span data-stu-id="60ad1-149">Each object of this type represents an activity performed on a database in the elastic pool.</span></span>
| [<span data-ttu-id="60ad1-150">ElasticPoolActivity</span><span class="sxs-lookup"><span data-stu-id="60ad1-150">ElasticPoolActivity</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_activity) | <span data-ttu-id="60ad1-151">從 `SqlElasticPool.listActivities()` 的清單中來擷取。</span><span class="sxs-lookup"><span data-stu-id="60ad1-151">Retrieved in a List from `SqlElasticPool.listActivities()`.</span></span> <span data-ttu-id="60ad1-152">此清單中的每個物件皆代表一個在彈性集區上 (而非彈性集區的資料庫上) 執行的活動。</span><span class="sxs-lookup"><span data-stu-id="60ad1-152">Each of object in the list represents an activity performed on the elastic pool (not the databases in the elastic pool).</span></span>

## <a name="next-steps"></a><span data-ttu-id="60ad1-153">後續步驟</span><span class="sxs-lookup"><span data-stu-id="60ad1-153">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]