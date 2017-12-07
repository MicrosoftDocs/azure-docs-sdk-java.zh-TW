---
title: "如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Spring Boot 應用程式部署至 Azure"
description: "了解如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Spring Boot 應用程式部署至 Azure。"
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 8e5ad501f5c00ee1265878a643793f6e9754bb68
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-to-azure"></a>如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Spring Boot 應用程式部署至 Azure

本文示範如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將範例 Spring Boot 應用程式部署至 Azure App Services。

> [!NOTE]
> 
> 針對 [Apache Maven](http://maven.apache.org/) 的[適用於 Azure Web 應用程式的 Maven 外掛程式](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin)提供 Azure App Service 到 Maven 專案的緊密整合，並且簡化開發人員將 Web 應用程式部署至 Azure App Service 的程序。
> 
> 適用於 Azure Web 應用程式的 Maven 外掛程式目前可供預覽。 雖然未來計劃有額外功能，但是現在僅支援 FTP 發佈。
> 

## <a name="prerequisites"></a>必要條件

若要完成本教學課程中的步驟，您必須具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* [Azure 命令列介面 (CLI)]。
* 最新的 [Java 開發套件 (JDK)] 1.7 版或更新版本。
* Apache 的 [Maven] 建置工具 (第 3 版)。
* [Git] 用戶端。

## <a name="clone-the-sample-spring-boot-web-app"></a>複製範例 Spring Boot Web 應用程式

在本節中，您會在本機複製已完成 Spring Boot 應用程式，並且進行測試。

1. 開啟命令提示字元或終端機視窗，並建立本機目錄來保存您的 Spring Boot 應用程式，然後變更至該目錄；例如：
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- 或 --
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. 將 [Spring Boot Getting Started] 範例專案複製到您建立的目錄中；例如：
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
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

## <a name="create-an-azure-service-principal"></a>建立 Azure 服務主體

在本節中，您建立 Azure 服務主體，Maven 外掛程式會在將您的 Web 應用程式部署至 Azure 時使用該服務主體。

1. 開啟命令提示字元。

1. 使用 Azure CLI 登入您的 Azure 帳戶：
   ```shell
   az login
   ```
   依照指示完成登入程序。

1. 建立 Azure 服務主體：
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   其中 `uuuuuuuu` 是使用者名稱，`pppppppp` 是服務主體的密碼。

1. Azure 使用 JSON 回應，類似下列範例：
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > 當您設定 Maven 外掛程式以將您的 Web 應用程式部署至 Azure 時，您會使用此 JSON 回應中的值。 `aaaaaaaa`、`uuuuuuuu`、`pppppppp` 和 `tttttttt` 是預留位置值，在此範例中使用，在您於下一節設定 Maven `settings.xml` 檔案時，更方便將這些值對應至個別元素。
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a>設定 Maven 以使用您的 Azure 服務主體

在本節中，您使用 Azure 服務主體的值，設定將您的 Web 應用程式部署至 Azure 時，Maven 使用的驗證。

1. 在文字編輯器中開啟您的 Maven`settings.xml` 檔案，這個檔案可能在如下列範例的路徑中：
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. 將本教學課程上一節的 Azure 服務主體設定新增至 settings.xml 檔案中的 `<servers>` 集合；例如：

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   其中：
   元素 | 說明
   ---|---|---
   `<id>` | 指定將您的 Web 應用程式部署至 Azure 時，Maven 用來查閱安全性設定的唯一名稱。
   `<client>` | 包含服務主體的 `appId` 值。
   `<tenant>` | 包含服務主體的 `tenant` 值。
   `<key>` | 包含服務主體的 `password` 值。
   `<environment>` | 定義目標 Azure 雲端環境，也就是此範例中的 `AZURE`。 (環境的完整清單可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得)

1. 儲存並關閉 settings.xml 檔案。

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-to-azure"></a>選擇性：將您的 Web 應用程式部署至 Azure 之前自訂 pom.xml

在文字編輯器中開啟 Spring Boot 應用程式的 `pom.xml` 檔案，然後找出 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。 此元素外觀會類似下列範例：

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

您可以為 Maven 外掛程式修改數個值，這些元素的詳細描述可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得。 也就是說，有數個值值得在這篇文章中反白顯示：

元素 | 說明
---|---|---
`<version>` | 指定[適用於 Azure Web 應用程式的 Maven 外掛程式]版本。 您應該檢查 [Maven 中央存放庫](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中所列的版本，確定您使用最新版本。
`<authentication>` | 指定 Azure 的驗證資訊，在此範例中包含 `<serverId>` 元素，其中包含 `azure-auth`，Maven 使用該值來查閱 Maven settings.xml 檔案 (您在本文稍早章節中定義) 中的 Azure 服務主體值。
`<resourceGroup>` | 指定目標資源群組，也就是此範例中的 `maven-plugin`。 如果該資源群組不存在，則系統會在部署期間建立它。
`<appName>` | 指定 Web 應用程式的目標名稱。 在此範例中，目標名稱是 `maven-web-app-${maven.build.timestamp}`，在此範例中會附加 `${maven.build.timestamp}` 尾碼以避免發生衝突。 (時間戳記是選擇性的；您可以為應用程式名稱指定任何唯一的字串。)
`<region>` | 指定目標區域，在此範例中是 `westus`。 (完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。)
`<javaVersion>` | 指定 Web 應用程式的 Java 執行階段版本。 (完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。)
`<deploymentType>` | 指定 Web 應用程式的部署類型。 雖然其他部署類型的支援正在開發中，現在只有 `ftp` 受到支援。
`<resources>` | 指定當 Maven 將 Web 應用程式部署至 Azure 時使用的資源和目標目的地。 在此範例中，兩個 `<resource>` 元素會指定 Maven 將會部署 Web 應用程式的 JAR 檔案和 Spring Boot 專案的 web.config 檔案。

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
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[適用於 Azure Web 應用程式的 Maven 外掛程式]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
