---
title: 透過 Maven 和 Azure 將 Spring Boot 應用程式部署到雲端
description: 了解如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Spring Boot 應用程式部署至雲端。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: robmcm;kevinzha;brborges
ms.date: 06/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: d58cafe3456150069ec8572c101c62d1b2c29c5d
ms.sourcegitcommit: e1a5d9687e006e8bf12d11747d45cf130a2c82af
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2018
ms.locfileid: "42703339"
---
# <a name="deploy-a-spring-boot-app-to-the-cloud-using-the-maven-plugin-for-azure-app-service"></a><span data-ttu-id="e778c-103">使用適用於 Azure App Service 的 Maven 外掛程式，將 Spring Boot 應用程式部署至雲端</span><span class="sxs-lookup"><span data-stu-id="e778c-103">Deploy a Spring Boot app to the cloud using the Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="e778c-104">本文示範如何使用適用於 Azure App Service Web Apps 的 Maven 外掛程式來部署範例 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e778c-104">This article demonstrates using the Maven Plugin for Azure App Service Web Apps to deploy a sample Spring Boot application.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="e778c-105">對於 [Apache Maven](http://maven.apache.org/) 的[適用於 Azure App Service Web Apps 的 Maven 外掛程式](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)，會提供 Azure App Service 和 Maven 專案的緊密整合，並且簡化開發人員將 Web 應用程式部署至 Azure App Service 的程序。</span><span class="sxs-lookup"><span data-stu-id="e778c-105">The [Maven Plugin for Azure App Service Web Apps](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>

