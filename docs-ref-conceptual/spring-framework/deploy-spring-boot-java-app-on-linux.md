---
title: 在適用於容器的 Azure App Service 上部署 Spring Boot Web App
description: 本教學課程會逐步引導您將 Spring Boot 應用程式部署為 Microsoft Azure 上之 Linux Web 應用程式的步驟。
services: azure app service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: azure app service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 407b852e24ef88d2fb075bd064f1acf2b107ddc1
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270860"
---
# <a name="deploy-a-spring-boot-application-on-azure-app-service-for-container"></a>在適用於容器的 Azure App Service 上部署 Spring Boot 應用程式

本教學課程會逐步引導您使用 [Docker] 將 [Spring Boot] 應用程式容器化，並且將您的 Docker 映像部署至 [Azure App Service](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro) 中的 Linux 主機。

## <a name="prerequisites"></a>必要條件

若要完成本教學課程中的步驟，您必須具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* [Azure 命令列介面 (CLI)]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven] 建置工具 (第 3 版)。
* [Git] 用戶端。
* [Docker] 用戶端。

> [!NOTE]
>
> 由於本教學課程的虛擬化需求，您無法遵循本文中關於虛擬機器的步驟；您必須在啟用虛擬化功能的情況下使用實體電腦。
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>建立 Spring Boot on Docker Getting Started Web 應用程式

下列步驟將引導您完成建立簡單 Spring Boot Web 應用程式，並在本機測試所需的步驟。

1. 開啟命令提示字元並建立本機目錄來保存您的應用程式，然後變更至該目錄；例如：
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- 或 --
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. 將 [Spring Boot on Docker Getting Started] 範例專案複製到您所建立的目錄中；例如：
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. 將目錄變更至已完成的專案；例如：
   ```
   cd gs-spring-boot-docker/complete
   ```

1. 使用 Maven 建立 JAR 檔案；例如：
   ```
   mvn package
   ```

1. 建立 Web 應用程式之後，將目錄變更為 JAR 檔案所在的 `target` 目錄，並啟動 Web 應用程式；例如：
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. 測試 Web 應用程式，方法是使用網頁瀏覽器在本機瀏覽它。 例如，如果您有 curl 可用，並將 Tomcat 伺服器設定為在連接埠 80 上執行：
   ```
   curl http://localhost
   ```

