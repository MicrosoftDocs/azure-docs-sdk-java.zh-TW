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
ms.openlocfilehash: ca788354d26964bd9f1e21a0d3a8005ff65ce4bc
ms.sourcegitcommit: 280d13b43cef94177d95e03879a5919da234a23c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "43324344"
---
# <a name="deploy-a-spring-boot-app-to-the-cloud-using-the-maven-plugin-for-azure-app-service"></a>使用適用於 Azure App Service 的 Maven 外掛程式，將 Spring Boot 應用程式部署至雲端

本文示範如何使用適用於 Azure App Service Web Apps 的 Maven 外掛程式來部署範例 Spring Boot 應用程式。

> [!NOTE]
> 
> 對於 [Apache Maven](http://maven.apache.org/) 的[適用於 Azure App Service Web Apps 的 Maven 外掛程式](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)，會提供 Azure App Service 和 Maven 專案的緊密整合，並且簡化開發人員將 Web 應用程式部署至 Azure App Service 的程序。

在使用 Maven 外掛程式之前，請在 Maven Central 上檢查外掛程式的最新可用版本：[![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22) 

## <a name="prerequisites"></a>必要條件

若要完成本教學課程中的步驟，您必須具備下列必要條件：

* Azure 訂用帳戶；如果您沒有 Azure 訂用帳戶，可以註冊[免費的 Azure 帳戶]。
* [Azure 命令列介面 (CLI)]。
* 最新的 [Java 開發套件 (JDK)] 1.7 版或更新版本。
* Apache 的 [Maven] 建置工具 (第 3 版)。
* [Git] 用戶端。

## <a name="clone-the-sample-spring-boot-web-app"></a>複製範例 Spring Boot Web 應用程式

在本節中，您會在本機複製已完成的 Spring Boot 應用程式，並且進行測試。

1. 開啟命令提示字元或終端機視窗，並建立本機目錄來保存您的 Spring Boot 應用程式，然後變更至該目錄；例如：
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- 或 --
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. 將 [Spring Boot Getting Started] 範例專案複製到您建立的目錄中；例如：
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. 將目錄變更至已完成的專案；例如：
   ```shell
   cd gs-spring-boot/complete
   ```

1. 使用 Maven 建立 JAR 檔案；例如：
   ```shell
   mvn clean package
   ```

1. 建立 Web 應用程式後，使用 Maven 啟動 Web 應用程式，例如：
   ```shell
   mvn spring-boot:run
   ```

1. 測試 Web 應用程式，方法是使用網頁瀏覽器在本機瀏覽它。 例如，如果您有 curl 可用，可以使用下列命令：
   ```shell
   curl http://localhost:8080
   ```

1. 您應該會看到顯示下列訊息：**Greetings from Spring Boot!**

## <a name="adjust-project-for-war-based-deployment-on-azure-app-service"></a>在 Azure App Service 上調整專案，以進行 WAR 型部署

在本節中，我們會快速調整 Spring Boot 專案，以在 Azure App Service 上部署為 WAR 檔案，其依預設會提供 Tomcat 做為執行階段。 針對此目的，有兩個檔案要修改：

- Maven `pom.xml` 檔案
- `Application` JAVA 類別

我們從 Maven 設定開始：

1. 開啟 `pom.xml`

1. 在頂端 `<artifactId>` 定義後新增 `<packaging>war</packaging>`：
   ```xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>

    <packaging>war</packaging>
   ```

1. 新增下列相依性：
   ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
   ```

現在開啟類別 `Application` (最好是在您的 IDE 已採用新相依性後)，並繼續下列修改：

1. 讓 [應用程式] 類別作為 `SpringBootServletInitializer` 的子類別：
   ```java
   @SpringBootApplication
   public class Application extends SpringBootServletInitializer {
     // ...
   }
   ```

1. 將下列方法新增至應用程式類別：
   ```java
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
           return application.sources(Application.class);
       }
   ```
1. 組織您的匯入項目，確保 `SpringApplicationBuilder` 和 `SpringBootServletInitializer` 已正確匯入。

您的應用程式現在已準備好部署到 Tomcat，和任何其他 Servlet 執行階段 (例如 Jetty)。

## <a name="add-the-maven-plugin-for-azure-app-service-web-apps"></a>為 Azure App Service Web Apps 新增 Maven 外掛程式

在本節中，我們會將可自動執行此應用程式整個部署的 Maven 外掛程式新增到 Azure App Service Web Apps。

1. 再次開啟 `pom.xml`。

1. 在 `<properties>` 中，使用屬性 `maven.build.timestamp.format` 設定自訂時間戳記格式。 因為 Azure App Service 會建立應用程式的公用 URL，此設定會用來產生部署名稱，並避免與其他使用者的即時部署發生衝突。
   ```xml
    <properties>
        <java.version>1.8</java.version>
        <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
    </properties>
   ```

1. 在 `<plugins>` 元素中，新增下列：
   ```xml
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
    </plugin>
   ```

有了這些設定後，您的 Maven 專案已準備好即時部署至 Azure App Service Web 應用程式。

## <a name="install-and-log-in-to-azure-cli"></a>安裝並登入 Azure CLI

讓 Maven 外掛程式部署 Spring Boot 應用程式最簡單且最輕鬆的方式是使用 [Azure CLI](https://docs.microsoft.com/cli/azure/)。 請確認您已進行安裝。

1. 使用 Azure CLI 登入您的 Azure 帳戶：
   
   ```shell
   az login
   ```
   
   依照指示完成登入程序。

## <a name="optionally-customize-pomxml-before-deploying"></a>(選擇性) 在部署之前自訂 pom.xml

在文字編輯器中開啟 Spring Boot 應用程式的 `pom.xml` 檔案，然後找出 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。 此元素外觀會類似下列範例：

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

您可以為 Maven 外掛程式修改數個值，這些元素的詳細描述可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得。 也就是說，有數個值值得在這篇文章中反白顯示：

| 元素 | 說明 |
|---|---|
| `<version>` | 指定[適用於 Azure Web 應用程式的 Maven 外掛程式]版本。 請檢查 [Maven 中央存放庫](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中所列的版本，確定您使用的是最新版本。 |
| `<resourceGroup>` | 指定目標資源群組，也就是此範例中的 `maven-plugin`。 如果該資源群組不存在，則系統會在部署期間建立它。 |
| `<appName>` | 指定 Web 應用程式的目標名稱。 在此範例中，目標名稱是 `maven-web-app-${maven.build.timestamp}`，在此範例中會附加 `${maven.build.timestamp}` 尾碼以避免發生衝突。 (時間戳記是選擇性的；您可以為應用程式名稱指定任何唯一的字串。) |
| `<region>` | 指定目標區域，在此範例中是 `westus`。 (完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。) |
| `<javaVersion>` | 指定 Web 應用程式的 Java 執行階段版本。 (完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。) |
| `<deploymentType>` | 指定 Web 應用程式的部署類型。 預設值為 `war`。 |

## <a name="build-and-deploy-your-web-app-to-azure"></a>建置 Web 應用程式並將其部署至 Azure

一旦您已設定本文上述章節中的所有設定，您已準備好將 Web 應用程式部署至 Azure。 若要這樣做，請使用下列步驟：

1. 如果您對 pom.xml 檔案進行任何變更，從您稍早使用的命令提示字元或終端機視窗，使用 Maven 重新建置 JAR 檔案；例如：
   ```shell
   mvn clean package
   ```

1. 使用 Maven 將您的 Web 應用程式部署至 Azure；例如：
   ```shell
   mvn azure-webapp:deploy
   ```

Maven 會將您的 Web 應用程式部署至 Azure；如果 Web 應用程式不存在，系統會加以建立。

已部署您的網站時，您就可以使用 [Azure 入口網站]來管理它。

* 您的 Web 應用程式會列在**應用程式服務** 中：

   ![列在 Azure 入口網站應用程式服務中的 Web 應用程式][AP01]

* Web 應用程式的 URL 會列在 Web 應用程式的 [概觀] 中：

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

## <a name="next-steps"></a>後續步驟

如需本文所討論之各種技術的詳細資訊，請參閱下列文章：

* [適用於 Azure Web 應用程式的 Maven 外掛程式]

* [從 Azure CLI 登入 Azure](/azure/xplat-cli-connect)

* [如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將容器化 Spring Boot 應用程式部署至 Azure](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [使用 Azure CLI 2.0 來建立 Azure 服務主體](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven 設定參考](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure 入口網站]: https://portal.azure.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[適用於 Azure Web 應用程式的 Maven 外掛程式]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
