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
# <a name="azure-event-hub-libraries-for-java"></a><span data-ttu-id="9ff23-104">適用於 Java 的 Azure 事件中樞程式庫</span><span class="sxs-lookup"><span data-stu-id="9ff23-104">Azure Event Hub libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="9ff23-105">概觀</span><span class="sxs-lookup"><span data-stu-id="9ff23-105">Overview</span></span>

<span data-ttu-id="9ff23-106">使用 [Azure 事件中樞](/azure/event-hubs/event-hubs-what-is-event-hubs)，從連線的 IoT 裝置和應用程式每秒收集和管理數百萬個事件。</span><span class="sxs-lookup"><span data-stu-id="9ff23-106">Collect and manage millions of events per second from connected IoT devices and applications with [Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs).</span></span>

<span data-ttu-id="9ff23-107">若要開始使用 Azure 事件中樞，請參閱[使用 Java 從 Azure 事件中樞接收事件](/azure/event-hubs/event-hubs-java-get-started-receive-eph)。</span><span class="sxs-lookup"><span data-stu-id="9ff23-107">To get started with Azure Event Hubs, see [Receive events from Azure Event Hubs using Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).</span></span>


## <a name="client-library"></a><span data-ttu-id="9ff23-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="9ff23-108">Client library</span></span>

<span data-ttu-id="9ff23-109">使用事件中樞用戶端程式庫，將事件從事件中樞傳送至 Azure 事件中樞並取用和處理事件。</span><span class="sxs-lookup"><span data-stu-id="9ff23-109">Send events to an Azure Event Hub and consume and process events from an Event Hub using the Event Hubs client library.</span></span>

<span data-ttu-id="9ff23-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="9ff23-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>0.14.3</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="9ff23-111">範例</span><span class="sxs-lookup"><span data-stu-id="9ff23-111">Example</span></span>

<span data-ttu-id="9ff23-112">將事件傳送到事件中樞。</span><span class="sxs-lookup"><span data-stu-id="9ff23-112">Send an event to an event hub.</span></span>

```java
ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName,sasKeyName, sasKey);

byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);
EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
ehClient.sendSync(sendEvent);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="9ff23-113">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="9ff23-113">Explore the Client APIs</span></span>](/java/api/overview/azure/eventhub/clientlibrary)


## <a name="samples"></a><span data-ttu-id="9ff23-114">範例</span><span class="sxs-lookup"><span data-stu-id="9ff23-114">Samples</span></span>

<span data-ttu-id="9ff23-115">[透過 JMS 寫入至事件中樞並從 Apache Storm 讀取][1]
[使用混合式 .NET/Java 拓撲從 EventHubs 進行讀取和寫入][2]</span><span class="sxs-lookup"><span data-stu-id="9ff23-115">[Write to Event Hub via JMS and read from Apache Storm][1]
[Read and write from EventHubs using a hybrid .NET/Java topology][2]</span></span> 

[1]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[2]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

<span data-ttu-id="9ff23-116">深入探索可在應用程式中使用的 [Azure 事件中樞 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=event)。</span><span class="sxs-lookup"><span data-stu-id="9ff23-116">Explore more [sample Java code for Azure Event Hubs](https://azure.microsoft.com/resources/samples/?platform=java&term=event) you can use in your apps.</span></span>

