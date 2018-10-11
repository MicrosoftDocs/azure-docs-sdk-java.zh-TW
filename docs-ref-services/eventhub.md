---
title: 適用於 Java 的 Azure 事件中樞程式庫
description: Java 事件中樞程式庫的參考文件
keywords: Azure, Java, SDK, API, 事件中樞, IoT, 串流處理
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: event-hub
ms.openlocfilehash: b6646ef27edace4247090e749c9a52cd6a33a82c
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893469"
---
# <a name="azure-event-hub-libraries-for-java"></a><span data-ttu-id="8910a-104">適用於 Java 的 Azure 事件中樞程式庫</span><span class="sxs-lookup"><span data-stu-id="8910a-104">Azure Event Hub libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="8910a-105">概觀</span><span class="sxs-lookup"><span data-stu-id="8910a-105">Overview</span></span>

<span data-ttu-id="8910a-106">使用 [Azure 事件中樞](/azure/event-hubs/event-hubs-what-is-event-hubs)，從連線的 IoT 裝置和應用程式每秒收集和管理數百萬個事件。</span><span class="sxs-lookup"><span data-stu-id="8910a-106">Collect and manage millions of events per second from connected IoT devices and applications with [Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs).</span></span>

<span data-ttu-id="8910a-107">若要開始使用 Azure 事件中樞，請參閱[使用 Java 從 Azure 事件中樞接收事件](/azure/event-hubs/event-hubs-java-get-started-receive-eph)。</span><span class="sxs-lookup"><span data-stu-id="8910a-107">To get started with Azure Event Hubs, see [Receive events from Azure Event Hubs using Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).</span></span>


## <a name="client-library"></a><span data-ttu-id="8910a-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="8910a-108">Client library</span></span>

<span data-ttu-id="8910a-109">使用事件中樞用戶端程式庫，將事件從事件中樞傳送至 Azure 事件中樞並取用和處理事件。</span><span class="sxs-lookup"><span data-stu-id="8910a-109">Send events to an Azure Event Hub and consume and process events from an Event Hub using the Event Hubs client library.</span></span>

<span data-ttu-id="8910a-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用[用戶端程式庫](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventhubs)。</span><span class="sxs-lookup"><span data-stu-id="8910a-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the [client library](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventhubs) in your project.</span></span>
 

## <a name="example"></a><span data-ttu-id="8910a-111">範例</span><span class="sxs-lookup"><span data-stu-id="8910a-111">Example</span></span>

<span data-ttu-id="8910a-112">將事件傳送到事件中樞。</span><span class="sxs-lookup"><span data-stu-id="8910a-112">Send an event to an event hub.</span></span>

```java
final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
                                            .setNamespaceName(namespaceName)
                                            .setEventHubName(eventHubName)
                                            .setSasKeyName(sasKeyName)
                                            .setSasKey(sasKey);
final EventHubClient ehClient = EventHubClient.createSync(connStr.toString());

final byte[] payloadBytes = "Test AMQP message".getBytes("UTF-8");
final EventData sendEvent = new EventData(payloadBytes);

ehClient.sendSync(sendEvent);
```


> [!div class="nextstepaction"]
> [<span data-ttu-id="8910a-113">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="8910a-113">Explore the Client APIs</span></span>](/java/api/overview/azure/eventhubs/client)



## <a name="samples"></a><span data-ttu-id="8910a-114">範例</span><span class="sxs-lookup"><span data-stu-id="8910a-114">Samples</span></span>

<span data-ttu-id="8910a-115">[使用範例瀏覽事件中樞資料層 API][1]</span><span class="sxs-lookup"><span data-stu-id="8910a-115">[Explore Event Hub data-plane API using samples][1]</span></span>

<span data-ttu-id="8910a-116">[透過 JMS 寫入至事件中樞並從 Apache Storm 讀取][2]</span><span class="sxs-lookup"><span data-stu-id="8910a-116">[Write to Event Hub via JMS and read from Apache Storm][2]</span></span>

<span data-ttu-id="8910a-117">[使用混合式 .NET/Java 拓撲從 EventHubs 讀取和寫入][3]</span><span class="sxs-lookup"><span data-stu-id="8910a-117">[Read and write from EventHubs using a hybrid .NET/Java topology][3]</span></span> 

[1]: https://github.com/Azure/azure-event-hubs/tree/master/samples/Java
[2]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[3]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

<span data-ttu-id="8910a-118">深入探索可在應用程式中使用的 [Azure 事件中樞 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=event)。</span><span class="sxs-lookup"><span data-stu-id="8910a-118">Explore more [sample Java code for Azure Event Hubs](https://azure.microsoft.com/resources/samples/?platform=java&term=event) you can use in your apps.</span></span>

