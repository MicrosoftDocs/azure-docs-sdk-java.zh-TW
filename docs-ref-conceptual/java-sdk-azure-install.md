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
# <a name="azure-libraries-for-java"></a>適用於 Java 的 Azure 程式庫

Azure 程式庫可協助您使用原生介面在 Java 應用程式中取用 Azure 服務。 每個程式庫都是獨立的，因此可以與其他程式庫分開使用。

| | | | |
|:-------------:|:----------:|:----:|:---:|
| [Azure 儲存體](#azure-storage) | [SQL Database](#sql-database)  | [Redis 快取](#redis-cache)   | [Azure Cosmos DB](#cosmos-db) |
| [服務匯流排](#servicebus)  | [Azure Active Directory](#azuread) | [金鑰保存庫](#keyvault)  | [事件中樞](#eventhub)
| [IoT 服務](#iotservice) | [IoT 裝置](#iotdevice) | [Data Lake](#datalake)  | [AppInsights](#appinsights) | 
| [Batch](#batch) | [管理 Azure 資源](#management) |

## <a name="install-with-maven"></a>使用 Maven 進行安裝

在 `pom.xml` 中新增相依性項目，以將程式庫匯入 [Maven](https://maven.apache.org) 專案。

例如，若要納入[適用於 Java 的 Azure 管理程式庫](#management)的最新版本：

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

Gradle 等其他 Java 建置工具也有受到支援，但本文未提供其安裝步驟。 請檢閱建置工具的文件以了解如何取用 Maven 匯入。

## <a name="azure-service-libraries"></a>Azure 服務程式庫

整合 Azure 服務，以對使用這些程式庫的應用程式新增功能。 若要深入了解如何使用 Azure 服務來建置應用程式，請前往 [Java 開發人員中心](https://azure.microsoft.com/develop/java)。

<a name="azure-storage"></a>

### <a name="azure-storageazurestoragestorage-introduction"></a>[Azure 儲存體](/azure/storage/storage-introduction)  

應用程式的資料儲存和傳訊。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.4.0</version>
</dependency>
```   

[範例](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [參考](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [版本資訊](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)

<a name="sql-database"></a>

### <a name="sql-databaseazuresql-databasesql-database-technical-overview"></a>[SQL Database](/azure/sql-database/sql-database-technical-overview)

適用於 Azure SQL Database 的 JDBC 驅動程式。

```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

[範例](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [參考](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [版本資訊](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)

<a name="redis-cache"></a>

### <a name="redis-cachehttpsazuremicrosoftcomservicescache"></a>[Redis 快取](https://azure.microsoft.com/services/cache/)

低延遲、高效能的金鑰-值存放區。

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```   

[範例](/azure/redis-cache/cache-java-get-started) | [參考](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [版本資訊](https://github.com/xetorthio/jedis/releases)  

<a name="cosmos-db"></a>

### <a name="azure-cosmos-dbazurecosmos-dbintroduction"></a>[Azure Cosmos DB](/azure/cosmos-db/introduction)

可擴充的 NoSQL 資料庫，內含 JSON 文件和 SQL 或 JavaScript 查詢語法。   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

[範例](/azure/cosmos-db/sql-api-java-application) | [參考](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [版本資訊](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)

<a name="servicebus"></a>
 
 ### <a name="servicebusazureservice-bus-messagingservice-bus-messaging-overview"></a>[ServiceBus](/azure/service-bus-messaging/service-bus-messaging-overview) 
    
 服務匯流排是企業級的交易傳訊平台服務。
 
 ```XML
 <dependency> 
     <groupId>com.microsoft.azure</groupId> 
     <artifactId>azure-servicebus</artifactId> 
     <version>1.0.0</version> 
 </dependency>   
 ```
 
 [範例](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [參考](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [版本資訊](https://github.com/Azure/azure-service-bus-java)   
  
<a name="azuread"></a>

### <a name="azure-active-directoryazureactive-directoryactive-directory-whatis"></a>[Azure Active Directory](/azure/active-directory/active-directory-whatis)   

應用程式的身分識別管理和安全登入。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```
   
[範例](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [參考](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [版本資訊](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)
 
<a name="keyvault"></a>

### <a name="key-vaultazurekey-vault"></a>[金鑰保存庫](/azure/key-vault) 

從應用程式安全地存取金鑰和祕密。 

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```

[範例](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [參考](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [版本資訊](https://github.com/Azure/azure-keyvault-java) 

<a name="eventhub"></a>

### <a name="event-hubazureevent-hubsevent-hubs-what-is-event-hubs"></a>[事件中樞](/azure/event-hubs/event-hubs-what-is-event-hubs) 
   
適用於檢測或 IoT 案例的高輸送量事件和遙測處理。

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-eventhubs</artifactId> 
    <version>0.14.4</version> 
</dependency>   
```

[範例](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [參考](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [版本資訊](https://github.com/Azure/azure-event-hubs-java)

<a name="iotservice"></a> 

### <a name="iot-serviceazureiot-hub"></a>[IoT 服務](/azure/iot-hub/)

管理身分識別、傳送訊息，以及從已向 IoT 中樞註冊的裝置取得意見反應。

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.7.23</version>
</dependency>
```   
   
[範例](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [參考](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [版本資訊](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="iotdevice"></a> 

### <a name="iot-deviceazureiot-hubiot-hub-devguide"></a>[IoT 裝置](/azure/iot-hub/iot-hub-devguide)

從裝置將訊息傳送到 IoT 中樞。  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.32</version>
</dependency>
```  

[範例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [參考](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [版本資訊](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="datalake"></a> 

### <a name="data-lake-storeazuredata-lake-storedata-lake-store-overview"></a>[Data Lake Store](/azure/data-lake-store/data-lake-store-overview)   
   
將任何大小的資料和圖形擷取到單一位置以進行分析。    

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

[範例](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [參考](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [版本資訊](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)

<a name="appinsights"></a> 

### <a name="appinsightsazureapplication-insightsapp-insights-overview"></a>[AppInsights](/azure/application-insights/app-insights-overview)

追蹤使用狀況、新增遙測並監視 Web 應用程式。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>
    <version>1.0.8</version>
</dependency>
```

[範例](/azure/application-insights/app-insights-java-get-started) | [參考](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [版本資訊](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)

<a name="batch"></a>

### <a name="batchazurebatch"></a>[Batch](/azure/batch)

在雲端有效地執行大規模的平行和高效能計算應用程式。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```

[範例](https://github.com/azure/azure-batch-samples) | [參考](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [版本資訊](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)

<a name="management"></a> 

## <a name="manage-azure-resources"></a>管理 Azure 資源

從應用程式程式碼建立、更新和刪除 Azure 資源。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

[範例](https://github.com/Azure/azure-sdk-for-java#sample-code) | [參考](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [版本資訊](java-sdk-azure-release-notes.md)

<a name="servicebus"></a>

### <a name="servicebushttpsdocsmicrosoftcomazureservice-bus-messagingservice-bus-messaging-overview"></a>[ServiceBus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) 
   
服務匯流排是企業級的交易傳訊平台服務。

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-servicebus</artifactId> 
    <version>1.0.0</version> 
</dependency>   
```

[範例](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [參考](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [版本資訊](https://github.com/Azure/azure-service-bus-java)

