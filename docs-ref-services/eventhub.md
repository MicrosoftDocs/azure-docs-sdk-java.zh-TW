---
title: "適用於 Java 的 Azure 事件中樞程式庫"
description: "Java 事件中樞程式庫的參考文件"
keywords: "Azure, Java, SDK, API, 事件中樞, IoT, 串流處理"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: event-hub
ms.openlocfilehash: 8e5b032624862ffbef18c718abf4fa29359b3e67
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="azure-event-hub-libraries-for-java"></a>適用於 Java 的 Azure 事件中樞程式庫

## <a name="overview"></a>概觀

使用 [Azure 事件中樞](/azure/event-hubs/event-hubs-what-is-event-hubs)，從連線的 IoT 裝置和應用程式每秒收集和管理數百萬個事件。

若要開始使用 Azure 事件中樞，請參閱[使用 Java 從 Azure 事件中樞接收事件](/azure/event-hubs/event-hubs-java-get-started-receive-eph)。


## <a name="client-library"></a>用戶端程式庫

使用事件中樞用戶端程式庫，將事件從事件中樞傳送至 Azure 事件中樞並取用和處理事件。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>0.14.3</version>
</dependency>
```   

## <a name="example"></a>範例

將事件傳送到事件中樞。

```java
ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName,sasKeyName, sasKey);

byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);
EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
ehClient.sendSync(sendEvent);
```

> [!div class="nextstepaction"]
> [探索用戶端 API](/java/api/overview/azure/eventhub/clientlibrary)


## <a name="samples"></a>範例

[透過 JMS 寫入至事件中樞並從 Apache Storm 讀取][1]
[使用混合式 .NET/Java 拓撲從 EventHubs 進行讀取和寫入][2] 

[1]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[2]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

深入探索可在應用程式中使用的 [Azure 事件中樞 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=event)。

