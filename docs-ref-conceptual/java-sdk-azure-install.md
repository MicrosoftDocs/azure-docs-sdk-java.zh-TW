---
title: 適用於 Java 開發人員的 Azure Microsoft Docs
description: 適用於 Azure 的 Java SDK 和 API 參考
keywords: Azure Java, Azure Java API 參考, Azure Java 類別庫, Azure SDK
author: routlaw
manager: douge
ms.assetid: 7b92e776-959b-4632-8b1d-047ce1417616
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: 5c8bb4b81080461285551573eefc0d76b47b2d3d
ms.sourcegitcommit: 61030d025614b084e897809e603b2ec79900ec8d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2018
ms.locfileid: "30302546"
---
# <a name="azure-libraries-for-java"></a><span data-ttu-id="1b857-104">適用於 Java 的 Azure 程式庫</span><span class="sxs-lookup"><span data-stu-id="1b857-104">Azure libraries for Java</span></span>

<span data-ttu-id="1b857-105">Azure 程式庫可協助您使用原生介面在 Java 應用程式中取用 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="1b857-105">Azure libraries help you consume Azure services in your Java apps using native interfaces.</span></span> <span data-ttu-id="1b857-106">每個程式庫都是獨立的，因此可以與其他程式庫分開使用。</span><span class="sxs-lookup"><span data-stu-id="1b857-106">Each library is independent and can be used separately from the others another.</span></span>

