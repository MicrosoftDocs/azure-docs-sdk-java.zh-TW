---
title: 將 Spring Boot Initializer 應用程式設定為使用 Azure Application Insights SpringBoot 簡易版
description: 將使用 Spring Initializr 所建立的 Spring Boot 應用程式設定為使用 Appliaction Insights SpringBoot 簡易版。
services: Application-Insights
documentationcenter: java
author: dhaval24
manager: alexklim
editor: ''
ms.assetid: ''
ms.author: dhdoshi
ms.date: 05/19/2018
ms.devlang: java
ms.service: Azure Monitor
ms.tgt_pltfrm: application-insights
ms.topic: article
ms.workload: na
ms.openlocfilehash: 0e57bfb304185b8b98dedfdecb2e0374c4a72fe5
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090771"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-application-insights"></a>將 Spring Boot Initializer 應用程式設定為使用 Application Insights

本文將逐步引導您使用 **[Spring Initializr]** 建立 Spring Boot 應用程式，其使用 Azure Application Insights Spring Boot 簡易版來進行雲端 JAVA 應用程式的端對端監視。

> [!NOTE]
> 
> *此簡易版目前為 *BETA 版 (公開預覽)<em>。</em>

## <a name="prerequisites"></a>先決條件

請務必具備下列必要條件，以便本文中說明的步驟：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* JAVA 開發套件 (JDK) 1.7 版和 1.8 版。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>使用 Spring Initializr 建立自訂應用程式

