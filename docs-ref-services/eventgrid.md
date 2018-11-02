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
# <a name="azure-event-grid-libraries-for-java"></a>適用於 Java 的 Azure 事件方格程式庫

建置事件導向的應用程式會傾聽並回應來自 Azure 服務和自訂來源的事件，這些事件會使用由 Azure Event Grid 處理的簡單型事件。

[進一步了解 ](/azure/event-grid/overview)Azure Event Grid 的相關資訊，並參考 [Azure Blob 儲存體教學課程](/azure/storage/blobs/storage-blob-event-quickstart)開始使用。 

## <a name="client-sdk"></a>用戶端 SDK

使用 Azure 事件方格用戶端 SDK 建立事件、驗證並發佈至主題。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventgrid</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="publish-events"></a>發佈事件

下列程式碼會向 Azure 進行驗證，並將自訂類型的 `EventGridEvent` 事件 `List` (在本範例中為 `Contoso.Items.ItemsReceived`) 發佈至主題。 您可以從 Azure CLI 中擷取範例中使用的主題金鑰和端點位址：

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
> [探索事件方格用戶端 API](/java/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a>管理 SDK

透過事件方格管理 SDK 來建立、更新或刪除 Event Grid 執行個體、主題和訂用帳戶。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.16.0</version>
</dependency>
```   

下列範例會從 [EventGrid Java 範例](https://github.com/Azure-Samples/event-grid-java-publish-consume-events)為基礎，建立事件方格訂用帳戶

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
> [探索管理 API](/java/api/overview/azure/eventgrid/management)