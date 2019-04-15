---
title: 如何搭配 Azure 事件中樞使用適用於 Apache Kafka 的 Spring Boot Starter
description: 了解如何將使用 Spring Boot Initializer 所建立的應用程式設定為搭配使用 Apache Kafka 與 Azure 事件中樞。
services: event-hubs
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: na
ms.openlocfilehash: f2cf66a4e0ac113406781b4859869ff4edab527e
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991532"
---
# <a name="how-to-use-the-spring-boot-starter-for-apache-kafka-with-azure-event-hubs"></a>如何搭配 Azure 事件中樞使用適用於 Apache Kafka 的 Spring Boot Starter

## <a name="overview"></a>概觀

本文會示範如何將使用 Spring Boot Initializer 建立的 Java 架構 Spring Cloud Stream Binder 設定為搭配使用 [Apache Kafka] 與 Azure 事件中樞。

## <a name="prerequisites"></a>必要條件

若要遵循本文中的步驟，需要具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。

> [!IMPORTANT]
>
> 您需要 Spring Boot 2.0 版或更新版本才能完成本文中的步驟。
>

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a>使用 Azure 入口網站建立 Azure 事件中樞

### <a name="create-an-azure-event-hub-namespace"></a>建立 Azure 事件中樞命名空間

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 依序按一下 [+ 建立資源]、[物聯網] 和 [事件中樞]。

   ![建立 Azure 事件中樞命名空間][IMG01]

1. 在 [建立命名空間] 頁面上，輸入下列資訊：

   * 輸入唯一**名稱**，這將成為事件中樞命名空間 URI 的一部分。 例如：如果您輸入 **wingtiptoys** 作為**名稱**，則 URI 會是 wingtiptoys.servicebus.windows.net。
   * 為事件中樞命名空間選擇 [定價層]。
   * 為命名空間指定 [啟用 Kafka]。
   * 選取您想要用於命名空間的**訂用帳戶**。
   * 指定是否要為命名空間建立新的**資源群組**，或選擇現有的資源群組。
   * 指定事件中樞命名空間的**位置**。

   ![指定 Azure 事件中樞命名空間選項][IMG02]

1. 指定上面列出的選項之後，按一下 [建立] 以建立您的命名空間。

### <a name="create-an-azure-event-hub-in-your-namespace"></a>在命名空間中建立 Azure 事件中樞

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站。

1. 按一下 [所有資源]，然後按一下您建立的命名空間。

   ![選取 Azure 事件中樞命名空間][IMG03]

1. 按一下 [事件中樞]，然後按一下 [+ 事件中樞]。

   ![新增 Azure 事件中樞][IMG04]

1. 在 [建立事件中樞] 頁面上，為您的事件中樞輸入唯一**名稱**，然後按一下 [建立]。

   ![建立 Azure 事件中樞][IMG05]

1. 事件中樞建好之後，即會列在 [事件中樞] 頁面上。

   ![建立 Azure 事件中樞][IMG06]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 建立簡單的 Spring Boot 應用程式

1. 瀏覽至 <https://start.spring.io/> 。

1. 指定下列選項：

   * 使用 **Java** 產生 **Maven** 專案。
   * 指定 **Spring Boot** 版本，應等於或大於 2.0。
   * 指定應用程式的**群組**和**成品**名稱。
   * 新增 **Web** 相依性。

      ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：com.wingtiptoys.kafka。
   >

1. 當您指定上面列出的選項之後，按一下 [產生專案]。

1. 出現提示時，將專案下載至本機電腦上的路徑。

   ![下載 Spring 專案][SI02]

1. 當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。

## <a name="configure-your-spring-boot-app-to-use-the-spring-cloud-kafka-stream-and-azure-event-hub-starters"></a>將 Spring Boot 應用程式設定為使用 Spring Cloud Kafka Stream 與 Azure 事件中樞 Starter

1. 在應用程式的根目錄中尋找 pom.xml 檔案；例如：

   `C:\SpringBoot\kafka\pom.xml`

   -或-

   `/users/example/home/kafka/pom.xml`

