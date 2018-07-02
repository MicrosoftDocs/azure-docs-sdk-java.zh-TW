---
title: 如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Azure Container Registry 中的 Spring Boot 應用程式部署至 Azure App Service
description: 本教學課程將逐步引導您藉由使用 Maven 外掛程式，將 Azure Container Registry 中的 Spring Boot 應用程式部署到 Azure App Service。
services: container-registry
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm;kevinzha
ms.date: 02/01/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 88eb64c07ad4f480dc2d2c2869e710c0ae910c4d
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090791"
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a>如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Azure Container Registry 中的 Spring Boot 應用程式部署至 Azure App Service

本文示範如何將範例 [Spring Boot] 應用程式部署至 Azure Container Registry，然後使用適用於 Azure Web 應用程式的 Maven 外掛程式，將應用程式部署至 Azure App Services。

> [!NOTE]
> 
> 針對 [Apache Maven](http://maven.apache.org/) 的適用於 Azure Web 應用程式的 Maven 外掛程式提供 Azure App Service 到 Maven 專案的緊密整合，並且簡化開發人員將 Web 應用程式部署至 Azure App Service 的程序。
> 
> 適用於 Azure Web 應用程式的 Maven 外掛程式目前可供預覽。 雖然未來計劃有額外功能，但是現在僅支援 FTP 發佈。
> 

## <a name="prerequisites"></a>先決條件

若要完成本教學課程中的步驟，您必須具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* [Azure 命令列介面 (CLI)]。
* 最新的 [Java 開發套件 (JDK)] 1.7 版或更新版本。
* Apache 的 [Maven] 建置工具 (第 3 版)。
* [Git] 用戶端。
* [Docker] 用戶端。

> [!NOTE]
>
> 由於本教學課程的虛擬化需求，您無法遵循本文中關於虛擬機器的步驟；您必須在啟用虛擬化功能的情況下使用實體電腦。
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a>在 Docker Web 應用程式上複製範例 Spring Boot

在本節中，您會在本機複製容器化 Spring Boot 應用程式，並且進行測試。

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

1. 將 [Spring Boot on Docker Getting Started] 範例專案複製到您所建立的目錄中；例如：
   ```shell
   git clone -b private-registry https://github.com/spring-guides/gs-spring-boot-docker
   ```

1. 將目錄變更至已完成的專案；例如：
   ```shell
   cd gs-spring-boot-docker/complete
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

1. 您應該會看到顯示下列訊息：**Hello Docker World**

   ![在本機瀏覽範例應用程式][SB01]

> [!NOTE]
>
> 當您在本機使用 Docker 時，可能會看到錯誤，指出您無法透過連接埠 2375 連線到 localhost。 如果發生這種情況，建議您在本機使用 Docker，無需使用 TLS。 若要這樣做，開啟您的 Docker 設定，然後勾選 [不使用 TLS 在 TCP://localhost:2375 上公開精靈] 選項。
>
> ![在本機的 TCP 連接埠 2375 上公開 Docker 精靈][TL01]

## <a name="create-an-azure-service-principal"></a>建立 Azure 服務主體

在本節中，您建立 Azure 服務主體，Maven 外掛程式會在將您的容器部署至 Azure 時使用該服務主體。

1. 開啟命令提示字元。

2. 使用 Azure CLI 登入您的 Azure 帳戶：
   ```azurecli
   az login
   ```
   依照指示完成登入程序。

3. 建立 Azure 服務主體：
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   其中：

   | 參數  |                    說明                     |
   |------------|----------------------------------------------------|
   | `uuuuuuuu` | 指定服務主體的使用者名稱。 |
   | `pppppppp` | 指定服務主體的密碼。  |


4. Azure 使用 JSON 回應，類似下列範例：
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
   > 當您設定 Maven 外掛程式以將您的容器部署至 Azure 時，您會使用此 JSON 回應中的值。 `aaaaaaaa`、`uuuuuuuu`、`pppppppp` 和 `tttttttt` 是預留位置值，在此範例中使用，在您於下一節設定 Maven `settings.xml` 檔案時，更方便將這些值對應至個別元素。
   >
   >

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>使用 Azure CLI 建立 Azure Container Registry

1. 開啟命令提示字元。

1. 登入您的 Azure 帳戶：
   ```azurecli
   az login
   ```

1. 為您將在這篇文章中使用的 Azure 資源建立資源群組：
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   將此範例中的 `wingtiptoysresources` 取代為資源群組的唯一名稱。

1. 在 Spring Boot 應用程式的資源群組中建立私用 Azure Container Registry： 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   將此範例中的 `wingtiptoysregistry` 取代為容器登錄的唯一名稱。

1. 擷取容器登錄的密碼：
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   Azure 會回應您的密碼，例如：
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a>將您的 Azure Container Registry 和 Azure 服務主體新增至您的 Maven 設定

1. 在文字編輯器中開啟您的 Maven`settings.xml` 檔案，這個檔案可能在如下列範例的路徑中：
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

2. 將這篇文章上一節的 Azure Container Registry 存取設定新增至 settings.xml 檔案中的 `<servers>` 集合；例如：

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   其中：

   |   元素    |                                 說明                                  |
   |--------------|------------------------------------------------------------------------------|
   |    `<id>`    |         包含私用 Azure Container Registry 名稱。          |
   | `<username>` |         包含私用 Azure Container Registry 名稱。          |
   | `<password>` | 包含您在這篇文章上一節擷取的密碼。 |


3. 將這篇文章稍早章節的 Azure 服務主體設定新增至 settings.xml 檔案中的 `<servers>` 集合；例如：

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

   |     元素     |                                                                                   說明                                                                                   |
   |-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `<id>`      |                                指定將您的 Web 應用程式部署至 Azure 時，Maven 用來查閱安全性設定的唯一名稱。                                |
   |   `<client>`    |                                                             包含服務主體的 `appId` 值。                                                             |
   |   `<tenant>`    |                                                            包含服務主體的 `tenant` 值。                                                             |
   |     `<key>`     |                                                           包含服務主體的 `password` 值。                                                            |
   | `<environment>` | 定義目標 Azure 雲端環境，也就是此範例中的 `AZURE`。 (環境的完整清單可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得) |


4. 儲存並關閉 settings.xml 檔案。

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a>建置您的 Docker 容器映像，並將它推送到您的 Azure Container Registry

1. 瀏覽至 Spring Boot 應用程式的已完成專案目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 pom.xml 檔案。

2. 使用本教學課程上一節的 Azure Container Registry 登入伺服器值來更新 pom.xml 檔案中的 `<properties>` 集合；例如：

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   其中：

   |           元素           |                                                                       說明                                                                       |
   |-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `<azure.containerRegistry>` |                                              指定私用 Azure Container Registry 名稱。                                               |
   |   `<docker.image.prefix>`   | 指定私用 Azure Container Registry 的 URL，它是藉由將 ".azurecr.io" 附加至私用容器登錄的名稱來衍生。 |


3. 確認 pom.xml 檔案中 Docker 外掛程式的 `<plugin>`，包含本教學課程上一個步驟中伺服器位址和登錄名稱的正確屬性。 例如︰

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   其中：

   |     元素     |                                       說明                                       |
   |-----------------|-----------------------------------------------------------------------------------------|
   |  `<serverId>`   |  指定屬性，該屬性包含私用 Azure Container Registry 的名稱。   |
   | `<registryUrl>` | 指定屬性，該屬性包含私用 Azure Container Registry 的 URL。 |


4. 瀏覽至您 Spring Boot 應用程式的已完成專案目錄，然後執行下列命令來重建應用程式，並將容器推送到您的 Azure Container Registry：

   ```
   mvn package docker:build -DpushImage 
   ```

5. 選擇性：瀏覽至 [Azure 入口網站]，並確認在容器登錄中有名為 **gs-spring-boot-docker** 的 Docker 容器映像。

   ![確認在 Azure 入口網站中的容器][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a>自訂 pom.xml，然後建立容器並將其部署至 Azure

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
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

您可以為 Maven 外掛程式修改數個值，這些元素的詳細描述可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得。 也就是說，有數個值值得在這篇文章中反白顯示：

| 元素 | 說明 |
|---|---|
| `<version>` | 指定[適用於 Azure Web 應用程式的 Maven 外掛程式]版本。 您應該檢查 [Maven 中央存放庫](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中所列的版本，確定您使用最新版本。 |
| `<authentication>` | 指定 Azure 的驗證資訊，在此範例中包含 `<serverId>` 元素，其中包含 `azure-auth`，Maven 使用該值來查閱 Maven settings.xml 檔案 (您在本文稍早章節中定義) 中的 Azure 服務主體值。 |
| `<resourceGroup>` | 指定目標資源群組，也就是此範例中的 `wingtiptoysresources`。 如果該資源群組不存在，則系統會在部署期間建立它。 |
| `<appName>` | 指定 Web 應用程式的目標名稱。 在此範例中，目標名稱是 `maven-linux-app-${maven.build.timestamp}`，在此範例中會附加 `${maven.build.timestamp}` 尾碼以避免發生衝突。 (時間戳記是選擇性的；您可以為應用程式名稱指定任何唯一的字串。) |
| `<region>` | 指定目標區域，在此範例中是 `westus`。 (完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。) |
| `<containerSettings>` | 指定屬性，該屬性包含容器的名稱和 URL。 |
| `<appSettings>` | 指定將您的 Web 應用程式部署至 Azure 時，Maven 使用的任何唯一設定。 在此範例中，`<property>` 元素包含子元素的名稱/值組，指定應用程式的連接埠。 |

> [!NOTE]
>
> 在此範例中變更連接埠號碼的設定，只有在您變更預設連接埠時才需要。
>

1. 如果您對 pom.xml 檔案進行任何變更，從您稍早使用的命令提示字元或終端機視窗，使用 Maven 重新建置 JAR 檔案；例如：
   ```shell
   mvn clean package
   ```

1. 使用 Maven 將您的 Web 應用程式部署至 Azure；例如：
   ```shell
   mvn azure-webapp:deploy
   ```

Maven 會將您的 Web 應用程式部署至 Azure；如果 Web 應用程式不存在，系統會加以建立。

> [!NOTE]
>
> 如果您在 pom.xml 檔案的 `<region>` 元素中指定的區域，在您啟動部署時沒有足夠的可用伺服器，您可能會看到類似下列範例的錯誤：
>
> ```
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> 如果發生這種情況，您可以指定另一個區域，然後重新執行 Maven 命令來部署應用程式。
>
>

已部署您的網站時，您就可以使用 [Azure 入口網站]來管理它。

* 您的 Web 應用程式會列在**應用程式服務** 中：

   ![列在 Azure 入口網站應用程式服務中的 Web 應用程式][AP01]

* Web 應用程式的 URL 會列在 Web 應用程式的 [概觀] 中：

   ![決定 Web 應用程式的 URL][AP02]

## <a name="next-steps"></a>後續步驟

如需本文所討論之各種技術的詳細資訊，請參閱下列文章：

* [適用於 Azure Web 應用程式的 Maven 外掛程式]

* [從 Azure CLI 登入 Azure](/azure/xplat-cli-connect)

* [使用 Azure CLI 2.0 來建立 Azure 服務主體](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven 設定參考](https://maven.apache.org/settings.html)

* [適用於 Maven 的 Docker 外掛程式]

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure 入口網站]: https://portal.azure.com/
[適用於 Azure Web 應用程式的 Maven 外掛程式]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service/containers/tutorial-custom-docker-image
[Docker]: https://www.docker.com/
[適用於 Maven 的 Docker 外掛程式]: https://github.com/spotify/docker-maven-plugin
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP02.png
[TL01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/TL01.png