1. 您應該會看到下列訊息：**Hello Docker World!**

   ![在本機瀏覽範例應用程式][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>建立 Azure Container Registry 以用作私人 Docker 登錄

下列步驟會逐步引導您使用 Azure 入口網站來建立 Azure Container Registry。

> [!NOTE]
>
> 如果您想要使用 Azure CLI 而不是 Azure 入口網站，請遵循下列[使用 Azure CLI 2.0 建立私人 Docker 容器登錄](/azure/container-registry/container-registry-get-started-azure-cli)中的步驟。
>

1. 瀏覽至 [Azure 入口網站]並登入。

   一旦您已在 Azure 入口網站登入您的帳戶後，就可以遵循[使用 Azure 入口網站建立私人 Docker 容器登錄]文章中的步驟，為便於了解，會在下列步驟中加以釋義。

1. 依序按一下功能表的 [+ 新增]  圖示、[容器]  ，以及 [Azure Container Registry]  。
   
   ![建立新的 Azure Container Registry][AR01]

1. 當 Azure Container Registry 範本的資訊頁面顯示時，按一下 [建立]  。 

   ![建立新的 Azure Container Registry][AR02]

1. 當 [建立容器登錄]  頁面顯示時，輸入您的 [登錄名稱]  和 [資源群組]  ，針對 [管理使用者]  選擇 [啟用]  ，然後按一下 [建立]  。

   ![設定 Azure Container Registry 設定][AR03]

1. 一旦建立容器登錄之後，瀏覽至 Azure 入口網站中的容器登錄，然後按一下 [存取金鑰]  。 將後續步驟的使用者名稱和密碼記下。

   ![Azure Container Registry 存取金鑰][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>設定要使用 Azure Container Registry 存取金鑰的 Maven

1. 瀏覽至 Spring Boot 應用程式的已完成專案目錄 (例如："*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 " */users/robert/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 pom.xml  檔案。

1. 使用最新版的 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) 以及本教學課程上一節的 Azure Container Registry 登入伺服器值和存取設定，更新 pom.xml  檔案中的 `<properties>` 集合。 例如︰

   ```xml
   <properties>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <username>wingtiptoysregistry</username>
      <password>{put your Azure Container Registry access key here}</password>
   </properties>
   ```

1. 將 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) 新增至 pom.xml  檔案中的 `<plugins>` 集合，然後在 `<from>/<image>` 指定基底映像、在 `<to>/<image>` 指定最終映像名稱，並在 `<to>/<auth>` 指定上一節中的使用者名稱和密碼。 例如︰

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
            <auth>
               <username>${username}</username>
               <password>${password}</password>
            </auth>
        </to>
     </configuration>
   </plugin>
   ```

1. 瀏覽至您 Spring Boot 應用程式的已完成專案目錄，然後執行下列命令來重建應用程式，並將容器推送到您的 Azure Container Registry：

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> 當您使用 Jib 將映像推送到 Azure Container Registry 時，映像將不接受 *Dockerfile*；如需詳細資訊，請參閱[此文件](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html)。
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>使用您的容器映像在 Azure App Service 上建立 Linux 的 Web 應用程式

1. 瀏覽至 [Azure 入口網站]並登入。

2. 依序按一下功能表的 [+ 建立資源]  圖示、[Web]  ，以及 [用於容器的 Web App]  。
   
   ![在 Azure 入口網站中建立新的 Web 應用程式][LX01]

3. 當 [Linux 上的 Web 應用程式]  頁面顯示時，輸入下列資訊：

   a. 為 [應用程式名稱]  輸入唯一名稱；例如："wingtiptoyslinux" 

   b. 從下拉式清單選擇 [訂用帳戶]  。

   c. 選擇現有 [資源群組]  ，或指定名稱以建立新的資源群組。

   d. 選擇 *Linux* 作為 **OS**。

   e. 按一下 [App Service 方案/位置]  ，並選擇現有的 App Service 方案，或按一下 [新建]  以建立新的 App Service 方案。

   f. 按一下 [設定容器]  ，並輸入下列資訊：

   * 選擇 [單一容器]  和 [Azure Container Registry]  。

   * **登錄**：選擇您先前建立的容器名稱，例如："wingtiptoysregistry" 

   * **映像**：選擇映像名稱，例如："gs-spring-boot-docker" 
   
   * **標籤**︰選擇映像的標記，例如："latest" 
   
   * **啟動檔案**：將其保留為空白，因為映像已有啟動命令
   
   e. 在輸入上述所有資訊後，按一下 [套用]  。

   ![設定 Web 應用程式設定][LX02]

4. 按一下頁面底部的 [新增]  。

> [!NOTE]
>
> Azure 會自動將網際網路要求對應到標準連接埠 80 或 8080 上執行的內嵌 Tomcat 伺服器。 不過，如果您將內嵌 Tomcat 伺服器設定為在自訂連接埠上執行時，則必須將環境變數新增至您的 web 應用程式，其可定義內嵌 Tomcat 伺服器的連接埠。 若要這樣做，請使用下列步驟：
>
> 1. 瀏覽至 [Azure 入口網站]並登入。
> 
> 2. 按一下 **App Service** 的圖示，並從清單中選取您的 Web 應用程式。
>
> 4. 按一下 [組態]  。 (下圖中的項目 #1。)
>
> 5. 在 [應用程式設定]  區段中，新增名為 **PORT** 的新設定，並輸入您的自訂連接埠號碼作為其值。 (下圖中的項目 #2、#3、#4。)
>
> 6. 按一下 [檔案]  。 (下列映像中的項目 #5。)
>
> ![在 Azure 入口網站中儲存自訂連接埠號碼][LX03]
>

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

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他資源

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)
* [將 Spring Boot 應用程式部署到 Azure Container Service 中的 Kubernetes 叢集](deploy-spring-boot-java-app-on-kubernetes.md)

如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。

如需 Spring Boot on Docker 範例專案的進一步詳細資訊，請參閱 [Spring Boot on Docker Getting Started]。

如需開始使用您自己的 Spring Boot 應用程式的說明，請參閱 **Spring Initializr**，網址為 https://start.spring.io/。

如需開始建立簡單 Spring Boot 應用程式的相關詳細資訊，請參閱 Spring Initializr，網址為 https://start.spring.io/。

如需如何搭配 Azure 使用自訂 Docker 映像的其他範例，請參閱[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]。

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure 入口網站]: https://portal.azure.com/
[使用 Azure 入口網站建立私人 Docker 容器登錄]: /azure/container-registry/container-registry-get-started-portal
[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[使用 Azure DevOps 和 Java]: /azure/devops/java/
[Maven]: http://maven.apache.org/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-linux/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-linux/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-linux/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-linux/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-linux/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-linux/AR04.png

[LX01]: ./media/deploy-spring-boot-java-app-on-linux/LX01.png
[LX02]: ./media/deploy-spring-boot-java-app-on-linux/LX02.png
[LX03]: ./media/deploy-spring-boot-java-app-on-linux/LX03.png
