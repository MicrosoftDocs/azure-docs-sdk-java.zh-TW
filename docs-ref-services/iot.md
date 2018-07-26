---
title: 適用於 Java 的 Azure IoT 中樞程式庫
description: Java Azure IoT 中樞程式庫的參考文件
keywords: Azure, Java, SDK, API, 事件, IoT, 串流, 裝置, iot 中樞
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: iot-hub
ms.openlocfilehash: 5e6a102b062b2fff6b297c7e3dda423d1448bcb0
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823611"
---
# <a name="azure-iot-libraries-for-java"></a>適用於 Java 的 Azure IoT 程式庫

使用 [Azure IoT 中樞](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub)來連線、監視及控制物聯網資產。

若要開始使用 Azure IoT 中樞，請參閱[使用 Java 將您的裝置連線至 IoT 中樞](/azure/iot-hub/iot-hub-java-java-getstarted)。

## <a name="iot-service-library"></a>IoT 服務程式庫

使用 IoT 服務程式庫註冊裝置，並將訊息從雲端傳送至已註冊的裝置。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.6.23</version>
</dependency>
```   

## <a name="iot-device-library"></a>Iot 裝置程式庫

使用 IoT 裝置程式庫將訊息傳送至雲端，並在裝置上接收訊息。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.31</version>
</dependency>
```

> [!div class="nextstepaction"]
> [探索用戶端 API](/java/api/overview/azure/iot/client)   

## <a name="example"></a>範例

將訊息從 Azure IoT 中樞傳送至裝置。

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


## <a name="samples"></a>範例

[IoT 裝置範例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)     
[IoT 服務範例](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples)

深入探索可在應用程式中使用的 [Azure IoT Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=iot)。