1. 在文字編輯器中開啟 pom.xml 檔案，並將 Spring Cloud Kafka Stream 和 Azure 事件中樞 Starter 新增至 `<dependencies>` 的清單：

   ```xml
   <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-stream-kafka</artifactId>
      <version>2.0.1.RELEASE</version>
   </dependency>
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-starter-eventhub</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][SI03]

1. 儲存並關閉 *pom.xml* 檔案。

## <a name="create-an-azure-credential-file"></a>建立 Azure 認證檔案

1. 開啟命令提示字元。

1. 瀏覽至 Spring Boot 應用程式的 resources 目錄；例如：

   ```shell
   cd C:\SpringBoot\eventhub\src\main\resources
   ```

   -或-

   ```shell
   cd /users/example/home/eventhub/src/main/resources
   ```

1. 登入您的 Azure 帳戶：

   ```azurecli
   az login
   ```

1. 列出您的訂用帳戶：

   ```azurecli
   az account list
   ```
   Azure 會傳回訂用帳戶清單，而且您必須複製所要使用之訂用帳戶的 GUID；例如：

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```
   
1. 指定您希望在 Azure 中使用的訂用帳戶 GUID，例如：

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. 建立 Azure 認證檔案：

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   此命令會在您的 resources 目錄中建立 my.azureauth 檔案，內含會類似下列範例：

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a>設定 Spring Boot 應用程式以使用您的 Azure 事件中樞

1. 在應用程式的 resources 目錄中尋找 *application.properties*；例如：

   `C:\SpringBoot\eventhub\src\main\resources\application.properties`

   -或-

   `/users/example/home/eventhub/src/main/resources/application.properties`

2. 在文字編輯器中開啟 application.properties 檔案、新增下列數行，然後使用您事件中樞的適當屬性來取代範例值：

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace

   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   ```
   其中：

   |                       欄位                       |                                                                                   說明                                                                                    |
   |---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `spring.cloud.azure.credential-file-path`     |                                                    指定您稍早在本教學課程中建立的 Azure 認證檔案。                                                    |
   |        `spring.cloud.azure.resource-group`        |                                                      指定包含您 Azure 事件中樞的 Azure 資源群組。                                                      |
   |            `spring.cloud.azure.region`            |                                           指定建立您 Azure 事件中樞時指定的地理區域。                                            |
   |      `spring.cloud.azure.eventhub.namespace`      |                                          指定建立您 Azure 事件中樞命名空間時指定的唯一名稱。                                           |
   | `spring.cloud.stream.bindings.input.destination`  |                            指定作為輸入目的地的 Azure 事件中樞，在本教學課程中是指您稍早在本教學課程中建立的中樞。                            |
   |    `spring.cloud.stream.bindings.input.group `    | 指定 Azure 事件中樞中的取用者群組，可以設定為 '$Default'，以使用您建立 Azure 事件中樞時所建立的基本取用者群組。 |
   | `spring.cloud.stream.bindings.output.destination` |                               指定作為輸出目的地的 Azure 事件中樞，這在本教學課程中會等同輸入目的地。                               |


3. 儲存並關閉 *application.properties* 檔案。

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a>新增範例程式碼來實作基本的事件中樞功能

在本節中，您會建立必要的 Java 類別，以便將事件傳送至事件中樞。

### <a name="modify-the-main-application-class"></a>修改主要應用程式類別

1. 在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：

   `C:\SpringBoot\kafka\src\main\java\com\wingtiptoys\kafka\KafkaApplication.java`

   -或-

   `/users/example/home/kafka/src/main/java/com/wingtiptoys/kafka/KafkaApplication.java`

1. 在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class KafkaApplication {
      public static void main(String[] args) {
         SpringApplication.run(KafkaApplication.class, args);
      }
   }
   ```

1. 儲存並關閉主要應用程式 Java 檔案。


### <a name="create-a-new-class-for-the-source-connector"></a>建立新的來源連接器類別

