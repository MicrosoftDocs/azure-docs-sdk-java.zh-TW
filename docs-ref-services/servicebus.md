---
title: 適用於 Java 的服務匯流排程式庫
description: 適用於服務匯流排之 Java 用戶端和管理程式庫的參考文件
keywords: Azure, Java, SDK, API, 傳訊, amqp, qpid, JMS, pubsub, pub-sub, 訊息代理程式
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: service-bus
ms.openlocfilehash: ed830b4f7ffa104174205f75ea2923235029ea80
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892779"
---
# <a name="service-bus-libraries-for-java"></a><span data-ttu-id="c012c-104">適用於 Java 的服務匯流排程式庫</span><span class="sxs-lookup"><span data-stu-id="c012c-104">Service Bus libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="c012c-105">概觀</span><span class="sxs-lookup"><span data-stu-id="c012c-105">Overview</span></span>

<span data-ttu-id="c012c-106">服務匯流排是企業級的交易傳訊平台服務，可透過深入的特色功能提供高度可靠的佇列和發佈/訂閱主題，這些功能包括排序的傳遞、工作階段、資料分割、排程、複雜的訂用帳戶，以及工作流程及交易處理。</span><span class="sxs-lookup"><span data-stu-id="c012c-106">Service Bus is an enterprise-class, transactional messaging platform service that provides highly reliable queues and publish/subscribe topics with deep feature capabilities such as ordered delivery, sessions, partitioning, scheduling, complex subscriptions, as well as workflow and transaction handling.</span></span>

<span data-ttu-id="c012c-107">您可以比較服務匯流排的特色功能，而且您會發現這些功能通常會超過舊式的高階內部部署訊息代理程式所能提供的功能。</span><span class="sxs-lookup"><span data-stu-id="c012c-107">The Service Bus feature capabilities are comparable and often exceed those of high-end, on-premises legacy message brokers.</span></span> <span data-ttu-id="c012c-108">服務匯流排功能可透過標準型通訊協定 (如 AMQP 1.0 和 HTTPS) 來獲得，而且所有的通訊協定手勢都會有完整的記載，以提供您廣泛的互通性。</span><span class="sxs-lookup"><span data-stu-id="c012c-108">The Service Bus features are available via standards-based protocols like AMQP 1.0 and HTTPS and all protocol gestures are fully documented, allowing for broad interoperability.</span></span> 

<span data-ttu-id="c012c-109">服務匯流排進階版將焦點放在高度可用且可靠耐用的傳訊技術，即使是具有大量本機資料中心的部署，也能提供具競爭力的輸送量效能，而且您也不必經歷硬體的選取和採購過程、不必規劃和執行部署，也沒有永無止盡的效能最佳化工作階段。</span><span class="sxs-lookup"><span data-stu-id="c012c-109">Focusing on highly available and reliable durable messaging, the Service Bus Premium provides competitive throughput performance even with substantial local datacenter deployments, but without hardware selection and acquisition processes, deployment planning and execution, and endless performance optimization sessions.</span></span> 

<span data-ttu-id="c012c-110">服務匯流排進階版是完全受控的供應項目，其專門保留了容量供每個租用戶使用，因此相較於商業內部部署代理程式，您只要以極低的整體成本，即可透過簡單的容量導向計價模式產生可預測的效能。</span><span class="sxs-lookup"><span data-stu-id="c012c-110">Service Bus Premium is a fully managed offering with dedicated capacity reserved for each tenant that yields predictable performance with a simple, capacity-oriented pricing model and at extremely lower overall cost than commercial on-premises brokers.</span></span> <span data-ttu-id="c012c-111">對於許多客戶來說，服務匯流排進階版可取代現今的專用內部部署傳訊叢集，即使連結的工作負載不是在雲端執行也沒關係。</span><span class="sxs-lookup"><span data-stu-id="c012c-111">For many customers, Service Bus Premium can replace dedicated on-premises messaging clusters today, even if the attached workloads do not run in the cloud.</span></span> 

