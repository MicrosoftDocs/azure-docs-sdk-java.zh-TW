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
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a><span data-ttu-id="f49d9-103">將 Spring Boot JAR 檔案 Web 應用程式部署至 Linux 上的 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f49d9-103">Deploy a Spring Boot JAR file web app to Azure App Service on Linux</span></span>

<span data-ttu-id="f49d9-104">本文示範如何使用[適用於 App Service Web Apps 的 Maven 外掛程式](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)，將 Spring Boot 應用程式封裝成 Java SE JAR，並部署至 [Linux 上的 Azure App Service](/azure/app-service/containers/)。</span><span class="sxs-lookup"><span data-stu-id="f49d9-104">This article demonstrates using the [Maven Plugin for Azure App Service Web Apps](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) to deploy a Spring Boot application packaged as a Java SE JAR to [Azure App Service on Linux](/azure/app-service/containers/).</span></span> <span data-ttu-id="f49d9-105">若要將應用程式相依性、執行階段及組態合併至單一可部署成品，請從 [Tomcat 與 WAR 檔案](/azure/app-service/containers/quickstart-java)中選擇 Java SE 部署。</span><span class="sxs-lookup"><span data-stu-id="f49d9-105">Choose Java SE deployment over [Tomcat and WAR files](/azure/app-service/containers/quickstart-java) when you want to consolidate your app's dependencies, runtime, and configuration into a single deployable artifact.</span></span>


<span data-ttu-id="f49d9-106">如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。</span><span class="sxs-lookup"><span data-stu-id="f49d9-106">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f49d9-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="f49d9-107">Prerequisites</span></span>

<span data-ttu-id="f49d9-108">若要完成本教學課程中的步驟，您必須已安裝以下項目並設定完成：</span><span class="sxs-lookup"><span data-stu-id="f49d9-108">To complete the steps in this tutorial, you'll need to have the following installed and configured:</span></span>