1. 在應用程式的 package 目錄中，建立名為 KafkaSource.java 的新 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;

   @EnableBinding(Source.class)
   @RestController
   public class KafkaSource {
      @Autowired
      private Source source;

      @PostMapping("/messages")
      public String sendMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```

1. 儲存並關閉 KafkaSource.java 檔案。

### <a name="create-a-new-class-for-the-sink-connector"></a>建立新的接收連接器類別

1. 在應用程式的 package 目錄中，建立名為 KafkaSink.java 的新 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：

   ```java
   package com.wingtiptoys.kafka;

   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;

   @EnableBinding(Sink.class)
   public class KafkaSink {
      private static final Logger LOGGER = LoggerFactory.getLogger(KafkaSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message) {
         LOGGER.info("New message received: " + message);
      }
   }
   ```

1. 儲存並關閉 KafkaSink.java 檔案。

## <a name="build-and-test-your-application"></a>建置與測試您的應用程式

1. 開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：

   `cd C:\SpringBoot\kafka`

   -或-

   `cd /users/example/home/kafka`

1. 使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 當您的應用程式執行時，可以使用cURL 來測試您的應用程式；例如：

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   您應該會看到 "hello" 已發佈至應用程式記錄。 例如︰

   ```shell
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka version : 1.0.2
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka commitId : 2a121f7b1d402825
   [wingtiptoyshub.container-0-C-1] INFO com.wingtiptoys.kafka.KafkaSink - New message received: hello
   ```


> [!NOTE]
> 
> 基於測試目的，您可以修改 KafkaSource.java，使其包含簡單的 HTML 表單，如下列範例所示：
> 
> ```java
> package com.wingtiptoys.kafka;
>    
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.cloud.stream.annotation.EnableBinding;
> import org.springframework.cloud.stream.messaging.Source;
> import org.springframework.messaging.support.GenericMessage;
> import org.springframework.web.bind.annotation.GetMapping;
> import org.springframework.web.bind.annotation.PostMapping;
> import org.springframework.web.bind.annotation.RequestBody;
> import org.springframework.web.bind.annotation.RequestParam;
> import org.springframework.web.bind.annotation.RestController;
> 
> @EnableBinding(Source.class)
> @RestController
> public class KafkaSource {
>   @Autowired
>   private Source source;
> 
>   @GetMapping("/")
>   public String sendForm() {
>     return "<html><body>" +
>       "<form action=\"/messages\" method=\"post\">" +
>       "<input type=\"text\" name=\"text\">" +
>       "<input type=\"submit\">" +
>       "</form></body><html>";
>     }
> 
>   @PostMapping("/messages")
>   public String sendMessage(@RequestBody String message) {
>     this.source.output().send(new GenericMessage<>(message));
>     return message;
>   }
> }
> ```
> 
> 這可讓您使用網頁瀏覽器來測試您的應用程式：
> 
> ![使用網頁瀏覽器測試應用程式][TB01]
> 
> 當您提交表單時，應用程式會顯示結果：
> 
> ![網頁瀏覽器中的應用程式回應][TB02]
> 

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他資源

若要深入了解適用於事件中樞 Stream Binder 和 Apache Kafka 的 Azure 支援，請參閱下列文章：

* [Azure 事件中樞是什麼？](/azure/event-hubs/event-hubs-about)

* [適用於 Apache Kafka 的 Azure 事件中樞](/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)

* [使用 Azure 入口網站來建立事件中樞命名空間和事件中樞](/azure/event-hubs/event-hubs-create)

* [建立已啟用 Apache kafka 的事件中樞](/azure/event-hubs/event-hubs-create-kafka-enabled)

如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱 [適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。

**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。 [Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。 為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。 除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。

<!-- URL List -->

[Apache Kafka]: http://kafka.apache.org
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[IMG01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-01.png
[IMG02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-02.png
[IMG03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-03.png
[IMG04]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-04.png
[IMG05]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-05.png
[IMG06]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-06.png
[IMG07]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-07.png
[IMG08]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-08.png

[SI01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-01.png
[SI02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-02.png
[SI03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-03.png

[TB01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-01.png
[TB02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-02.png