<span data-ttu-id="c012c-112">請到[傳訊文件章節](https://docs.microsoft.com/azure/service-bus-messaging/)深入了解服務匯流排的概念</span><span class="sxs-lookup"><span data-stu-id="c012c-112">Learn more about Service Bus concepts [in the messaging documentation section](https://docs.microsoft.com/azure/service-bus-messaging/)</span></span> 

<span data-ttu-id="c012c-113">針對 Java 開發人員，服務匯流排會提供由 Microsoft 支援的原生 API，而且服務匯流排也可與符合 AMQP 1.0 標準的程式庫 (例如 Apache Qpid Proton 的 JMS 提供者) 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="c012c-113">For Java developers, Service Bus provides a Microsoft supported native API and Service Bus can also be used with AMQP 1.0 compliant libraries such as Apache Qpid Proton's JMS provider.</span></span>

## <a name="client-library"></a><span data-ttu-id="c012c-114">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="c012c-114">Client library</span></span>

<span data-ttu-id="c012c-115">官方版的服務匯流排用戶端可於 [GitHub 上的原始程式碼](https://github.com/azure/azure-service-bus-java)取得，至於二進位檔和封裝的來源則可於 [Maven 中心](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22)取得。</span><span class="sxs-lookup"><span data-stu-id="c012c-115">The official Service Bus client is available in [source code form on GitHub](https://github.com/azure/azure-service-bus-java) and binaries and packaged sources [are available on Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22).</span></span>

<span data-ttu-id="c012c-116">**[範例程式碼存放庫](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)包含以下範例：**</span><span class="sxs-lookup"><span data-stu-id="c012c-116">**The [sample code repository](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/) contains samples for:**</span></span>
* <span data-ttu-id="c012c-117">如何使用 [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java)</span><span class="sxs-lookup"><span data-stu-id="c012c-117">How to use the [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java)</span></span>
* <span data-ttu-id="c012c-118">如何使用 [TopicClient and SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java)</span><span class="sxs-lookup"><span data-stu-id="c012c-118">How to use the [TopicClient and SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java)</span></span>
* <span data-ttu-id="c012c-119">如何使用來自服務匯流排的 [MessageSender 和 MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) 訊息。</span><span class="sxs-lookup"><span data-stu-id="c012c-119">How to use [MessageSender and MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) messages from Service Bus.</span></span>

<span data-ttu-id="c012c-120">將相依性新增至 Maven 專案的 `pom.xml` 檔案，以在您自己的專案中使用程式庫。</span><span class="sxs-lookup"><span data-stu-id="c012c-120">Add a dependency to your Maven project's `pom.xml` file to use the library in your own project.</span></span> <span data-ttu-id="c012c-121">指定所需的版本。</span><span class="sxs-lookup"><span data-stu-id="c012c-121">Specify the version as desired.</span></span>

<span data-ttu-id="c012c-122">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="c012c-122">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-servicebus</artifactId>
    <version>1.0.0</version>
</dependency>
```

```java
public class BasicSendReceiveWithQueueClient {
    // Connection String for the namespace can be obtained from the Azure portal under the
    // 'Shared Access policies' section.
    private static final String connectionString = "{connection string}";
    private static final String queueName = "{queue name}";
    private static IQueueClient queueClient;
    private static int totalSend = 100;
    private static int totalReceived = 0;

    public static void main(String[] args) throws Exception {

        Log.log("Starting BasicSendReceiveWithQueueClient sample");

        // create client
        Log.log("Create queue client.");
        queueClient = new QueueClient(new ConnectionStringBuilder(connectionString, queueName), ReceiveMode.PeekLock);

        // send and receive
        queueClient.registerMessageHandler(new MessageHandler(queueClient), new MessageHandlerOptions(1, false, Duration.ofMinutes(1)));
        for (int i = 0; i < totalSend; i++) {
            int j = i;
            Log.log("Sending message #%d.", j);
            queueClient.sendAsync(new Message("" + i)).thenRunAsync(() -> { Log.log("Sent message #%d.", j);});
        }

        while(totalReceived != totalSend) {
            Thread.sleep(1000);
        }

        Log.log("Received all messages, exiting the sample.");
        Log.log("Closing queue client.");
        queueClient.close();
    }

    static class MessageHandler implements IMessageHandler {
        private IQueueClient client;

        public MessageHandler(IQueueClient client) {
            this.client = client;
        }

        @Override
        public CompletableFuture<Void> onMessageAsync(IMessage iMessage) {
            Log.log("Received message with sq#: %d and lock token: %s.", iMessage.getSequenceNumber(), iMessage.getLockToken());
            return this.client.completeAsync(iMessage.getLockToken()).thenRunAsync(() -> {
                Log.log("Completed message sq#: %d and locktoken: %s", iMessage.getSequenceNumber(), iMessage.getLockToken());
                totalReceived++;
            });
        }

        @Override
        public void notifyException(Throwable throwable, ExceptionPhase exceptionPhase) {
            Log.log(exceptionPhase + "-" + throwable.getMessage());
        }
    }
}
```

> [!div class="nextstepaction"]
> <span data-ttu-id="c012c-123">[探索用戶端 API](/java/api/overview/azure/servicebus/client)
> [在此尋找更多範例 (另請參閱上述資訊以取得詳細資料)](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)</span><span class="sxs-lookup"><span data-stu-id="c012c-123">[Explore the Client APIs](/java/api/overview/azure/servicebus/client)
[Find more examples here (See also above for more details)](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)</span></span>

## <a name="management-api"></a><span data-ttu-id="c012c-124">管理 API</span><span class="sxs-lookup"><span data-stu-id="c012c-124">Management API</span></span>

<span data-ttu-id="c012c-125">使用管理 API 建立和管理命名空間、主題、佇列和訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="c012c-125">Create and manage namespaces, topics, queues, and subscriptions with the management API.</span></span>

<span data-ttu-id="c012c-126">**請在此此尋找更多範例：**</span><span class="sxs-lookup"><span data-stu-id="c012c-126">**Please find some examples here:**</span></span>
* [<span data-ttu-id="c012c-127">管理服務匯流排佇列</span><span class="sxs-lookup"><span data-stu-id="c012c-127">Manage Service Bus queues</span></span>](https://github.com/Azure-Samples/service-bus-java-manage-queue-with-basic-features)
* [<span data-ttu-id="c012c-128">建立和訂閱服務匯流排主題</span><span class="sxs-lookup"><span data-stu-id="c012c-128">Create and subscribe to Service Bus topics</span></span>](https://github.com/Azure-Samples/service-bus-java-manage-publish-subscribe-with-basic-features)

<span data-ttu-id="c012c-129">**在專案中使用管理 API：**
\\</span><span class="sxs-lookup"><span data-stu-id="c012c-129">**Use the Management API in your project:**
\\</span></span>
<span data-ttu-id="c012c-130">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="c012c-130">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-servicebus</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="c012c-131">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="c012c-131">Explore the Management APIs</span></span>](/java/api/overview/azure/servicebus/management)

<span data-ttu-id="c012c-132">深入探索可在應用程式中使用的 [Azure 服務匯流排 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=bus)。</span><span class="sxs-lookup"><span data-stu-id="c012c-132">Explore more [sample Java code for Azure Service Bus](https://azure.microsoft.com/resources/samples/?platform=java&term=bus) you can use in your apps.</span></span>