| | | | |
|:-------------:|:----------:|:----:|:---:|
| [<span data-ttu-id="1b857-107">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="1b857-107">Azure Storage</span></span>](#azure-storage) | [<span data-ttu-id="1b857-108">SQL Database</span><span class="sxs-lookup"><span data-stu-id="1b857-108">SQL Database</span></span>](#sql-database)  | [<span data-ttu-id="1b857-109">Redis 快取</span><span class="sxs-lookup"><span data-stu-id="1b857-109">Redis Cache</span></span>](#redis-cache)   | [<span data-ttu-id="1b857-110">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1b857-110">Azure Cosmos DB</span></span>](#cosmos-db) |
| [<span data-ttu-id="1b857-111">服務匯流排</span><span class="sxs-lookup"><span data-stu-id="1b857-111">Service Bus</span></span>](#servicebus)  | [<span data-ttu-id="1b857-112">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1b857-112">Azure Active Directory</span></span>](#azuread) | [<span data-ttu-id="1b857-113">金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="1b857-113">Key Vault</span></span>](#keyvault)  | [<span data-ttu-id="1b857-114">事件中樞</span><span class="sxs-lookup"><span data-stu-id="1b857-114">Event Hub</span></span>](#eventhub)
| [<span data-ttu-id="1b857-115">IoT 服務</span><span class="sxs-lookup"><span data-stu-id="1b857-115">IoT Service</span></span>](#iotservice) | [<span data-ttu-id="1b857-116">IoT 裝置</span><span class="sxs-lookup"><span data-stu-id="1b857-116">IoT Device</span></span>](#iotdevice) | [<span data-ttu-id="1b857-117">Data Lake</span><span class="sxs-lookup"><span data-stu-id="1b857-117">Data Lake</span></span>](#datalake)  | [<span data-ttu-id="1b857-118">AppInsights</span><span class="sxs-lookup"><span data-stu-id="1b857-118">AppInsights</span></span>](#appinsights) | 
| [<span data-ttu-id="1b857-119">Batch</span><span class="sxs-lookup"><span data-stu-id="1b857-119">Batch</span></span>](#batch) | [<span data-ttu-id="1b857-120">管理 Azure 資源</span><span class="sxs-lookup"><span data-stu-id="1b857-120">Manage Azure resources</span></span>](#management) |

## <a name="install-with-maven"></a><span data-ttu-id="1b857-121">使用 Maven 進行安裝</span><span class="sxs-lookup"><span data-stu-id="1b857-121">Install with Maven</span></span>

<span data-ttu-id="1b857-122">在 `pom.xml` 中新增相依性項目，以將程式庫匯入 [Maven](https://maven.apache.org) 專案。</span><span class="sxs-lookup"><span data-stu-id="1b857-122">Add a dependency entry in your `pom.xml` to import a library into your [Maven](https://maven.apache.org) project.</span></span>

<span data-ttu-id="1b857-123">例如，若要納入[適用於 Java 的 Azure 管理程式庫](#management)的最新版本：</span><span class="sxs-lookup"><span data-stu-id="1b857-123">For example, to include the latest version of the [Azure management libraries for Java](#management):</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

<span data-ttu-id="1b857-124">Gradle 等其他 Java 建置工具也有受到支援，但本文未提供其安裝步驟。</span><span class="sxs-lookup"><span data-stu-id="1b857-124">Other Java build tools like Gradle are supported but the install steps are not provided in this article.</span></span> <span data-ttu-id="1b857-125">請檢閱建置工具的文件以了解如何取用 Maven 匯入。</span><span class="sxs-lookup"><span data-stu-id="1b857-125">Review the documentation for your build tool on how to consume Maven imports.</span></span>

## <a name="azure-service-libraries"></a><span data-ttu-id="1b857-126">Azure 服務程式庫</span><span class="sxs-lookup"><span data-stu-id="1b857-126">Azure service libraries</span></span>

<span data-ttu-id="1b857-127">整合 Azure 服務，以對使用這些程式庫的應用程式新增功能。</span><span class="sxs-lookup"><span data-stu-id="1b857-127">Integrate Azure services to add functionality to your apps using these libraries.</span></span> <span data-ttu-id="1b857-128">若要深入了解如何使用 Azure 服務來建置應用程式，請前往 [Java 開發人員中心](https://azure.microsoft.com/develop/java)。</span><span class="sxs-lookup"><span data-stu-id="1b857-128">Learn more about building apps with Azure services at the [Java developer center](https://azure.microsoft.com/develop/java).</span></span>

<a name="azure-storage"></a>

### <a name="azure-storageazurestoragestorage-introduction"></a>[<span data-ttu-id="1b857-129">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="1b857-129">Azure Storage</span></span>](/azure/storage/storage-introduction)  

<span data-ttu-id="1b857-130">應用程式的資料儲存和傳訊。</span><span class="sxs-lookup"><span data-stu-id="1b857-130">Data storage and messaging for your applications.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.4.0</version>
</dependency>
```   

<span data-ttu-id="1b857-131">[範例](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [參考](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [版本資訊](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)</span><span class="sxs-lookup"><span data-stu-id="1b857-131">[Samples](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [Reference](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [Release Notes](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)</span></span>

<a name="sql-database"></a>

### <a name="sql-databaseazuresql-databasesql-database-technical-overview"></a>[<span data-ttu-id="1b857-132">SQL Database</span><span class="sxs-lookup"><span data-stu-id="1b857-132">SQL Database</span></span>](/azure/sql-database/sql-database-technical-overview)

<span data-ttu-id="1b857-133">適用於 Azure SQL Database 的 JDBC 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="1b857-133">JDBC driver for Azure SQL Database.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

<span data-ttu-id="1b857-134">[範例](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [參考](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [版本資訊](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)</span><span class="sxs-lookup"><span data-stu-id="1b857-134">[Samples](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [Reference](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [Release Notes](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)</span></span>

<a name="redis-cache"></a>

### <a name="redis-cachehttpsazuremicrosoftcomservicescache"></a>[<span data-ttu-id="1b857-135">Redis 快取</span><span class="sxs-lookup"><span data-stu-id="1b857-135">Redis Cache</span></span>](https://azure.microsoft.com/services/cache/)

<span data-ttu-id="1b857-136">低延遲、高效能的金鑰-值存放區。</span><span class="sxs-lookup"><span data-stu-id="1b857-136">Low-latency, high-performance key-value store.</span></span>

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```   

<span data-ttu-id="1b857-137">[範例](/azure/redis-cache/cache-java-get-started) | [參考](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [版本資訊](https://github.com/xetorthio/jedis/releases)</span><span class="sxs-lookup"><span data-stu-id="1b857-137">[Samples](/azure/redis-cache/cache-java-get-started) | [Reference](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [Release Notes](https://github.com/xetorthio/jedis/releases)</span></span>  

<a name="cosmos-db"></a>

### <a name="azure-cosmos-dbazurecosmos-dbintroduction"></a>[<span data-ttu-id="1b857-138">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1b857-138">Azure Cosmos DB</span></span>](/azure/cosmos-db/introduction)

<span data-ttu-id="1b857-139">可擴充的 NoSQL 資料庫，內含 JSON 文件和 SQL 或 JavaScript 查詢語法。</span><span class="sxs-lookup"><span data-stu-id="1b857-139">Scalable NoSQL database with JSON documents and a SQL or JavaScript query syntax.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

<span data-ttu-id="1b857-140">[範例](/azure/cosmos-db/sql-api-java-application) | [參考](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [版本資訊](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)</span><span class="sxs-lookup"><span data-stu-id="1b857-140">[Samples](/azure/cosmos-db/sql-api-java-application) | [Reference](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [Release Notes](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)</span></span>

<a name="servicebus"></a>
 
 ### <a name="servicebusazureservice-bus-messagingservice-bus-messaging-overview"></a>[<span data-ttu-id="1b857-141">ServiceBus</span><span class="sxs-lookup"><span data-stu-id="1b857-141">ServiceBus</span></span>](/azure/service-bus-messaging/service-bus-messaging-overview) 
    
 <span data-ttu-id="1b857-142">服務匯流排是企業級的交易傳訊平台服務。</span><span class="sxs-lookup"><span data-stu-id="1b857-142">Service Bus is an enterprise-class, transactional messaging platform service.</span></span>
 
 ```XML
 <dependency> 
     <groupId>com.microsoft.azure</groupId> 
     <artifactId>azure-servicebus</artifactId> 
     <version>1.0.0</version> 
 </dependency>   
 ```
 
 <span data-ttu-id="1b857-143">[範例](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [參考](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [版本資訊](https://github.com/Azure/azure-service-bus-java)</span><span class="sxs-lookup"><span data-stu-id="1b857-143">[Samples](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Reference](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Release Notes](https://github.com/Azure/azure-service-bus-java)</span></span>   
  
<a name="azuread"></a>

### <a name="azure-active-directoryazureactive-directoryactive-directory-whatis"></a>[<span data-ttu-id="1b857-144">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1b857-144">Azure Active Directory</span></span>](/azure/active-directory/active-directory-whatis)   

<span data-ttu-id="1b857-145">應用程式的身分識別管理和安全登入。</span><span class="sxs-lookup"><span data-stu-id="1b857-145">Identity management and secure sign-in for your applications.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```
   
<span data-ttu-id="1b857-146">[範例](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [參考](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [版本資訊](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)</span><span class="sxs-lookup"><span data-stu-id="1b857-146">[Samples](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [Reference](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [Release Notes](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)</span></span>
 
<a name="keyvault"></a>

### <a name="key-vaultazurekey-vault"></a>[<span data-ttu-id="1b857-147">金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="1b857-147">Key Vault</span></span>](/azure/key-vault) 

<span data-ttu-id="1b857-148">從應用程式安全地存取金鑰和祕密。</span><span class="sxs-lookup"><span data-stu-id="1b857-148">Safely access keys and secrets from your applications.</span></span> 

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```

<span data-ttu-id="1b857-149">[範例](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [參考](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [版本資訊](https://github.com/Azure/azure-keyvault-java)</span><span class="sxs-lookup"><span data-stu-id="1b857-149">[Samples](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [Reference](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [Release Notes](https://github.com/Azure/azure-keyvault-java)</span></span> 

<a name="eventhub"></a>

### <a name="event-hubazureevent-hubsevent-hubs-what-is-event-hubs"></a>[<span data-ttu-id="1b857-150">事件中樞</span><span class="sxs-lookup"><span data-stu-id="1b857-150">Event Hub</span></span>](/azure/event-hubs/event-hubs-what-is-event-hubs) 
   
<span data-ttu-id="1b857-151">適用於檢測或 IoT 案例的高輸送量事件和遙測處理。</span><span class="sxs-lookup"><span data-stu-id="1b857-151">High-throughput event and telemetry handling for your instrumentation or IoT scenarios.</span></span>

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-eventhubs</artifactId> 
    <version>0.14.4</version> 
</dependency>   
```

<span data-ttu-id="1b857-152">[範例](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [參考](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [版本資訊](https://github.com/Azure/azure-event-hubs-java)</span><span class="sxs-lookup"><span data-stu-id="1b857-152">[Samples](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [Reference](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [Release Notes](https://github.com/Azure/azure-event-hubs-java)</span></span>

<a name="iotservice"></a> 

### <a name="iot-serviceazureiot-hub"></a>[<span data-ttu-id="1b857-153">IoT 服務</span><span class="sxs-lookup"><span data-stu-id="1b857-153">IoT Service</span></span>](/azure/iot-hub/)

<span data-ttu-id="1b857-154">管理身分識別、傳送訊息，以及從已向 IoT 中樞註冊的裝置取得意見反應。</span><span class="sxs-lookup"><span data-stu-id="1b857-154">Manage identities, send messages, and get feedback from devices registered with your IoT hub.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.7.23</version>
</dependency>
```   
   
<span data-ttu-id="1b857-155">[範例](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [參考](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [版本資訊](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)</span><span class="sxs-lookup"><span data-stu-id="1b857-155">[Samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [Reference](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Release Notes](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)</span></span>

<a name="iotdevice"></a> 

### <a name="iot-deviceazureiot-hubiot-hub-devguide"></a>[<span data-ttu-id="1b857-156">IoT 裝置</span><span class="sxs-lookup"><span data-stu-id="1b857-156">IoT Device</span></span>](/azure/iot-hub/iot-hub-devguide)

<span data-ttu-id="1b857-157">從裝置將訊息傳送到 IoT 中樞。</span><span class="sxs-lookup"><span data-stu-id="1b857-157">Send a message to an IoT hub from your device.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.32</version>
</dependency>
```  

<span data-ttu-id="1b857-158">[範例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [參考](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [版本資訊](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)</span><span class="sxs-lookup"><span data-stu-id="1b857-158">[Samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [Reference](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Release Notes](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)</span></span>

<a name="datalake"></a> 

### <a name="data-lake-storeazuredata-lake-storedata-lake-store-overview"></a>[<span data-ttu-id="1b857-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="1b857-159">Data Lake Store</span></span>](/azure/data-lake-store/data-lake-store-overview)   
   
<span data-ttu-id="1b857-160">將任何大小的資料和圖形擷取到單一位置以進行分析。</span><span class="sxs-lookup"><span data-stu-id="1b857-160">Capture data of any size and shape into a single location for performing analytics.</span></span>    

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

<span data-ttu-id="1b857-161">[範例](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [參考](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [版本資訊](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)</span><span class="sxs-lookup"><span data-stu-id="1b857-161">[Samples](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [Reference](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [Release Notes](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)</span></span>

<a name="appinsights"></a> 

### <a name="appinsightsazureapplication-insightsapp-insights-overview"></a>[<span data-ttu-id="1b857-162">AppInsights</span><span class="sxs-lookup"><span data-stu-id="1b857-162">AppInsights</span></span>](/azure/application-insights/app-insights-overview)

<span data-ttu-id="1b857-163">追蹤使用狀況、新增遙測並監視 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b857-163">Track usage, add telemetry, and monitor your web apps.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>
    <version>1.0.8</version>
</dependency>
```

<span data-ttu-id="1b857-164">[範例](/azure/application-insights/app-insights-java-get-started) | [參考](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [版本資訊](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)</span><span class="sxs-lookup"><span data-stu-id="1b857-164">[Samples](/azure/application-insights/app-insights-java-get-started) | [Reference](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [Release Notes](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)</span></span>

<a name="batch"></a>

### <a name="batchazurebatch"></a>[<span data-ttu-id="1b857-165">Batch</span><span class="sxs-lookup"><span data-stu-id="1b857-165">Batch</span></span>](/azure/batch)

<span data-ttu-id="1b857-166">在雲端有效地執行大規模的平行和高效能計算應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b857-166">Run large-scale parallel and high-performance computing applications efficiently in the cloud.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```

<span data-ttu-id="1b857-167">[範例](https://github.com/azure/azure-batch-samples) | [參考](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [版本資訊](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)</span><span class="sxs-lookup"><span data-stu-id="1b857-167">[Samples](https://github.com/azure/azure-batch-samples) | [Reference](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [Release Notes](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)</span></span>

<a name="management"></a> 

## <a name="manage-azure-resources"></a><span data-ttu-id="1b857-168">管理 Azure 資源</span><span class="sxs-lookup"><span data-stu-id="1b857-168">Manage Azure resources</span></span>

<span data-ttu-id="1b857-169">從應用程式程式碼建立、更新和刪除 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="1b857-169">Create, update, and delete Azure resources from your application code.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

<span data-ttu-id="1b857-170">[範例](https://github.com/Azure/azure-sdk-for-java#sample-code) | [參考](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [版本資訊](java-sdk-azure-release-notes.md)</span><span class="sxs-lookup"><span data-stu-id="1b857-170">[Samples](https://github.com/Azure/azure-sdk-for-java#sample-code) | [Reference](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [Release Notes](java-sdk-azure-release-notes.md)</span></span>

<a name="servicebus"></a>

### <a name="servicebushttpsdocsmicrosoftcomazureservice-bus-messagingservice-bus-messaging-overview"></a>[<span data-ttu-id="1b857-171">ServiceBus</span><span class="sxs-lookup"><span data-stu-id="1b857-171">ServiceBus</span></span>](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) 
   
<span data-ttu-id="1b857-172">服務匯流排是企業級的交易傳訊平台服務。</span><span class="sxs-lookup"><span data-stu-id="1b857-172">Service Bus is an enterprise-class, transactional messaging platform service.</span></span>

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-servicebus</artifactId> 
    <version>1.0.0</version> 
</dependency>   
```

<span data-ttu-id="1b857-173">[範例](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [參考](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [版本資訊](https://github.com/Azure/azure-service-bus-java)</span><span class="sxs-lookup"><span data-stu-id="1b857-173">[Samples](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Reference](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Release Notes](https://github.com/Azure/azure-service-bus-java)</span></span>