* <span data-ttu-id="f49d9-109">[Azure CLI](/cli/azure/)，安裝在本機或透過 [Azure Cloud Shell](https://shell.azure.com)安裝。</span><span class="sxs-lookup"><span data-stu-id="f49d9-109">The [Azure CLI](/cli/azure/), either locally or through [Azure Cloud Shell](https://shell.azure.com).</span></span>
* <span data-ttu-id="f49d9-110">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="f49d9-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="f49d9-111">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="f49d9-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="f49d9-112">Apache 的 [Maven](https://maven.apache.org/) 3.0 版。</span><span class="sxs-lookup"><span data-stu-id="f49d9-112">Apache's [Maven](https://maven.apache.org/), Version 3).</span></span>
* <span data-ttu-id="f49d9-113">[Git](https://git-scm.com/downloads) 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f49d9-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="install-and-sign-in-to-azure-cli"></a><span data-ttu-id="f49d9-114">安裝並登入 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f49d9-114">Install and sign in to Azure CLI</span></span>

<span data-ttu-id="f49d9-115">讓 Maven 外掛程式部署 Spring Boot 應用程式最簡單且最輕鬆的方式是使用 [Azure CLI](/cli/azure/)。</span><span class="sxs-lookup"><span data-stu-id="f49d9-115">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](/cli/azure/).</span></span>

<span data-ttu-id="f49d9-116">使用 Azure CLI 登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="f49d9-116">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
<span data-ttu-id="f49d9-117">依照指示完成登入程序。</span><span class="sxs-lookup"><span data-stu-id="f49d9-117">Follow the instructions to complete the sign-in process.</span></span>

## <a name="clone-the-sample-app"></a><span data-ttu-id="f49d9-118">複製範例應用程式</span><span class="sxs-lookup"><span data-stu-id="f49d9-118">Clone the sample app</span></span>

<span data-ttu-id="f49d9-119">在本節中，您會在本機複製已完成的 Spring Boot 應用程式，並且進行測試。</span><span class="sxs-lookup"><span data-stu-id="f49d9-119">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="f49d9-120">開啟命令提示字元或終端機視窗，並建立本機目錄來保存您的 Spring Boot 應用程式，然後變更至該目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="f49d9-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="f49d9-121">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="f49d9-121">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="f49d9-122">將 [Spring Boot Getting Started] 範例專案複製到您建立的目錄中；例如：</span><span class="sxs-lookup"><span data-stu-id="f49d9-122">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="f49d9-123">將目錄變更至已完成的專案；例如：</span><span class="sxs-lookup"><span data-stu-id="f49d9-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="f49d9-124">使用 Maven 建立 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="f49d9-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="f49d9-125">建立 Web 應用程式後，使用 Maven 啟動 Web 應用程式，例如：</span><span class="sxs-lookup"><span data-stu-id="f49d9-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="f49d9-126">測試 Web 應用程式，方法是使用網頁瀏覽器在本機瀏覽它。</span><span class="sxs-lookup"><span data-stu-id="f49d9-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="f49d9-127">例如，如果您有 curl 可用，可以使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="f49d9-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="f49d9-128">您應該會看到下列訊息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="f49d9-128">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="configure-maven-plugin-for-azure-app-service"></a><span data-ttu-id="f49d9-129">為 Azure App Service 設定 Maven 外掛程式</span><span class="sxs-lookup"><span data-stu-id="f49d9-129">Configure Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="f49d9-130">在本節中，您將設定 Spring Boot 專案`pom.xml`，以便 Maven 可以將應用程式部署至 Linux 上的 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="f49d9-130">In this section, you will configure the Spring Boot project `pom.xml` so that Maven can deploy the app to Azure App Service on Linux.</span></span>

1. <span data-ttu-id="f49d9-131">在程式碼編輯器中，開啟 `pom.xml`。</span><span class="sxs-lookup"><span data-stu-id="f49d9-131">Open `pom.xml` in a code editor.</span></span>

2. <span data-ttu-id="f49d9-132">在 pom.xml 的 `<build>` 區段中，於 `<plugins>` 標記內新增下列 `<plugin>` 項目。</span><span class="sxs-lookup"><span data-stu-id="f49d9-132">In the `<build>` section of the pom.xml, add the following `<plugin>` entry inside the `<plugins>` tag.</span></span>

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.6.0</version>
   </plugin>
   ```

3. <span data-ttu-id="f49d9-133">接著您可以設定部署、在命令提示字元中執行 Maven 命令 `mvn azure-webapp:config` 並使用**數字**在提示中選擇這些選項：</span><span class="sxs-lookup"><span data-stu-id="f49d9-133">Then you can configure the deployment, run the maven command `mvn azure-webapp:config` in the Command Prompt and use the **number** to choose these options in the prompt:</span></span>
    * <span data-ttu-id="f49d9-134">**OS**：Linux</span><span class="sxs-lookup"><span data-stu-id="f49d9-134">**OS**: linux</span></span>  
    * <span data-ttu-id="f49d9-135">**javaVersion**：jre8</span><span class="sxs-lookup"><span data-stu-id="f49d9-135">**javaVersion**: jre8</span></span>
    * <span data-ttu-id="f49d9-136">**runtimeStack**：jre8</span><span class="sxs-lookup"><span data-stu-id="f49d9-136">**runtimeStack**: jre8</span></span>

<span data-ttu-id="f49d9-137">看到 **Confirm (Y/N)** 提示時，按下 **'y'** 完成設定。</span><span class="sxs-lookup"><span data-stu-id="f49d9-137">When you get the **Confirm (Y/N)** prompt, press **'y'** and the configuration is done.</span></span>

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

4. <span data-ttu-id="f49d9-138">將 `<appSettings>` 區段新增到 `<azure-webapp-maven-plugin>` 的 `<configuration>` 區段，以接聽連接埠 *80*。</span><span class="sxs-lookup"><span data-stu-id="f49d9-138">Add the `<appSettings>` section to the `<configuration>` section of `<azure-webapp-maven-plugin>` to listen on the *80* port.</span></span>

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

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="f49d9-139">將應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="f49d9-139">Deploy the app to Azure</span></span>

<span data-ttu-id="f49d9-140">一旦您已設定本文上述章節中的所有設定，您已準備好將 Web 應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="f49d9-140">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="f49d9-141">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="f49d9-141">To do so, use the following steps:</span></span>

1. <span data-ttu-id="f49d9-142">如果您對 pom.xml  檔案進行任何變更，從您稍早使用的命令提示字元或終端機視窗，使用 Maven 重新建置 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="f49d9-142">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="f49d9-143">使用 Maven 將您的 Web 應用程式部署至 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="f49d9-143">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="f49d9-144">Maven 會將您的 Web 應用程式部署至 Azure；如果 Web 應用程式或 Web 應用程式方案不存在，系統會為您建立。</span><span class="sxs-lookup"><span data-stu-id="f49d9-144">Maven will deploy your web app to Azure; if the web app or web app plan does not already exist, it will be created for you.</span></span>

<span data-ttu-id="f49d9-145">網站部署完成時，您就可以使用 [Azure 入口網站]來管理。</span><span class="sxs-lookup"><span data-stu-id="f49d9-145">When your web has been deployed, you will be able to manage it through the [Azure portal].</span></span>

* <span data-ttu-id="f49d9-146">您的 Web 應用程式會列在**應用程式服務** 中：</span><span class="sxs-lookup"><span data-stu-id="f49d9-146">Your web app will be listed in **App Services**:</span></span>

   ![列在 Azure 入口網站應用程式服務中的 Web 應用程式][AP01]

* <span data-ttu-id="f49d9-148">Web 應用程式的 URL 會列在 Web 應用程式的 [概觀]  中：</span><span class="sxs-lookup"><span data-stu-id="f49d9-148">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![決定 Web 應用程式的 URL][AP02]

<span data-ttu-id="f49d9-150">使用與之前相同的 cURL 命令來確認部署是否成功；請使用入口網站中的 Web 應用程式 URL，不要使用 `localhost`。</span><span class="sxs-lookup"><span data-stu-id="f49d9-150">Verify that the deployment was successful by using the same cURL command as before, using your web app URL from the Portal instead of `localhost`.</span></span> <span data-ttu-id="f49d9-151">您應該會看到下列訊息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="f49d9-151">You should see the following message displayed: **Greetings from Spring Boot!**</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f49d9-152">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f49d9-152">Next steps</span></span>

<span data-ttu-id="f49d9-153">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="f49d9-153">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f49d9-154">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="f49d9-154">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="f49d9-155">其他資源</span><span class="sxs-lookup"><span data-stu-id="f49d9-155">Additional Resources</span></span>

<span data-ttu-id="f49d9-156">如需本文所討論之各種技術的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="f49d9-156">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="f49d9-157">[適用於 Azure Web 應用程式的 Maven 外掛程式]</span><span class="sxs-lookup"><span data-stu-id="f49d9-157">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="f49d9-158">如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將容器化 Spring Boot 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="f49d9-158">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="f49d9-159">使用 Azure CLI 2.0 來建立 Azure 服務主體</span><span class="sxs-lookup"><span data-stu-id="f49d9-159">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="f49d9-160">Maven 設定參考</span><span class="sxs-lookup"><span data-stu-id="f49d9-160">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /java/azure/
[Azure 入口網站]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[適用於 Azure Web 應用程式的 Maven 外掛程式]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
