---
title: 適用於 Java 的 Azure IoT 中樞程式庫
description: Java Azure IoT 中樞程式庫的參考文件
keywords: Azure, Java, SDK, API, 事件, IoT, 串流, 裝置, iot 中樞
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.technology: azure
ms.devlang: java
ms.service: iot-hub
ms.openlocfilehash: 497b2a72d851b8e43a48384c6f1a160e8a38cbe6
ms.sourcegitcommit: bb7286fad75a2bb43e6ce1a8f1b09e701147c9f9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/02/2018
ms.locfileid: "48047145"
---
# <a name="azure-iot-libraries-for-java"></a><span data-ttu-id="e4eb5-104">適用於 Java 的 Azure IoT 程式庫</span><span class="sxs-lookup"><span data-stu-id="e4eb5-104">Azure IoT libraries for Java</span></span>

<span data-ttu-id="e4eb5-105">使用 [Azure IoT 中樞](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub)來連線、監視及控制物聯網資產。</span><span class="sxs-lookup"><span data-stu-id="e4eb5-105">Connect, monitor, and control Internet of Things assets with [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub).</span></span>

<span data-ttu-id="e4eb5-106">若要開始使用 Azure IoT 中樞，請參閱[使用 Java 將您的裝置連線至 IoT 中樞](/azure/iot-hub/iot-hub-java-java-getstarted)。</span><span class="sxs-lookup"><span data-stu-id="e4eb5-106">To get started with Azure IoT Hub, see [Connect your device to your IoT hub using Java](/azure/iot-hub/iot-hub-java-java-getstarted).</span></span>

## <a name="iot-service-library"></a><span data-ttu-id="e4eb5-107">IoT 服務程式庫</span><span class="sxs-lookup"><span data-stu-id="e4eb5-107">IoT Service library</span></span>

<span data-ttu-id="e4eb5-108">使用 IoT 服務程式庫註冊裝置，並將訊息從雲端傳送至已註冊的裝置。</span><span class="sxs-lookup"><span data-stu-id="e4eb5-108">Register devices and send messages from the cloud to registered devices using the IoT Service library.</span></span>

<span data-ttu-id="e4eb5-109">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="e4eb5-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.6.23</version>
</dependency>
```   

## <a name="iot-device-library"></a><span data-ttu-id="e4eb5-110">Iot 裝置程式庫</span><span class="sxs-lookup"><span data-stu-id="e4eb5-110">Iot Device library</span></span>

<span data-ttu-id="e4eb5-111">使用 IoT 裝置程式庫將訊息傳送至雲端，並在裝置上接收訊息。</span><span class="sxs-lookup"><span data-stu-id="e4eb5-111">Send messages to the cloud and receive messages on devices using the IoT Device library.</span></span>

<span data-ttu-id="e4eb5-112">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="e4eb5-112">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.31</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e4eb5-113">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="e4eb5-113">Explore the Client APIs</span></span>](/java/api/overview/azure/iot/client)   

## <a name="example"></a><span data-ttu-id="e4eb5-114">範例</span><span class="sxs-lookup"><span data-stu-id="e4eb5-114">Example</span></span>

<span data-ttu-id="e4eb5-115">將訊息從 Azure IoT 中樞傳送至裝置。</span><span class="sxs-lookup"><span data-stu-id="e4eb5-115">Send a message from Azure IoT Hub to a device.</span></span>

```java
Message messageToSend = new Message(messageText);
messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
messageToSend.setMessageId(java.util.UUID.randomUUID().toString());

// set message properties
Map<String, String> propertiesToSend = new HashMap<String, String>();
propertiesToSend.put(messagePropertyKey,messagePropertyKey);
messageToSend.setProperties(propertiesToSend);

CompletableFuture<Void> future = serviceClient.sendAsync(deviceId, messageToSend);
try {
    future.get();
}
catch (ExecutionException e) {
    System.out.println("Exception : " + e.getMessage());
}
```


## <a name="samples"></a><span data-ttu-id="e4eb5-116">範例</span><span class="sxs-lookup"><span data-stu-id="e4eb5-116">Samples</span></span>

<span data-ttu-id="e4eb5-117">[IoT 裝置範例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)   </span><span class="sxs-lookup"><span data-stu-id="e4eb5-117">[IoT Device samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)   </span></span>  
[<span data-ttu-id="e4eb5-118">IoT 服務範例</span><span class="sxs-lookup"><span data-stu-id="e4eb5-118">IoT Service samples</span></span>](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples)

<span data-ttu-id="e4eb5-119">深入探索可在應用程式中使用的 [Azure IoT Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=iot)。</span><span class="sxs-lookup"><span data-stu-id="e4eb5-119">Explore more [sample Java code for Azure IoT](https://azure.microsoft.com/resources/samples/?platform=java&term=iot) you can use in your apps.</span></span>
