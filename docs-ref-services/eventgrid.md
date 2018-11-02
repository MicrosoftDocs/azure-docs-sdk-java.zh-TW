---
title: 適用於 Java 的 Azure 事件方格程式庫
description: Azure 事件方格 Java 程式庫的參考文件
keywords: Azure, Java, SDK, API, 事件方格, 傳訊, 事件驅動
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 07/11/2017
ms.topic: managed-reference
ms.devlang: java
ms.service: event-grid
ms.openlocfilehash: 35acbdeede2fbc541128029ad43f149209a057c4
ms.sourcegitcommit: db36eeb667a0bfe6d113953bb0583240db61b0f4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "50134624"
---
# <a name="azure-event-grid-libraries-for-java"></a><span data-ttu-id="336ce-104">適用於 Java 的 Azure 事件方格程式庫</span><span class="sxs-lookup"><span data-stu-id="336ce-104">Azure Event Grid libraries for Java</span></span>

<span data-ttu-id="336ce-105">建置事件導向的應用程式會傾聽並回應來自 Azure 服務和自訂來源的事件，這些事件會使用由 Azure Event Grid 處理的簡單型事件。</span><span class="sxs-lookup"><span data-stu-id="336ce-105">Build event-driven applications that listen and react to events from Azure services and custom sources using simple HTTP-based event handling with Azure Event Grid.</span></span>

<span data-ttu-id="336ce-106">[進一步了解 ](/azure/event-grid/overview)Azure Event Grid 的相關資訊，並參考 [Azure Blob 儲存體教學課程](/azure/storage/blobs/storage-blob-event-quickstart)開始使用。</span><span class="sxs-lookup"><span data-stu-id="336ce-106">[Learn more](/azure/event-grid/overview) about Azure Event Grid and get started with the [Azure Blob storage event tutorial](/azure/storage/blobs/storage-blob-event-quickstart).</span></span> 

## <a name="client-sdk"></a><span data-ttu-id="336ce-107">用戶端 SDK</span><span class="sxs-lookup"><span data-stu-id="336ce-107">Client SDK</span></span>

<span data-ttu-id="336ce-108">使用 Azure 事件方格用戶端 SDK 建立事件、驗證並發佈至主題。</span><span class="sxs-lookup"><span data-stu-id="336ce-108">Create events, authenticate, and post to topics using the Azure Event Grid Client SDK.</span></span>

<span data-ttu-id="336ce-109">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="336ce-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventgrid</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="publish-events"></a><span data-ttu-id="336ce-110">發佈事件</span><span class="sxs-lookup"><span data-stu-id="336ce-110">Publish events</span></span>

<span data-ttu-id="336ce-111">下列程式碼會向 Azure 進行驗證，並將自訂類型的 `EventGridEvent` 事件 `List` (在本範例中為 `Contoso.Items.ItemsReceived`) 發佈至主題。</span><span class="sxs-lookup"><span data-stu-id="336ce-111">The following code authenticates with Azure and publishes a `List` of  `EventGridEvent` events of a custom type (in this example, `Contoso.Items.ItemsReceived` ) to a topic.</span></span> <span data-ttu-id="336ce-112">您可以從 Azure CLI 中擷取範例中使用的主題金鑰和端點位址：</span><span class="sxs-lookup"><span data-stu-id="336ce-112">The topic key and endpoint address used in the sample can be retrieved from the Azure CLI:</span></span>

```azurecli-interactive
az eventgrid topic show -g gridResourceGroup -n topicName
az eventgrid topic key list -g gridResourceGroup -n topicName
```

```java
// Create an event grid client.
TopicCredentials topicCredentials = new TopicCredentials(System.getenv("EVENTGRID_TOPIC_KEY"));
EventGridClient client = new EventGridClientImpl(topicCredentials);

// Publish custom events to the EventGrid.
System.out.println("Publish custom events to the EventGrid");
List<EventGridEvent> eventsList = new ArrayList<>();
for (int i = 0; i < 5; i++) {
    eventsList.add(new EventGridEvent(
        UUID.randomUUID().toString(),
        String.format("Door%d", i),
        new ContosoItemReceivedEventData("Contoso Item SKU #1"),
        "Contoso.Items.ItemReceived",
        DateTime.now(),
        "2.0"
    ));
}

String eventGridEndpoint = String.format("https://%s/", new URI(System.getenv("EVENTGRID_TOPIC_ENDPOINT")).getHost());

client.publishEvents(eventGridEndpoint, eventsList);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="336ce-113">探索事件方格用戶端 API</span><span class="sxs-lookup"><span data-stu-id="336ce-113">Explore the Event Grid Client APIs</span></span>](/java/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a><span data-ttu-id="336ce-114">管理 SDK</span><span class="sxs-lookup"><span data-stu-id="336ce-114">Management SDK</span></span>

<span data-ttu-id="336ce-115">透過事件方格管理 SDK 來建立、更新或刪除 Event Grid 執行個體、主題和訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="336ce-115">Create, update, or delete Event Grid instances, topics, and subscriptions with the Event Grid management SDK.</span></span>

<span data-ttu-id="336ce-116">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="336ce-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.16.0</version>
</dependency>
```   

<span data-ttu-id="336ce-117">下列範例會從 [EventGrid Java 範例](https://github.com/Azure-Samples/event-grid-java-publish-consume-events)為基礎，建立事件方格訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="336ce-117">The following example creates an Event Grid subscription, taken from the [EventGrid Java samples](https://github.com/Azure-Samples/event-grid-java-publish-consume-events)</span></span>

```java
// Create an event grid subscription.
//
System.out.println("Creating an Azure EventGrid Subscription");

EventSubscription eventSubscription = eventGridManager.eventSubscriptions().define(eventSubscriptionName)
    .withScope(eventGridTopic.id())
    .withDestination(new EventHubEventSubscriptionDestination()
        .withResourceId(eventHub.id()))
    .withFilter(new EventSubscriptionFilter()
        .withIsSubjectCaseSensitive(false)
        .withSubjectBeginsWith("")
        .withSubjectEndsWith(""))
    .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="336ce-118">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="336ce-118">Explore the management APIs</span></span>](/java/api/overview/azure/eventgrid/management)