1. 瀏覽至 [https://start.spring.io/](https://start.spring.io/)。

1. 指定您想要使用 **JAVA** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱，然後在相依性區段選取 Web 相依性。

   ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：com.example.demo。
   >

1. 按一下按鈕以**產生專案**。

1. 出現提示時，將專案下載至本機電腦上的路徑。

1. 當您在本機系統上擷取檔案之後，就可以開始編輯自訂的 Spring Boot 應用程式。

   ![自訂的 Spring Boot 專案檔][SI02]

## <a name="create-an-application-insights-resource-on-azure"></a>在 Azure 中建立 Application Insights 資源

1. 瀏覽至 Azure 入口網站 <https://portal.azure.com/>，然後按一下 [+ 新增]。

   ![Azure 入口網站][AZ01]

1. 按一下 [管理工具]，然後按一下 [Application Insights]。

   ![Azure 入口網站][AZ02]

1. 在 [新的 Application Insights 資源] 頁面上，指定下列資訊：

   * 輸入您的 Application Insights 資源**名稱**。
   * 選擇**應用程式類型**為 JAVA Web 應用程式。
   * 指定**訂用帳戶**、**資源群組**和**位置**。
   * 如果您想要在 Azure 入口網站釘選資源，選取 [釘選到儀表板] 選項。

   指定這些選項之後，按一下 [建立] 以建立您的 Application Insights 資源。

   ![Azure 入口網站][AZ03]

1. 在您的資源建立之後，您會看到它列在您的 Azure [儀表板] 上，以及 [所有資源] 頁面下方。 您可以在以上這些位置點選資源，開啟 Application Insights 資源的概觀頁面。 在此概觀頁面上，請複製**檢測金鑰**。

   ![Azure 入口網站][AZ04]

## <a name="configure-your-downloaded-spring-boot-application-to-use-application-insights"></a>設定您下載的 Spring Boot 應用程式以使用 Application Insights

1. 在應用程式的根目錄中找出 *POM.xml* 檔案，並在相依性區段中新增下列相依性。 

```XML
 <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>1.0.0-BETA</version>
</dependency>
```

1. 在應用程式的 [資源] 目錄中尋找 application.properties 檔案，如果該檔案不存在，則加以建立。

   ![尋找 application.properties 檔案][RE01]

1. 在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用適當認證的適當屬性來取代範例值：

   ```yaml
   # Specify the instrumentation key of your Application Insights resource.
   azure.application-insights.instrumentation-key=[your ikey from the resource]
   # Specify the name of your springboot application. This can be any logical name you would like to give to your app.
   spring.application.name=[your app name]
   ```

   如需微調 Application Insights 的更多方法，請參閱 [Application Insights Springboot 簡易版讀我檔案](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)。

   > [!NOTE]
   > 
   > 您可以將不同的 Application Insights 檢測金鑰 (即 不同資源) 用於不同的設定檔，如 PROD、DEV 等。請參閱 [Spring Boot 設定檔特定屬性]以取得其他資訊。 

1. 儲存並關閉 *application.properties* 檔案。

1. 在套件的來源資料夾下建立名為 controller 的資料夾，例如：

   `D:\Microsoft\demo\src\main\java\com\example\demo\controller`

   -或-

   `/users/example/home/demo/src/main/java/com/example/demo/controller`

1. 在 *controller* 資料夾中建立名稱為 *TestController.java* 的新檔案。 在文字編輯器中開啟檔案，並加入下列程式碼：

   ```java
    package com.example.demo;

    import com.microsoft.applicationinsights.TelemetryClient;
    import java.io.IOException;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    import com.microsoft.applicationinsights.telemetry.Duration;

    @RestController
    @RequestMapping("/sample")
    public class TestController {

        @Autowired
        TelemetryClient telemetryClient;

        @GetMapping("/hello")
        public String hello() {

            //track a custom event  
            telemetryClient.trackEvent("Sending a custom event...");

            //trace a custom trace
            telemetryClient.trackTrace("Sending a custom trace....");

            //track a custom metric
            telemetryClient.trackMetric("custom metric", 1.0);

            //track a custom dependency
            telemetryClient.trackDependency("SQL", "Insert", new Duration(0, 0, 1, 1, 1), true);

            return "hello";
        }
    }
   ```

   其中，您必須使用專案的套件名稱來取代 `com.example.demo`。

1. 儲存並關閉 *TestController.java* 檔案。

1. 使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 使用網頁瀏覽器瀏覽至 http://localhost:8080/sample/hello 來測試 Web 應用程式；如果有可用的 curl，請使用類似下列範例的語法：

   ```shell
   curl http://localhost:8080/sample/hello
   ```

   您應該會從顯示的範例控制器中 看到 "hello!" 訊息。 Application Insights 會自動收集此要求，並傳送為遙測項目，連同其關聯的自訂事件、自訂計量、自訂相依性和自訂追蹤，如控制器邏輯中所指定。 

   幾秒鐘後您應該會在 Azure 入口網站上看到資料。 

   ![Azure 入口網站][AZ05]

   您可以按一下應用程式對應圖格，以檢視高階元件及元件彼此的互動。 建議在這裡取得整個應用程式的高階概觀。 每個 Spring Boot 微服務可由 spring 應用程式名稱辨識。 請務必進行設定。

   ![Azure 入口網站][AZ08] 

## <a name="configure-springboot-application-to-send-log4j-logs-to-application-insights"></a>設定 Springboot 應用程式以傳送 log4j 記錄至 Application Insights

1. 修改專案的 POM.xml 檔案，並依下列步驟新增/修改相依性區段。 

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j2</artifactId>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>1.0.0-BETA</version>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-logging-log4j2</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

2. 儲存並關閉 *POM.xml* 檔案。

3. 在 \src\main\resources 資料夾中，建立新的檔案 *log4j2.xml* 並加以設定。 例如︰

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Configuration packages="com.microsoft.applicationinsights.log4j.v2">
  <Properties>
    <Property name="LOG_PATTERN">
      %d{yyyy-MM-dd HH:mm:ss.SSS} %5p ${hostName} --- [%15.15t] %-40.40c{1.} : %m%n%ex
    </Property>
  </Properties>
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
    <ApplicationInsightsAppender name="aiAppender">
    </ApplicationInsightsAppender>
  </Appenders>
  <Loggers>
    <Root level="trace">
      <AppenderRef ref="Console"  />
      <AppenderRef ref="aiAppender"  />
    </Root>
  </Loggers>
</Configuration>
```
4. 建置並再次執行 Spring Boot 應用程式，如上所示。 

在幾秒鐘內，您應該會在 Azure 入口網站上看到所有 spring 記錄可供使用。 

![Azure 入口網站][AZ06]

您甚至可以查看詳細的記錄訊息，並在 Analytics 入口網站上執行分析。 

![Azure 入口網站][AZ07]

## <a name="next-steps"></a>後續步驟

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式](deploy-spring-boot-java-app-on-kubernetes.md)

Application Insights 支援自動收集外部相依性，及其與連入要求的相互關聯。 我們目前支援 Oracle、MsSQL、MySQL 和 Redis 的自動集合。 如需啟用自動集合的詳細資訊，請遵循[如何使用 Application Insights JAVA 代理程式](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-java-agent)一文。

如需 Azure Application Insights 及其監控功能的詳細資訊，請參閱 **[Application Insights]** 首頁。

如需 Application Insights Spring Boot 簡易版其他設定的詳細資訊，請參閱此[連結](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)。

針對功能要求和潛在錯誤 (bug)，請在 [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) 存放庫開啟問題。

如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。

**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。 [Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。 為了協助開發人員開始使用 Spring Boot，[https://github.com/spring-guides/](https://github.com/spring-guides/) 上提供了數個範例 Spring Boot 套件。 除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。

<!-- URL List -->

[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 設定檔特定屬性]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Application Insights]: https://docs.microsoft.com/en-us/azure/application-insights/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_2.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_3.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Get_IKey.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/OverviewBladeResults.png
[AZ06]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Search_and_traces.png
[AZ07]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/traces_details.png
[AZ08]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/AppMap.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/spring_start.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/After_extract.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationproperties_loc.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationinsightsproperties.png
