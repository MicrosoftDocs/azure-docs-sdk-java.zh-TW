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
ms.date: 12/19/2018
ms.devlang: java
ms.service: Azure Monitor
ms.tgt_pltfrm: application-insights
ms.topic: article
ms.workload: na
ms.openlocfilehash: bf4f7e51f3108d684503465050d69461240f17e3
ms.sourcegitcommit: 9df42bd342ef2d25d56a6045f1ab1baf6f2c250e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2019
ms.locfileid: "54237289"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-application-insights"></a><span data-ttu-id="25d84-103">將 Spring Boot Initializer 應用程式設定為使用 Application Insights</span><span class="sxs-lookup"><span data-stu-id="25d84-103">Configure a Spring Boot Initializer app to use Application Insights</span></span>

<span data-ttu-id="25d84-104">本文將逐步引導您使用 **[Spring Initializr]** 建立 Spring Boot 應用程式，其使用 Azure Application Insights Spring Boot 簡易版來進行雲端 JAVA 應用程式的端對端監視。</span><span class="sxs-lookup"><span data-stu-id="25d84-104">This article walks you through creating a Spring Boot application using **[Spring Initializr]**, that uses Azure Application Insights Spring Boot Starter for end-to-end monitoring of Java applications on cloud.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25d84-105">必要條件</span><span class="sxs-lookup"><span data-stu-id="25d84-105">Prerequisites</span></span>

