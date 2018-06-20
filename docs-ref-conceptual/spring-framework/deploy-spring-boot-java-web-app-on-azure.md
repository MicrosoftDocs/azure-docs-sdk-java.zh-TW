---
title: 透過 Azure App Service 將 Spring Boot 應用程式部署至雲端
description: 本教學課程將逐步引導開發人員使用 Azure App Service 將 Spring Boot 入門 Web 應用程式部署至雲端。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: asirveda;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: adf779e2ba6ca73ea3a2406613f9622cc9ecbf99
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954521"
---
# <a name="deploy-a-spring-boot-application-to-the-cloud-with-azure-app-service"></a>透過 Azure App Service 將 Spring Boot 應用程式部署至雲端

本教學課程將引導您完成建立範例 [Spring Boot] Getting Started Web 應用程式，並將它部署到 [Azure App Service]。

### <a name="prerequisites"></a>先決條件

若要完成本教學課程中的步驟，您必須具備下列項目：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* 最新的 [Java 開發工具組 (JDK)]。
* Apache 的 [Maven] 建置工具 (第 3 版)。
* [Git] 用戶端。

## <a name="create-the-spring-boot-getting-started-web-app"></a>建立 Spring Boot Getting Started Web 應用程式

下列步驟將引導您完成建立簡單 Spring Boot Web 應用程式並在本機測試所需的步驟。

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

1. 將 [Spring Boot Getting Started] 範例專案複製到您剛建立的目錄中；例如：
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. 將目錄變更至已完成的專案；例如：
   ```
   cd gs-spring-boot
   cd complete
   ```

1. 使用 Maven 建立 JAR 檔案；例如：
   ```
   mvn package
   ```

1. 建立 Web 應用程式之後，將目錄變更至 JAR 檔案並啟動 Web 應用程式；例如：
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. 使用網頁瀏覽器瀏覽至 http://localhost:8080 來測試 Web 應用程式；如果有可用的 curl，請使用類似下列範例的語法：
   ```
   curl http://localhost:8080
   ```

1. 您應該會看到顯示下列訊息：**Greetings from Spring Boot!**

   ![瀏覽範例應用程式][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a>建立 Azure Web 應用程式以搭配 Java 使用

下列步驟將引導您完成建立 Azure Web 應用程式、設定 Java 的必要設定及設定您的 FTP 認證的步驟。

1. 瀏覽至 [Azure 入口網站]並登入。

1. 當您登入 Azure 入口網站的帳戶之後，按一下 [應用程式服務] 的功能表圖示：
   
   ![Azure 入口網站][AZ01]

1. 當 [應用程式服務] 頁面顯示時，按一下 [+ 新增] 建立新的應用程式服務。

   ![建立應用程式服務][AZ02]

1. 當 Web 應用程式範本清單顯示時，按一下基本 Microsoft Web 應用程式的連結。

   ![Web 應用程式範本][AZ03]

1. 當 Web 應用程式範本的資訊頁面顯示時，按一下 [建立]。

   ![建立 Web 應用程式][AZ04]

1. 為您的 Web 應用程式提供唯一的名稱並指定任何其他設定，然後按一下 [建立]。

   ![建立 Web 應用程式設定][AZ05]

1. 建立您的 Web 應用程式之後，按一下 [應用程式服務] 的功能表圖示，然後按一下您新建立的 Web 應用程式：

   ![列出 Web 應用程式][AZ06]

1. 當您的 Web 應用程式顯示時，使用下列步驟指定 Java 版本：

   a. 按一下 [應用程式設定] 功能表項目。

   b. 針對 Java 版本選擇 [Java 8]。

   c. 針對 Java 次要版本選擇 [最新]。

   d. 針對 Web 容器選擇 [Newest Tomcat 8.5] \(最新的 Tomcat 8.5) (實際上不會使用此容器；Azure 將會使用您 Spring Boot 應用程式中的容器)。

   e. 按一下 [檔案] 。

   ![應用程式設定][AZ07]

1. 使用下列步驟指定您的 FTP 部署認證：

   a. 按一下 [部署認證] 功能表項目。

   b. 指定您的使用者名稱和密碼。

   c. 按一下 [檔案] 。

   ![指定部署認證][AZ08]

1. 使用下列步驟擷取您的 FTP 連線資訊：

   a. 按一下 [部署認證] 功能表項目。

   b. 複製您完整的 FTP 使用者名稱和 URL，並加以儲存供本教學課程的下一節使用。

   ![FTP URL 和認證][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a>將您的 Spring Boot Web 應用程式部署到 Azure

下列步驟將引導您完成將 Spring Boot Web 應用程式部署到 Azure 的步驟。

1. 開啟 Windows 記事本之類的文字編輯器，並將下列文字貼入新文件，然後將檔案儲存為 *web.config*：
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. 當您將 *web.config* 檔案儲存至系統之後，使用本教學課程上一節的 URL、使用者名稱和密碼透過 FTP 連線到您的 Web 應用程式。 例如︰
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. 將遠端目錄變更至您 Web 應用程式的根資料夾 (在 */site/wwwroot*中)，然後複製您 Spring Boot 應用程式中的 JAR 檔案和上述 *web.config*。 例如︰
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. 當您將 JAR 和 *web.config* 檔案部署到 Web 應用程式之後，您必須使用 Azure 入口網站重新啟動 Web 應用程式：

   ![重新啟動您的 Web 應用程式][AZ10]

1. 使用網頁瀏覽器瀏覽至 Web 應用程式的 URL 來測試 Web 應用程式；如果有可用的 curl，請使用類似下列範例的語法：
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. 您應該會看到顯示下列訊息：**Greetings from Spring Boot!**

   ![瀏覽範例應用程式][SB02]

## <a name="next-steps"></a>後續步驟

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure Container Service 中的 Linux](deploy-spring-boot-java-app-on-linux.md)

* [將 Spring Boot 應用程式部署到 Azure Container Service 中的 Kubernetes 叢集](deploy-spring-boot-java-app-on-kubernetes.md)

如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。

如需使用 FTP 將 Web 應用程式部署到 Azure 的其他資訊，請參閱[使用 FTP/S 將您的應用程式部署至 Azure App Service]。

如需 Spring Boot 範例專案的進一步詳細資訊，請參閱 [Spring Boot Getting Started]。

如需開始使用您自己的 Spring Boot 應用程式的說明，請參閱 **Spring Initializr** (網址為 https://start.spring.io/)。

如需如何設定 Web 應用程式之其他設定的詳細資訊，請參閱[在 Azure App Service 中設定 Web 應用程式]。

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[Azure 入口網站]: https://portal.azure.com/
[在 Azure App Service 中設定 Web 應用程式]: /azure/app-service/web-sites-configure
[使用 FTP/S 將您的應用程式部署至 Azure App Service]: https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java 開發工具組 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-web-app-on-azure/SB01.png
[SB02]: ./media/deploy-spring-boot-java-web-app-on-azure/SB02.png

[AZ01]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ01.png
[AZ02]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ02.png
[AZ03]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ03.png
[AZ04]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ04.png
[AZ05]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ05.png
[AZ06]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ06.png
[AZ07]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ07.png
[AZ08]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ08.png
[AZ09]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ09.png
[AZ10]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ10.png
