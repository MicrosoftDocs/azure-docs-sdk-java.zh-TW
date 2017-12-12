---
title: "將 Spring Boot 應用程式部署到 Azure App Service"
description: "本教學課程將引導開發人員完成將 Spring Boot Getting Started Web 應用程式部署到 Azure App Service 的步驟。"
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: b520cc80360f8162c929bb2cc88c24311a7e20f8
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a><span data-ttu-id="1d7e2-103">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1d7e2-103">Deploy a Spring Boot Application to the Azure App Service</span></span>

<span data-ttu-id="1d7e2-104">本教學課程將引導您完成建立範例 [Spring Boot] Getting Started Web 應用程式，並將它部署到 [Azure App Service]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-104">This tutorial will walk you though creating a sample [Spring Boot] Getting Started web app and deploying it to [Azure App Service].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="1d7e2-105">必要條件</span><span class="sxs-lookup"><span data-stu-id="1d7e2-105">Prerequisites</span></span>

<span data-ttu-id="1d7e2-106">若要完成本教學課程中的步驟，您必須具備下列項目：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-106">In order to complete the steps in this tutorial, you need to have the following:</span></span>

* <span data-ttu-id="1d7e2-107">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="1d7e2-108">最新的 [Java 開發工具組 (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-108">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="1d7e2-109">Apache 的 [Maven] 建置工具 (第 3 版)。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-109">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="1d7e2-110">[Git] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-110">A [Git] client.</span></span>

## <a name="create-the-spring-boot-getting-started-web-app"></a><span data-ttu-id="1d7e2-111">建立 Spring Boot Getting Started Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="1d7e2-111">Create the Spring Boot Getting Started web app</span></span>

<span data-ttu-id="1d7e2-112">下列步驟將引導您完成建立簡單 Spring Boot Web 應用程式並在本機測試所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-112">The following steps will walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="1d7e2-113">開啟命令提示字元並建立本機目錄來保存您的應用程式，然後變更至該目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-113">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="1d7e2-114">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="1d7e2-114">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="1d7e2-115">將 [Spring Boot Getting Started] 範例專案複製到您剛建立的目錄中；例如：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-115">Clone the [Spring Boot Getting Started] sample project into the directory you just created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. <span data-ttu-id="1d7e2-116">將目錄變更至已完成的專案；例如：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-116">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot
   cd complete
   ```

1. <span data-ttu-id="1d7e2-117">使用 Maven 建立 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-117">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="1d7e2-118">建立 Web 應用程式之後，將目錄變更至 JAR 檔案並啟動 Web 應用程式；例如：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-118">Once the web app has been created, change directory to the JAR file and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. <span data-ttu-id="1d7e2-119">使用網頁瀏覽器瀏覽至 http://localhost:8080 來測試 Web 應用程式；如果有可用的 curl，請使用類似下列範例的語法：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-119">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="1d7e2-120">您應該會看到顯示下列訊息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="1d7e2-120">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![瀏覽範例應用程式][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a><span data-ttu-id="1d7e2-122">建立 Azure Web 應用程式以搭配 Java 使用</span><span class="sxs-lookup"><span data-stu-id="1d7e2-122">Create an Azure web app for use with Java</span></span>

<span data-ttu-id="1d7e2-123">下列步驟將引導您完成建立 Azure Web 應用程式、設定 Java 的必要設定及設定您的 FTP 認證的步驟。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-123">The following steps will walk you through the steps to create an Azure Web App, configure the required settings for Java, and configure your FTP credentials.</span></span>

1. <span data-ttu-id="1d7e2-124">瀏覽至 [Azure 入口網站]並登入。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-124">Browse to the [Azure portal] and log in.</span></span>

1. <span data-ttu-id="1d7e2-125">當您登入 Azure 入口網站的帳戶之後，按一下 [應用程式服務] 的功能表圖示：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-125">Once you have logged into your account on the Azure portal, click the menu icon for **App Services**:</span></span>
   
   ![Azure 入口網站][AZ01]

1. <span data-ttu-id="1d7e2-127">當 [應用程式服務] 頁面顯示時，按一下 [+ 新增] 建立新的應用程式服務。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-127">When the **App Services** page is displayed, click **+ Add** to create a new App Service.</span></span>

   ![建立應用程式服務][AZ02]

1. <span data-ttu-id="1d7e2-129">當 Web 應用程式範本清單顯示時，按一下基本 Microsoft Web 應用程式的連結。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-129">When the list of web app templates is displayed, click the link for the basic Microsoft Web App.</span></span>

   ![Web 應用程式範本][AZ03]

1. <span data-ttu-id="1d7e2-131">當 Web 應用程式範本的資訊頁面顯示時，按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-131">When the information page for the Web App template is displayed, click **Create**.</span></span>

   ![建立 Web 應用程式][AZ04]

1. <span data-ttu-id="1d7e2-133">為您的 Web 應用程式提供唯一的名稱並指定任何其他設定，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-133">Provide a unique name for your web app and specify any additional settings, and then **Create**.</span></span>

   ![建立 Web 應用程式設定][AZ05]

1. <span data-ttu-id="1d7e2-135">建立您的 Web 應用程式之後，按一下 [應用程式服務] 的功能表圖示，然後按一下您新建立的 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-135">Once your web app has been created, click the menu icon for **App Services**, and then click your newly-created web app:</span></span>

   ![列出 Web 應用程式][AZ06]

1. <span data-ttu-id="1d7e2-137">當您的 Web 應用程式顯示時，使用下列步驟指定 Java 版本：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-137">When your web app is displayed, specify the Java version by using the following steps:</span></span>

   <span data-ttu-id="1d7e2-138">a.</span><span class="sxs-lookup"><span data-stu-id="1d7e2-138">a.</span></span> <span data-ttu-id="1d7e2-139">按一下 [應用程式設定] 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-139">Click the **Application Settings** menu item.</span></span>

   <span data-ttu-id="1d7e2-140">b.這是另一個 C# 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-140">b.</span></span> <span data-ttu-id="1d7e2-141">針對 Java 版本選擇 [Java 8]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-141">Choose **Java 8** for the Java version.</span></span>

   <span data-ttu-id="1d7e2-142">c.</span><span class="sxs-lookup"><span data-stu-id="1d7e2-142">c.</span></span> <span data-ttu-id="1d7e2-143">針對 Java 次要版本選擇 [最新]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-143">Choose **Newest** for the minor Java version.</span></span>

   <span data-ttu-id="1d7e2-144">d.</span><span class="sxs-lookup"><span data-stu-id="1d7e2-144">d.</span></span> <span data-ttu-id="1d7e2-145">針對 Web 容器選擇 [Newest Tomcat 8.5] \(最新的 Tomcat 8.5)</span><span class="sxs-lookup"><span data-stu-id="1d7e2-145">Choose **Newest Tomcat 8.5** for the web container.</span></span> <span data-ttu-id="1d7e2-146">(實際上不會使用此容器；Azure 將會使用您 Spring Boot 應用程式中的容器)。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-146">(This container will not actually be used; Azure will use the container from your Spring Boot application.)</span></span>

   <span data-ttu-id="1d7e2-147">e.</span><span class="sxs-lookup"><span data-stu-id="1d7e2-147">e.</span></span> <span data-ttu-id="1d7e2-148">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-148">Click **Save**.</span></span>

   ![應用程式設定][AZ07]

1. <span data-ttu-id="1d7e2-150">使用下列步驟指定您的 FTP 部署認證：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-150">Specify your FTP deployment credentials by using the following steps:</span></span>

   <span data-ttu-id="1d7e2-151">a.</span><span class="sxs-lookup"><span data-stu-id="1d7e2-151">a.</span></span> <span data-ttu-id="1d7e2-152">按一下 [部署認證] 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-152">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="1d7e2-153">b.這是另一個 C# 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-153">b.</span></span> <span data-ttu-id="1d7e2-154">指定您的使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-154">Specify your username and password.</span></span>

   <span data-ttu-id="1d7e2-155">c.</span><span class="sxs-lookup"><span data-stu-id="1d7e2-155">c.</span></span> <span data-ttu-id="1d7e2-156">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-156">Click **Save**.</span></span>

   ![指定部署認證][AZ08]

1. <span data-ttu-id="1d7e2-158">使用下列步驟擷取您的 FTP 連線資訊：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-158">Retrieve your FTP connection information by using the following steps:</span></span>

   <span data-ttu-id="1d7e2-159">a.</span><span class="sxs-lookup"><span data-stu-id="1d7e2-159">a.</span></span> <span data-ttu-id="1d7e2-160">按一下 [部署認證] 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-160">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="1d7e2-161">b.這是另一個 C# 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-161">b.</span></span> <span data-ttu-id="1d7e2-162">複製您完整的 FTP 使用者名稱和 URL，並加以儲存供本教學課程的下一節使用。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-162">Copy your full FTP username and URL and save them for the next section of this tutorial.</span></span>

   ![FTP URL 和認證][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a><span data-ttu-id="1d7e2-164">將您的 Spring Boot Web 應用程式部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="1d7e2-164">Deploy your Spring Boot web app to Azure</span></span>

<span data-ttu-id="1d7e2-165">下列步驟將引導您完成將 Spring Boot Web 應用程式部署到 Azure 的步驟。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-165">The following steps will walk you through the steps to deploy your Spring Boot web app to Azure.</span></span>

1. <span data-ttu-id="1d7e2-166">開啟 Windows 記事本之類的文字編輯器，並將下列文字貼入新文件，然後將檔案儲存為 *web.config*：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-166">Open a text editor such as Windows Notepad and paste the following text into a new document, then save the file as *web.config*:</span></span>
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

1. <span data-ttu-id="1d7e2-167">當您將 *web.config* 檔案儲存至系統之後，使用本教學課程上一節的 URL、使用者名稱和密碼透過 FTP 連線到您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-167">After you have saved the *web.config* file to your system, connect to your web app via FTP using the URL, username, and password from the preceding section of this tutorial.</span></span> <span data-ttu-id="1d7e2-168">例如：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-168">For example:</span></span>
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. <span data-ttu-id="1d7e2-169">將遠端目錄變更至您 Web 應用程式的根資料夾 (在 */site/wwwroot*中)，然後複製您 Spring Boot 應用程式中的 JAR 檔案和上述 *web.config*。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-169">Change the remote directory to the root folder of your web app, (which is at */site/wwwroot*), then copy the JAR file from your Spring Boot application and the *web.config* from earlier.</span></span> <span data-ttu-id="1d7e2-170">例如：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-170">For example:</span></span>
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. <span data-ttu-id="1d7e2-171">當您將 JAR 和 *web.config* 檔案部署到 Web 應用程式之後，您必須使用 Azure 入口網站重新啟動 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-171">After you have deployed your JAR and *web.config* files to your web app, you need to restart your web app using the Azure portal:</span></span>

   ![][AZ10]

1. <span data-ttu-id="1d7e2-172">使用網頁瀏覽器瀏覽至 Web 應用程式的 URL 來測試 Web 應用程式；如果有可用的 curl，請使用類似下列範例的語法：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-172">Test the web app by browsing to your web app's URL using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. <span data-ttu-id="1d7e2-173">您應該會看到顯示下列訊息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="1d7e2-173">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![瀏覽範例應用程式][SB02]

## <a name="next-steps"></a><span data-ttu-id="1d7e2-175">後續步驟</span><span class="sxs-lookup"><span data-stu-id="1d7e2-175">Next steps</span></span>

<span data-ttu-id="1d7e2-176">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="1d7e2-176">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="1d7e2-177">將 Spring Boot 應用程式部署到 Azure Container Service 中的 Linux</span><span class="sxs-lookup"><span data-stu-id="1d7e2-177">Deploy a Spring Boot Application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

* [<span data-ttu-id="1d7e2-178">將 Spring Boot 應用程式部署到 Azure Container Service 中的 Kubernetes 叢集</span><span class="sxs-lookup"><span data-stu-id="1d7e2-178">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="1d7e2-179">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-179">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="1d7e2-180">如需使用 FTP 將 Web 應用程式部署到 Azure 的其他資訊，請參閱[使用 FTP/S 將您的應用程式部署至 Azure App Service]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-180">For additional information about depoying web apps to Azure using FTP, see [Deploy your app to Azure App Service using FTP/S].</span></span>

<span data-ttu-id="1d7e2-181">如需 Spring Boot 範例專案的進一步詳細資訊，請參閱 [Spring Boot Getting Started]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-181">For further details about the Spring Boot sample project, see [Spring Boot Getting Started].</span></span>

<span data-ttu-id="1d7e2-182">如需開始使用您自己的 Spring Boot 應用程式的說明，請參閱 **Spring Initializr** (網址為 https://start.spring.io/)。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-182">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="1d7e2-183">如需如何設定 Web 應用程式之其他設定的詳細資訊，請參閱[在 Azure App Service 中設定 Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="1d7e2-183">For more information about configuring additional settings for your web app, see [Configure web apps in Azure App Service].</span></span>

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