<span data-ttu-id="25d84-106">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="25d84-106">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="25d84-107">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="25d84-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="25d84-108">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="25d84-108">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="25d84-109">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="25d84-109">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="25d84-110">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="25d84-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="25d84-111">Application Insights Spring Boot 入門版**目前不支援**使用 Web Flux 和 Netty API。</span><span class="sxs-lookup"><span data-stu-id="25d84-111">Web Flux and Netty APIs are **not currently supported** with the Application Insights Spring Boot starter.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="25d84-112">使用 Spring Initializr 建立自訂應用程式</span><span class="sxs-lookup"><span data-stu-id="25d84-112">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="25d84-113">瀏覽至 [https://start.spring.io/](https://start.spring.io/)。</span><span class="sxs-lookup"><span data-stu-id="25d84-113">Browse to [https://start.spring.io/](https://start.spring.io/).</span></span>

1. <span data-ttu-id="25d84-114">指定您想要使用 **JAVA** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱，然後在相依性區段選取 Web 相依性。</span><span class="sxs-lookup"><span data-stu-id="25d84-114">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then select web dependency in the dependenies section.</span></span>

   ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="25d84-116">Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：com.example.demo。</span><span class="sxs-lookup"><span data-stu-id="25d84-116">The Spring Initializr will use the **Group** and **Artifact** names to create the package name; for example: *com.example.demo*.</span></span>
   >

1. <span data-ttu-id="25d84-117">按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="25d84-117">Click the button to **Generate Project**.</span></span>

1. <span data-ttu-id="25d84-118">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="25d84-118">When prompted, download the project to a path on your local computer.</span></span>

1. <span data-ttu-id="25d84-119">當您在本機系統上擷取檔案之後，就可以開始編輯自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="25d84-119">After you have extracted the files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![自訂的 Spring Boot 專案檔][SI02]

## <a name="create-an-application-insights-resource-on-azure"></a><span data-ttu-id="25d84-121">在 Azure 中建立 Application Insights 資源</span><span class="sxs-lookup"><span data-stu-id="25d84-121">Create an Application Insights Resource on Azure</span></span>

1. <span data-ttu-id="25d84-122">瀏覽至 Azure 入口網站 <https://portal.azure.com/>，然後按一下 [+ 新增]。</span><span class="sxs-lookup"><span data-stu-id="25d84-122">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure 入口網站][AZ01]

1. <span data-ttu-id="25d84-124">按一下 **管理工具** ，然後按一下 **Application Insights** 。</span><span class="sxs-lookup"><span data-stu-id="25d84-124">Click **Management Tools**, and then click **Application Insights**.</span></span>

   ![Azure 入口網站][AZ02]

1. <span data-ttu-id="25d84-126">在 [新的 Application Insights 資源] 頁面上，指定下列資訊：</span><span class="sxs-lookup"><span data-stu-id="25d84-126">On the **New Application Insights Resource** page, specify the following information:</span></span>

   * <span data-ttu-id="25d84-127">輸入您的 Application Insights 資源**名稱**。</span><span class="sxs-lookup"><span data-stu-id="25d84-127">Enter the **Name** for your Application Insights resource.</span></span>
   * <span data-ttu-id="25d84-128">選擇**應用程式類型**為 JAVA Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="25d84-128">Choose the **Application Type** to Java Web Application.</span></span>
   * <span data-ttu-id="25d84-129">指定**訂用帳戶**、**資源群組**和**位置**。</span><span class="sxs-lookup"><span data-stu-id="25d84-129">Specify your **Subscription**, **Resource group** and **Location**.</span></span>
   * <span data-ttu-id="25d84-130">如果您想要在 Azure 入口網站釘選資源，選取 [釘選到儀表板] 選項。</span><span class="sxs-lookup"><span data-stu-id="25d84-130">Select Pin to dashboard option, if you would like to pin the resource on your Azure portal.</span></span>

   <span data-ttu-id="25d84-131">指定這些選項之後，按一下 [建立] 以建立您的 Application Insights 資源。</span><span class="sxs-lookup"><span data-stu-id="25d84-131">When you have specified these options, click **Create** to create your Application Insights resource.</span></span>

   ![Azure 入口網站][AZ03]

1. <span data-ttu-id="25d84-133">在您的資源建立之後，您會看到它列在您的 Azure [儀表板] 上，以及 [所有資源] 頁面下方。</span><span class="sxs-lookup"><span data-stu-id="25d84-133">Once your resource has been created, you will see it listed on your Azure **Dashboard**, as well as under the **All Resources** pages.</span></span> <span data-ttu-id="25d84-134">您可以在以上這些位置點選資源，開啟 Application Insights 資源的概觀頁面。</span><span class="sxs-lookup"><span data-stu-id="25d84-134">You can click on your resource on any of those locations to open the overview page of the Application Insights resource.</span></span> <span data-ttu-id="25d84-135">在此概觀頁面上，請複製**檢測金鑰**。</span><span class="sxs-lookup"><span data-stu-id="25d84-135">From this overview page please copy the **instrumentation key**.</span></span>

   ![Azure 入口網站][AZ04]

## <a name="configure-your-downloaded-spring-boot-application-to-use-application-insights"></a><span data-ttu-id="25d84-137">設定您下載的 Spring Boot 應用程式以使用 Application Insights</span><span class="sxs-lookup"><span data-stu-id="25d84-137">Configure your downloaded Spring Boot Application to use Application Insights</span></span>

1. <span data-ttu-id="25d84-138">在應用程式的根目錄中找出 *POM.xml* 檔案，並在相依性區段中新增下列相依性。</span><span class="sxs-lookup"><span data-stu-id="25d84-138">Locate the *POM.xml* file in the root directory of your app, and add the following dependency in its dependencies section.</span></span> 

```XML
 <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>
```

1. <span data-ttu-id="25d84-139">在應用程式的 [資源] 目錄中尋找 application.properties 檔案，如果該檔案不存在，則加以建立。</span><span class="sxs-lookup"><span data-stu-id="25d84-139">Locate the *application.properties* file in the *resources* directory of your app, or create the file if it does not already exist.</span></span>

   ![尋找 application.properties 檔案][RE01]

1. <span data-ttu-id="25d84-141">在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用適當認證的適當屬性來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="25d84-141">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties with appropriate credentials:</span></span>

   ```yaml
   # Specify the instrumentation key of your Application Insights resource.
   azure.application-insights.instrumentation-key=[your ikey from the resource]
   # Specify the name of your springboot application. This can be any logical name you would like to give to your app.
   spring.application.name=[your app name]
   ```

   <span data-ttu-id="25d84-142">如需微調 Application Insights 的更多方法，請參閱 [Application Insights Springboot 簡易版讀我檔案](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)。</span><span class="sxs-lookup"><span data-stu-id="25d84-142">For more ways to fine tune Application Insights please refer to [Application Insights Springboot starter readme](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="25d84-143">您可以將不同的 Application Insights 檢測金鑰 (即</span><span class="sxs-lookup"><span data-stu-id="25d84-143">You can use different Application Insights instrumentation keys (i.e</span></span> <span data-ttu-id="25d84-144">不同資源) 用於不同的設定檔，如 PROD、DEV 等。請參閱 [Spring Boot 設定檔特定屬性]以取得其他資訊。</span><span class="sxs-lookup"><span data-stu-id="25d84-144">different resources) for different profiles like PROD, DEV etc. Please refer to [Spring Boot Profile Specific Properties] for additional information.</span></span> 

1. <span data-ttu-id="25d84-145">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="25d84-145">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="25d84-146">在套件的來源資料夾下建立名為 controller 的資料夾，例如：</span><span class="sxs-lookup"><span data-stu-id="25d84-146">Create a folder named *controller* under the source folder for your package; for example:</span></span>

   `D:\Microsoft\demo\src\main\java\com\example\demo\controller`

   <span data-ttu-id="25d84-147">-或-</span><span class="sxs-lookup"><span data-stu-id="25d84-147">-or-</span></span>

   `/users/example/home/demo/src/main/java/com/example/demo/controller`

1. <span data-ttu-id="25d84-148">在 *controller* 資料夾中建立名稱為 *TestController.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="25d84-148">Create a new file named *TestController.java* in the *controller* folder.</span></span> <span data-ttu-id="25d84-149">在文字編輯器中開啟檔案，並加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="25d84-149">Open the file in a text editor and add the following code to it:</span></span>

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

   <span data-ttu-id="25d84-150">其中，您必須使用專案的套件名稱來取代 `com.example.demo`。</span><span class="sxs-lookup"><span data-stu-id="25d84-150">Where you will need to replace `com.example.demo` with the package name for your project.</span></span>

1. <span data-ttu-id="25d84-151">儲存並關閉 *TestController.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="25d84-151">Save and close the *TestController.java* file.</span></span>

1. <span data-ttu-id="25d84-152">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="25d84-152">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="25d84-153">使用網頁瀏覽器瀏覽至 http://localhost:8080/sample/hello 來測試 Web 應用程式；如果有可用的 curl，請使用類似下列範例的語法：</span><span class="sxs-lookup"><span data-stu-id="25d84-153">Test the web app by browsing to http://localhost:8080/sample/hello using a web browser, or use the syntax like the following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080/sample/hello
   ```

   <span data-ttu-id="25d84-154">您應該會從顯示的範例控制器中</span><span class="sxs-lookup"><span data-stu-id="25d84-154">You should see the "hello!"</span></span> <span data-ttu-id="25d84-155">看到 "hello!" 訊息。</span><span class="sxs-lookup"><span data-stu-id="25d84-155">message from your sample controller displayed.</span></span> <span data-ttu-id="25d84-156">Application Insights 會自動收集此要求，並傳送為遙測項目，連同其關聯的自訂事件、自訂計量、自訂相依性和自訂追蹤，如控制器邏輯中所指定。</span><span class="sxs-lookup"><span data-stu-id="25d84-156">Application Insights will automatically collect this request and send it as a telemetry item with it's associated custom event, custom metric, custom dependency and custom trace as specified in the controller logic.</span></span> 

   <span data-ttu-id="25d84-157">幾秒鐘後您應該會在 Azure 入口網站上看到資料。</span><span class="sxs-lookup"><span data-stu-id="25d84-157">After a few seconds you should see the data on Azure portal.</span></span> 

   ![Azure 入口網站][AZ05]

   <span data-ttu-id="25d84-159">您可以按一下應用程式對應圖格，以檢視高階元件及元件彼此的互動。</span><span class="sxs-lookup"><span data-stu-id="25d84-159">You can click on Application Map tile to view high level components and their interaction with each other.</span></span> <span data-ttu-id="25d84-160">建議在這裡取得整個應用程式的高階概觀。</span><span class="sxs-lookup"><span data-stu-id="25d84-160">This is a recommended place to get a high level overview of entire application.</span></span> <span data-ttu-id="25d84-161">每個 Spring Boot 微服務可由 spring 應用程式名稱辨識。</span><span class="sxs-lookup"><span data-stu-id="25d84-161">Each Spring Boot Microservice is recognized by the spring application name.</span></span> <span data-ttu-id="25d84-162">請務必進行設定。</span><span class="sxs-lookup"><span data-stu-id="25d84-162">Please remember to set it.</span></span>

   ![Azure 入口網站][AZ08] 

## <a name="configure-springboot-application-to-send-log4j-logs-to-application-insights"></a><span data-ttu-id="25d84-164">設定 Springboot 應用程式以傳送 log4j 記錄至 Application Insights</span><span class="sxs-lookup"><span data-stu-id="25d84-164">Configure Springboot Application to send log4j logs to Application Insights</span></span>

1. <span data-ttu-id="25d84-165">修改專案的 POM.xml 檔案，並依下列步驟新增/修改相依性區段。</span><span class="sxs-lookup"><span data-stu-id="25d84-165">Modify the POM.xml file of the project and add/modify the dependencies section with following.</span></span> 

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
        <version>1.1.1</version>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-logging-log4j2</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

2. <span data-ttu-id="25d84-166">儲存並關閉 *POM.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="25d84-166">Save and close the *POM.xml* file.</span></span>

3. <span data-ttu-id="25d84-167">在 \src\main\resources 資料夾中，建立新的檔案 *log4j2.xml* 並加以設定。</span><span class="sxs-lookup"><span data-stu-id="25d84-167">In \src\main\resources folder, create a new file *log4j2.xml* and configure it.</span></span> <span data-ttu-id="25d84-168">例如︰</span><span class="sxs-lookup"><span data-stu-id="25d84-168">For example:</span></span>

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
4. <span data-ttu-id="25d84-169">建置並再次執行 Spring Boot 應用程式，如上所示。</span><span class="sxs-lookup"><span data-stu-id="25d84-169">Build and run the Spring Boot application again as shown above.</span></span> 

<span data-ttu-id="25d84-170">在幾秒鐘內，您應該會在 Azure 入口網站上看到所有 spring 記錄可供使用。</span><span class="sxs-lookup"><span data-stu-id="25d84-170">Within few seconds, you should see all the spring logs being available on Azure Portal.</span></span> 

![Azure 入口網站][AZ06]

<span data-ttu-id="25d84-172">您甚至可以查看詳細的記錄訊息，並在 Analytics 入口網站上執行分析。</span><span class="sxs-lookup"><span data-stu-id="25d84-172">You can even look at the detailed log messages and do analysis on Analytics Portal.</span></span> 

![Azure 入口網站][AZ07]

## <a name="next-steps"></a><span data-ttu-id="25d84-174">後續步驟</span><span class="sxs-lookup"><span data-stu-id="25d84-174">Next steps</span></span>

<span data-ttu-id="25d84-175">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="25d84-175">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="25d84-176">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="25d84-176">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="25d84-177">其他資源</span><span class="sxs-lookup"><span data-stu-id="25d84-177">Additional Resources</span></span>

<span data-ttu-id="25d84-178">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="25d84-178">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="25d84-179">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="25d84-179">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="25d84-180">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="25d84-180">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="25d84-181">Application Insights 支援自動收集外部相依性，及其與連入要求的相互關聯。</span><span class="sxs-lookup"><span data-stu-id="25d84-181">Application Insights supports automatic collection of external dependencies and its correlation with incoming requests.</span></span> <span data-ttu-id="25d84-182">我們目前支援 Oracle、MsSQL、MySQL 和 Redis 的自動集合。</span><span class="sxs-lookup"><span data-stu-id="25d84-182">Currently we support autocollection of Oracle, MsSQL, MySQL and Redis.</span></span> <span data-ttu-id="25d84-183">如需啟用自動集合的詳細資訊，請遵循[如何使用 Application Insights JAVA 代理程式](/azure/application-insights/app-insights-java-agent)一文。</span><span class="sxs-lookup"><span data-stu-id="25d84-183">For more details on enabling autocollection please follow the article [how to use Application Insights Java agent](/azure/application-insights/app-insights-java-agent).</span></span>

<span data-ttu-id="25d84-184">如需 Azure Application Insights 及其監控功能的詳細資訊，請參閱 **[Application Insights]** 首頁。</span><span class="sxs-lookup"><span data-stu-id="25d84-184">For more information about Azure Application Insights, and it's monitoring capabilities, see the **[Application Insights]** home page.</span></span>

<span data-ttu-id="25d84-185">如需 Application Insights Spring Boot 簡易版其他設定的詳細資訊，請參閱此[連結](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)。</span><span class="sxs-lookup"><span data-stu-id="25d84-185">For more information about additional configuration details of Application Insights Spring Boot Starter, please refer to this [link](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span></span>

<span data-ttu-id="25d84-186">針對功能要求和潛在錯誤 (bug)，請在 [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) 存放庫開啟問題。</span><span class="sxs-lookup"><span data-stu-id="25d84-186">For feature requests and potential bugs, please open issues on our [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) repository.</span></span>

<span data-ttu-id="25d84-187">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="25d84-187">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="25d84-188">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="25d84-188">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="25d84-189">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="25d84-189">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="25d84-190">為了協助開發人員開始使用 Spring Boot，[https://github.com/spring-guides/](https://github.com/spring-guides/) 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="25d84-190">To help developers get started with Spring Boot, several sample Spring Boot packages are available at [https://github.com/spring-guides/](https://github.com/spring-guides/).</span></span> <span data-ttu-id="25d84-191">除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="25d84-191">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 設定檔特定屬性]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Boot Profile Specific Properties]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Application Insights]: /azure/application-insights/

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
