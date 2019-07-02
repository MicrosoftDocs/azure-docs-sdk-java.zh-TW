---
title: 透過 Maven 和 Azure 將 Spring Boot JAR 檔案應用程式部署到雲端
description: 了解如何使用適用於 Azure Web App for Linux 的 Maven 外掛程式，將 Spring Boot 應用程式部署至雲端。
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: brborges
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: b133290d1f14429cbf36d6ed5a67d27e1a637593
ms.sourcegitcommit: 599405a9ce892d75073ef0776befa2fa22407b4c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2019
ms.locfileid: "67237605"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a>將 Spring Boot JAR 檔案 Web 應用程式部署至 Linux 上的 Azure App Service

本文示範如何使用[適用於 App Service Web Apps 的 Maven 外掛程式](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)，將 Spring Boot 應用程式封裝成 Java SE JAR，並部署至 [Linux 上的 Azure App Service](/azure/app-service/containers/)。 若要將應用程式相依性、執行階段及組態合併至單一可部署成品，請從 [Tomcat 與 WAR 檔案](/azure/app-service/containers/quickstart-java)中選擇 Java SE 部署。


如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

## <a name="prerequisites"></a>必要條件

若要完成本教學課程中的步驟，您必須已安裝以下項目並設定完成：

* [Azure CLI](/cli/azure/)，安裝在本機或透過 [Azure Cloud Shell](https://shell.azure.com)安裝。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven](https://maven.apache.org/) 3.0 版。
* [Git](https://git-scm.com/downloads) 用戶端。

## <a name="install-and-sign-in-to-azure-cli"></a>安裝並登入 Azure CLI

讓 Maven 外掛程式部署 Spring Boot 應用程式最簡單且最輕鬆的方式是使用 [Azure CLI](/cli/azure/)。

使用 Azure CLI 登入您的 Azure 帳戶：
   
   ```shell
   az login
   ```
   
依照指示完成登入程序。

## <a name="clone-the-sample-app"></a>複製範例應用程式

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

1. 您應該會看到下列訊息：**Greetings from Spring Boot!**

## <a name="configure-maven-plugin-for-azure-app-service"></a>為 Azure App Service 設定 Maven 外掛程式

在本節中，您將設定 Spring Boot 專案`pom.xml`，以便 Maven 可以將應用程式部署至 Linux 上的 Azure App Service。

1. 在程式碼編輯器中，開啟 `pom.xml`。

2. 在 pom.xml 的 `<build>` 區段中，於 `<plugins>` 標記內新增下列 `<plugin>` 項目。

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.6.0</version>
   </plugin>
   ```

3. 接著您可以設定部署、在命令提示字元中執行 Maven 命令 `mvn azure-webapp:config` 並使用**數字**在提示中選擇這些選項：
    * **OS**：Linux  
    * **javaVersion**：jre8
    * **runtimeStack**：jre8

看到 **Confirm (Y/N)** 提示時，按下 **'y'** 完成設定。

```cmd
~@Azure:~/gs-spring-boot/complete$ mvn azure-webapp:config
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< org.springframework:gs-spring-boot >-----------------
[INFO] Building gs-spring-boot 0.1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.6.0:config (default-cli) @ gs-spring-boot ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. jre8 [*]
2. java11
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. jre8
3. TOMCAT 8.5 [*]
4. WILDFLY 14
Enter index to use: 2
Please confirm webapp properties
AppName : gs-spring-boot-1559091271202
ResourceGroup : gs-spring-boot-1559091271202-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : JAVA 8-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

4. 將 `<appSettings>` 區段新增到 `<azure-webapp-maven-plugin>` 的 `<configuration>` 區段，以接聽連接埠 *80*。

    ```xml
   <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.6.0</version>
       <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>gs-spring-boot-1559091271202-rg</resourceGroup>
          <appName>gs-spring-boot-1559091271202</appName>
          <region>westeurope</region>
          <pricingTier>P1V2</pricingTier>

          <!-- Begin of App Settings  -->
          <appSettings>
             <property>
                   <name>JAVA_OPTS</name>
                   <value>-Dserver.port=80</value>
             </property>
          </appSettings>
          <!-- End of App Settings  -->
          ...
         </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a>將應用程式部署至 Azure

一旦您已設定本文上述章節中的所有設定，您已準備好將 Web 應用程式部署至 Azure。 若要這樣做，請使用下列步驟：

1. 如果您對 pom.xml  檔案進行任何變更，從您稍早使用的命令提示字元或終端機視窗，使用 Maven 重新建置 JAR 檔案；例如：
   ```shell
   mvn clean package
   ```

1. 使用 Maven 將您的 Web 應用程式部署至 Azure；例如：
   ```shell
   mvn azure-webapp:deploy
   ```

Maven 會將您的 Web 應用程式部署至 Azure；如果 Web 應用程式或 Web 應用程式方案不存在，系統會為您建立。

網站部署完成時，您就可以使用 [Azure 入口網站]來管理。

* 您的 Web 應用程式會列在**應用程式服務** 中：

   ![列在 Azure 入口網站應用程式服務中的 Web 應用程式][AP01]

* Web 應用程式的 URL 會列在 Web 應用程式的 [概觀]  中：

   ![決定 Web 應用程式的 URL][AP02]

使用與之前相同的 cURL 命令來確認部署是否成功；請使用入口網站中的 Web 應用程式 URL，不要使用 `localhost`。 您應該會看到下列訊息：**Greetings from Spring Boot!** 

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他資源

如需本文所討論之各種技術的詳細資訊，請參閱下列文章：

* [適用於 Azure Web 應用程式的 Maven 外掛程式]

* [如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將容器化 Spring Boot 應用程式部署至 Azure](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [使用 Azure CLI 2.0 來建立 Azure 服務主體](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven 設定參考](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /java/azure/
[Azure 入口網站]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[適用於 Azure Web 應用程式的 Maven 外掛程式]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