<span data-ttu-id="e778c-106">在使用 Maven 外掛程式之前，請在 Maven Central 上檢查外掛程式的最新可用版本：[![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22)</span><span class="sxs-lookup"><span data-stu-id="e778c-106">Before using the Maven plugin, check on Maven Central for the latest available version of the plugin: [![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e778c-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="e778c-107">Prerequisites</span></span>

<span data-ttu-id="e778c-108">若要完成本教學課程中的步驟，您必須具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="e778c-108">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="e778c-109">Azure 訂用帳戶；如果您沒有 Azure 訂用帳戶，可以註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="e778c-109">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="e778c-110">[Azure 命令列介面 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="e778c-110">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="e778c-111">最新的 [Java 開發套件 (JDK)] 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e778c-111">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="e778c-112">Apache 的 [Maven] 建置工具 (第 3 版)。</span><span class="sxs-lookup"><span data-stu-id="e778c-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="e778c-113">[Git] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e778c-113">A [Git] client.</span></span>

## <a name="clone-the-sample-spring-boot-web-app"></a><span data-ttu-id="e778c-114">複製範例 Spring Boot Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="e778c-114">Clone the sample Spring Boot web app</span></span>

<span data-ttu-id="e778c-115">在本節中，您會在本機複製已完成的 Spring Boot 應用程式，並且進行測試。</span><span class="sxs-lookup"><span data-stu-id="e778c-115">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="e778c-116">開啟命令提示字元或終端機視窗，並建立本機目錄來保存您的 Spring Boot 應用程式，然後變更至該目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="e778c-116">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="e778c-117">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="e778c-117">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="e778c-118">將 [Spring Boot Getting Started] 範例專案複製到您建立的目錄中；例如：</span><span class="sxs-lookup"><span data-stu-id="e778c-118">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="e778c-119">將目錄變更至已完成的專案；例如：</span><span class="sxs-lookup"><span data-stu-id="e778c-119">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="e778c-120">使用 Maven 建立 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="e778c-120">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="e778c-121">建立 Web 應用程式後，使用 Maven 啟動 Web 應用程式，例如：</span><span class="sxs-lookup"><span data-stu-id="e778c-121">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="e778c-122">測試 Web 應用程式，方法是使用網頁瀏覽器在本機瀏覽它。</span><span class="sxs-lookup"><span data-stu-id="e778c-122">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="e778c-123">例如，如果您有 curl 可用，可以使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="e778c-123">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="e778c-124">您應該會看到顯示下列訊息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="e778c-124">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="adjust-project-for-war-based-deployment-on-azure-app-service"></a><span data-ttu-id="e778c-125">在 Azure App Service 上調整專案，以進行 WAR 型部署</span><span class="sxs-lookup"><span data-stu-id="e778c-125">Adjust project for WAR-based deployment on Azure App Service</span></span>

<span data-ttu-id="e778c-126">在本節中，我們會快速調整 Spring Boot 專案，以在 Azure App Service 上部署為 WAR 檔案，其依預設會提供 Tomcat 做為執行階段。</span><span class="sxs-lookup"><span data-stu-id="e778c-126">In this section we will quickly adjust the Spring Boot project to be deployed as a WAR file on Azure App Service, which provides Tomcat as the runtime by default.</span></span> <span data-ttu-id="e778c-127">針對此目的，有兩個檔案要修改：</span><span class="sxs-lookup"><span data-stu-id="e778c-127">For this to work, there are two files to be modified:</span></span>

- <span data-ttu-id="e778c-128">Maven `pom.xml` 檔案</span><span class="sxs-lookup"><span data-stu-id="e778c-128">The Maven `pom.xml` file</span></span>
- <span data-ttu-id="e778c-129">`Application` JAVA 類別</span><span class="sxs-lookup"><span data-stu-id="e778c-129">The `Application` Java class</span></span>

<span data-ttu-id="e778c-130">我們從 Maven 設定開始：</span><span class="sxs-lookup"><span data-stu-id="e778c-130">Let's start with the Maven settings:</span></span>

1. <span data-ttu-id="e778c-131">開啟 `pom.xml`</span><span class="sxs-lookup"><span data-stu-id="e778c-131">Open `pom.xml`</span></span>

1. <span data-ttu-id="e778c-132">在頂端 `<artifactId>` 定義後新增 `<packaging>war</packaging>`：</span><span class="sxs-lookup"><span data-stu-id="e778c-132">Add `<packaging>war</packaging>` right after the `<artifactId>` definition at the top:</span></span>
   ```xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>

    <packaging>war</packaging>
   ```

1. <span data-ttu-id="e778c-133">新增下列相依性：</span><span class="sxs-lookup"><span data-stu-id="e778c-133">Add the following dependency:</span></span>
   ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
   ```

<span data-ttu-id="e778c-134">現在開啟類別 `Application` (最好是在您的 IDE 已採用新相依性後)，並繼續下列修改：</span><span class="sxs-lookup"><span data-stu-id="e778c-134">Now open the class `Application`, hopefully after your IDE has already picked up the new dependencies, and proceed with the following modifications:</span></span>

1. <span data-ttu-id="e778c-135">讓 [應用程式] 類別作為 `SpringBootServletInitializer` 的子類別：</span><span class="sxs-lookup"><span data-stu-id="e778c-135">Make class Application a subclass of `SpringBootServletInitializer`:</span></span>
   ```java
   @SpringBootApplication
   public class Application extends SpringBootServletInitializer {
     // ...
   }
   ```

1. <span data-ttu-id="e778c-136">將下列方法新增至應用程式類別：</span><span class="sxs-lookup"><span data-stu-id="e778c-136">Add the following method to the Application class:</span></span>
   ```java
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
           return application.sources(Application.class);
       }
   ```

<span data-ttu-id="e778c-137">您的應用程式現在已準備好部署到 Tomcat，和任何其他 Servlet 執行階段 (例如 Jetty)。</span><span class="sxs-lookup"><span data-stu-id="e778c-137">Your application is now ready to be deployed to Tomcat and any other Servlet runtime (e.g. Jetty).</span></span>

## <a name="add-the-maven-plugin-for-azure-app-service-web-apps"></a><span data-ttu-id="e778c-138">為 Azure App Service Web Apps 新增 Maven 外掛程式</span><span class="sxs-lookup"><span data-stu-id="e778c-138">Add the Maven Plugin for Azure App Service Web Apps</span></span>

<span data-ttu-id="e778c-139">在本節中，我們會將可自動執行此應用程式整個部署的 Maven 外掛程式新增到 Azure App Service Web Apps。</span><span class="sxs-lookup"><span data-stu-id="e778c-139">In this section, we will add a Maven plugin that will automate the entire deployment of this application into Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="e778c-140">再次開啟 `pom.xml`。</span><span class="sxs-lookup"><span data-stu-id="e778c-140">Open `pom.xml` once again.</span></span>

1. <span data-ttu-id="e778c-141">在 `<properties>` 中，使用屬性 `maven.build.timestamp.format` 設定自訂時間戳記格式。</span><span class="sxs-lookup"><span data-stu-id="e778c-141">Inside `<properties>`, set a custom timestamp format with the property `maven.build.timestamp.format`.</span></span> <span data-ttu-id="e778c-142">因為 Azure App Service 會建立應用程式的公用 URL，此設定會用來產生部署名稱，並避免與其他使用者的即時部署發生衝突。</span><span class="sxs-lookup"><span data-stu-id="e778c-142">Because Azure App Service creates a public URL for your application, this setting will be used to generate the name of your deployment, and avoid conflict with other users' live deployments.</span></span>
   ```xml
    <properties>
        <java.version>1.8</java.version>
        <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
    </properties>
   ```

1. <span data-ttu-id="e778c-143">在 `<plugins>` 元素中，新增下列：</span><span class="sxs-lookup"><span data-stu-id="e778c-143">In the `<plugins>` element, add the following:</span></span>
   ```xml
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
    </plugin>
   ```

<span data-ttu-id="e778c-144">有了這些設定後，您的 Maven 專案已準備好即時部署至 Azure App Service Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e778c-144">With these settings, your Maven project is now ready for live deployment to Azure App Service Web App.</span></span>

## <a name="install-and-log-in-to-azure-cli"></a><span data-ttu-id="e778c-145">安裝並登入 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e778c-145">Install and log in to Azure CLI</span></span>

<span data-ttu-id="e778c-146">讓 Maven 外掛程式部署 Spring Boot 應用程式最簡單且最輕鬆的方式是使用 [Azure CLI](https://docs.microsoft.com/cli/azure/)。</span><span class="sxs-lookup"><span data-stu-id="e778c-146">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](https://docs.microsoft.com/cli/azure/).</span></span> <span data-ttu-id="e778c-147">請確認您已進行安裝。</span><span class="sxs-lookup"><span data-stu-id="e778c-147">Make sure you have it installed.</span></span>

1. <span data-ttu-id="e778c-148">使用 Azure CLI 登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="e778c-148">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
   <span data-ttu-id="e778c-149">依照指示完成登入程序。</span><span class="sxs-lookup"><span data-stu-id="e778c-149">Follow the instructions to complete the sign-in process.</span></span>

## <a name="optionally-customize-pomxml-before-deploying"></a><span data-ttu-id="e778c-150">(選擇性) 在部署之前自訂 pom.xml</span><span class="sxs-lookup"><span data-stu-id="e778c-150">Optionally, customize pom.xml before deploying</span></span>

<span data-ttu-id="e778c-151">在文字編輯器中開啟 Spring Boot 應用程式的 `pom.xml` 檔案，然後找出 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。</span><span class="sxs-lookup"><span data-stu-id="e778c-151">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="e778c-152">此元素外觀會類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="e778c-152">This element should resemble the following example:</span></span>

   ```xml
  <plugins>
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
      <configuration>
         <resourceGroup>maven-projects</resourceGroup>
         <appName>${project.artifactId}-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>war</deploymentType>
      </configuration>
    </plugin>
  </plugins>
   ```

<span data-ttu-id="e778c-153">您可以為 Maven 外掛程式修改數個值，這些元素的詳細描述可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得。</span><span class="sxs-lookup"><span data-stu-id="e778c-153">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="e778c-154">也就是說，有數個值值得在這篇文章中反白顯示：</span><span class="sxs-lookup"><span data-stu-id="e778c-154">That being said, there are several values that are worth highlighting in this article:</span></span>

| <span data-ttu-id="e778c-155">元素</span><span class="sxs-lookup"><span data-stu-id="e778c-155">Element</span></span> | <span data-ttu-id="e778c-156">說明</span><span class="sxs-lookup"><span data-stu-id="e778c-156">Description</span></span> |
|---|---|
| `<version>` | <span data-ttu-id="e778c-157">指定[適用於 Azure Web 應用程式的 Maven 外掛程式]版本。</span><span class="sxs-lookup"><span data-stu-id="e778c-157">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="e778c-158">請檢查 [Maven 中央存放庫](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中所列的版本，確定您使用的是最新版本。</span><span class="sxs-lookup"><span data-stu-id="e778c-158">Verify the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span> |
| `<resourceGroup>` | <span data-ttu-id="e778c-159">指定目標資源群組，也就是此範例中的 `maven-plugin`。</span><span class="sxs-lookup"><span data-stu-id="e778c-159">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="e778c-160">如果該資源群組不存在，則系統會在部署期間建立它。</span><span class="sxs-lookup"><span data-stu-id="e778c-160">The resource group is created during deployment if it does not already exist.</span></span> |
| `<appName>` | <span data-ttu-id="e778c-161">指定 Web 應用程式的目標名稱。</span><span class="sxs-lookup"><span data-stu-id="e778c-161">Specifies the target name for your web app.</span></span> <span data-ttu-id="e778c-162">在此範例中，目標名稱是 `maven-web-app-${maven.build.timestamp}`，在此範例中會附加 `${maven.build.timestamp}` 尾碼以避免發生衝突。</span><span class="sxs-lookup"><span data-stu-id="e778c-162">In this example, the target name is `maven-web-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="e778c-163">(時間戳記是選擇性的；您可以為應用程式名稱指定任何唯一的字串。)</span><span class="sxs-lookup"><span data-stu-id="e778c-163">(The timestamp is optional; you can specify any unique string for the app name.)</span></span> |
| `<region>` | <span data-ttu-id="e778c-164">指定目標區域，在此範例中是 `westus`。</span><span class="sxs-lookup"><span data-stu-id="e778c-164">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="e778c-165">(完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。)</span><span class="sxs-lookup"><span data-stu-id="e778c-165">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<javaVersion>` | <span data-ttu-id="e778c-166">指定 Web 應用程式的 Java 執行階段版本。</span><span class="sxs-lookup"><span data-stu-id="e778c-166">Specifies the Java runtime version for your web app.</span></span> <span data-ttu-id="e778c-167">(完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。)</span><span class="sxs-lookup"><span data-stu-id="e778c-167">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<deploymentType>` | <span data-ttu-id="e778c-168">指定 Web 應用程式的部署類型。</span><span class="sxs-lookup"><span data-stu-id="e778c-168">Specifies deployment type for your web app.</span></span> <span data-ttu-id="e778c-169">預設值為 `war`。</span><span class="sxs-lookup"><span data-stu-id="e778c-169">Default is `war`.</span></span> |

## <a name="build-and-deploy-your-web-app-to-azure"></a><span data-ttu-id="e778c-170">建置 Web 應用程式並將其部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="e778c-170">Build and deploy your web app to Azure</span></span>

<span data-ttu-id="e778c-171">一旦您已設定本文上述章節中的所有設定，您已準備好將 Web 應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="e778c-171">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="e778c-172">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="e778c-172">To do so, use the following steps:</span></span>

1. <span data-ttu-id="e778c-173">如果您對 pom.xml 檔案進行任何變更，從您稍早使用的命令提示字元或終端機視窗，使用 Maven 重新建置 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="e778c-173">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="e778c-174">使用 Maven 將您的 Web 應用程式部署至 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="e778c-174">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="e778c-175">Maven 會將您的 Web 應用程式部署至 Azure；如果 Web 應用程式不存在，系統會加以建立。</span><span class="sxs-lookup"><span data-stu-id="e778c-175">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

<span data-ttu-id="e778c-176">已部署您的網站時，您就可以使用 [Azure 入口網站]來管理它。</span><span class="sxs-lookup"><span data-stu-id="e778c-176">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="e778c-177">您的 Web 應用程式會列在**應用程式服務** 中：</span><span class="sxs-lookup"><span data-stu-id="e778c-177">Your web app will be listed in **App Services**:</span></span>

   ![列在 Azure 入口網站應用程式服務中的 Web 應用程式][AP01]

* <span data-ttu-id="e778c-179">Web 應用程式的 URL 會列在 Web 應用程式的 [概觀] 中：</span><span class="sxs-lookup"><span data-stu-id="e778c-179">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![決定 Web 應用程式的 URL][AP02]

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="e778c-181">後續步驟</span><span class="sxs-lookup"><span data-stu-id="e778c-181">Next steps</span></span>

<span data-ttu-id="e778c-182">如需本文所討論之各種技術的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="e778c-182">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="e778c-183">[適用於 Azure Web 應用程式的 Maven 外掛程式]</span><span class="sxs-lookup"><span data-stu-id="e778c-183">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="e778c-184">從 Azure CLI 登入 Azure</span><span class="sxs-lookup"><span data-stu-id="e778c-184">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="e778c-185">如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將容器化 Spring Boot 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="e778c-185">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="e778c-186">使用 Azure CLI 2.0 來建立 Azure 服務主體</span><span class="sxs-lookup"><span data-stu-id="e778c-186">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="e778c-187">Maven 設定參考</span><span class="sxs-lookup"><span data-stu-id="e778c-187">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure 入口網站]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[適用於 Azure Web 應用程式的 Maven 外掛程式]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
