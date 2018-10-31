---
title: 透過 Maven 和 Azure 將 Spring Boot JAR 檔案應用程式部署到雲端
description: 了解如何使用適用於 Azure Web App for Linux 的 Maven 外掛程式，將 Spring Boot 應用程式部署至雲端。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.author: robmcm
ms.date: 10/18/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: dc3038fed6859203f36e0c4dc9a9b01e81a7c4c5
ms.sourcegitcommit: dae7511a9d93ca7f388d5b0e05dc098e22c2f2f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2018
ms.locfileid: "49962492"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a><span data-ttu-id="7f42e-103">將 Spring Boot JAR 檔案 Web 應用程式部署至 Linux 上的 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7f42e-103">Deploy a Spring Boot JAR file web app to Azure App Service on Linux</span></span>

<span data-ttu-id="7f42e-104">本文示範如何使用[適用於 App Service Web Apps 的 Maven 外掛程式](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)，將 Spring Boot 應用程式封裝成 Java SE JAR，並部署至 [Linux 上的 Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/containers/)。</span><span class="sxs-lookup"><span data-stu-id="7f42e-104">This article demonstrates using the [Maven Plugin for Azure App Service Web Apps](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) to deploy a Spring Boot application packaged as a Java SE JAR to [Azure App Service on Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/).</span></span> <span data-ttu-id="7f42e-105">若要將應用程式相依性、執行階段及組態合併至單一可部署成品，請從 [Tomcat 與 WAR 檔案](/azure/app-service/containers/quickstart-java)中選擇 Java SE 部署。</span><span class="sxs-lookup"><span data-stu-id="7f42e-105">Choose Java SE deployment over [Tomcat and WAR files](/azure/app-service/containers/quickstart-java) when you want to consolidate your app's dependencies, runtime, and configuration into a single deployable artifact.</span></span>


<span data-ttu-id="7f42e-106">如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。</span><span class="sxs-lookup"><span data-stu-id="7f42e-106">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f42e-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="7f42e-107">Prerequisites</span></span>

<span data-ttu-id="7f42e-108">若要完成本教學課程中的步驟，您必須已安裝以下項目並設定完成：</span><span class="sxs-lookup"><span data-stu-id="7f42e-108">To complete the steps in this tutorial, you'll need to have the following installed and configured:</span></span>

* <span data-ttu-id="7f42e-109">[Azure CLI](/cli/azure/)，安裝在本機或透過 [Azure Cloud Shell](https://shell.azure.com)安裝。</span><span class="sxs-lookup"><span data-stu-id="7f42e-109">The [Azure CLI](/cli/azure/), either locally or through [Azure Cloud Shell](https://shell.azure.com).</span></span>
* <span data-ttu-id="7f42e-110">[Java 開發套件 (JDK)](https://www.azul.com/downloads/azure-only/zulu/) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="7f42e-110">[Java Development Kit (JDK)](https://www.azul.com/downloads/azure-only/zulu/), version 1.7 or later.</span></span>
* <span data-ttu-id="7f42e-111">Apache 的 [Maven](https://maven.apache.org/) 3.0 版。</span><span class="sxs-lookup"><span data-stu-id="7f42e-111">Apache's [Maven](https://maven.apache.org/), Version 3).</span></span>
* <span data-ttu-id="7f42e-112">[Git](https://git-scm.com/downloads) 用戶端。</span><span class="sxs-lookup"><span data-stu-id="7f42e-112">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="install-and-sign-in-to-azure-cli"></a><span data-ttu-id="7f42e-113">安裝並登入 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7f42e-113">Install and sign in to Azure CLI</span></span>

<span data-ttu-id="7f42e-114">讓 Maven 外掛程式部署 Spring Boot 應用程式最簡單且最輕鬆的方式是使用 [Azure CLI](https://docs.microsoft.com/cli/azure/)。</span><span class="sxs-lookup"><span data-stu-id="7f42e-114">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](https://docs.microsoft.com/cli/azure/).</span></span>

<span data-ttu-id="7f42e-115">使用 Azure CLI 登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="7f42e-115">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
<span data-ttu-id="7f42e-116">依照指示完成登入程序。</span><span class="sxs-lookup"><span data-stu-id="7f42e-116">Follow the instructions to complete the sign-in process.</span></span>

## <a name="clone-the-sample-app"></a><span data-ttu-id="7f42e-117">複製範例應用程式</span><span class="sxs-lookup"><span data-stu-id="7f42e-117">Clone the sample app</span></span>

<span data-ttu-id="7f42e-118">在本節中，您會在本機複製已完成的 Spring Boot 應用程式，並且進行測試。</span><span class="sxs-lookup"><span data-stu-id="7f42e-118">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="7f42e-119">開啟命令提示字元或終端機視窗，並建立本機目錄來保存您的 Spring Boot 應用程式，然後變更至該目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="7f42e-119">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="7f42e-120">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="7f42e-120">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="7f42e-121">將 [Spring Boot Getting Started] 範例專案複製到您建立的目錄中；例如：</span><span class="sxs-lookup"><span data-stu-id="7f42e-121">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="7f42e-122">將目錄變更至已完成的專案；例如：</span><span class="sxs-lookup"><span data-stu-id="7f42e-122">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="7f42e-123">使用 Maven 建立 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="7f42e-123">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="7f42e-124">建立 Web 應用程式後，使用 Maven 啟動 Web 應用程式，例如：</span><span class="sxs-lookup"><span data-stu-id="7f42e-124">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="7f42e-125">測試 Web 應用程式，方法是使用網頁瀏覽器在本機瀏覽它。</span><span class="sxs-lookup"><span data-stu-id="7f42e-125">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="7f42e-126">例如，如果您有 curl 可用，可以使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="7f42e-126">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="7f42e-127">您應該會看到顯示下列訊息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="7f42e-127">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="configure-maven-plugin-for-azure-app-service"></a><span data-ttu-id="7f42e-128">為 Azure App Service 設定 Maven 外掛程式</span><span class="sxs-lookup"><span data-stu-id="7f42e-128">Configure Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="7f42e-129">在本節中，您將設定 Spring Boot 專案`pom.xml`，以便 Maven 可以將應用程式部署至 Linux 上的 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="7f42e-129">In this section, you will configure the Spring Boot project `pom.xml` so that Maven can deploy the app to Azure App Service on Linux.</span></span>

1. <span data-ttu-id="7f42e-130">在程式碼編輯器中，開啟 `pom.xml`。</span><span class="sxs-lookup"><span data-stu-id="7f42e-130">Open `pom.xml` in a code editor.</span></span>

2. <span data-ttu-id="7f42e-131">在 pom.xml 的 `<build>` 區段中，於 `<plugins>` 標記內新增下列 `<plugin>` 項目。</span><span class="sxs-lookup"><span data-stu-id="7f42e-131">In the `<build>` section of the pom.xml, add the following `<plugin>` entry inside the `<plugins>` tag.</span></span>

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.4.0</version>
    <configuration>
      <deploymentType>jar</deploymentType>

      <!-- configure app to run on port 80, required by App Service -->
      <appSettings>
        <property> 
          <name>JAVA_OPTS</name> 
          <value>-Dserver.port=80</value> 
        </property> 
      </appSettings>

      <!-- Web App information -->
      <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
      <appName>${WEBAPP_NAME}</appName>
      <region>${REGION}</region>  

      <!-- Java Runtime Stack for Web App on Linux-->
      <linuxRuntime>jre8</linuxRuntime>
    </configuration>
   </plugin>
   ```

3. <span data-ttu-id="7f42e-132">更新外掛程式組態中的下列預留位置：</span><span class="sxs-lookup"><span data-stu-id="7f42e-132">Update the following placeholders in the plugin configuration:</span></span>

| <span data-ttu-id="7f42e-133">Placeholder</span><span class="sxs-lookup"><span data-stu-id="7f42e-133">Placeholder</span></span> | <span data-ttu-id="7f42e-134">說明</span><span class="sxs-lookup"><span data-stu-id="7f42e-134">Description</span></span> |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | <span data-ttu-id="7f42e-135">要在其中建立 Web 應用程式的新資源群組名稱。</span><span class="sxs-lookup"><span data-stu-id="7f42e-135">Name for the new resource group in which to create your web app.</span></span> <span data-ttu-id="7f42e-136">將應用程式的所有資源放在群組中，藉此同時管理。</span><span class="sxs-lookup"><span data-stu-id="7f42e-136">By putting all the resources for an app in a group, you can manage them together.</span></span> <span data-ttu-id="7f42e-137">例如，刪除資源群組會刪除所有與應用程式相關聯的資源。</span><span class="sxs-lookup"><span data-stu-id="7f42e-137">For example, deleting the resource group would delete all resources associated with the app.</span></span> <span data-ttu-id="7f42e-138">使用唯一的新資源群組名稱來更新此值，例如 TestResources。</span><span class="sxs-lookup"><span data-stu-id="7f42e-138">Update this value with a unique new resource group name, for example, *TestResources*.</span></span> <span data-ttu-id="7f42e-139">您在下一節中會使用此資源群組名稱來清除所有的 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="7f42e-139">You will use this resource group name to clean up all Azure resources in a later section.</span></span> |
| `WEBAPP_NAME` | <span data-ttu-id="7f42e-140">應用程式名稱會成為 Web 應用程式在部署至 Azure 時的部分主機名稱 (WEBAPP_NAME.azurewebsites.net)。</span><span class="sxs-lookup"><span data-stu-id="7f42e-140">The app name will be part the host name for the web app when deployed to Azure (WEBAPP_NAME.azurewebsites.net).</span></span> <span data-ttu-id="7f42e-141">將此值更新為新 Azure Web 應用程式的唯一名稱 (例如 contoso)，它將會主控您的 Java 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f42e-141">Update this value with a unique name for the new Azure web app, which will host your Java app, for example *contoso*.</span></span> |
| `REGION` | <span data-ttu-id="7f42e-142">託管 Web 應用程式的 Azure 區域，例如 `westus2`。</span><span class="sxs-lookup"><span data-stu-id="7f42e-142">An Azure region where the web app is hosted, for example `westus2`.</span></span> <span data-ttu-id="7f42e-143">您可以使用 `az account list-locations` 命令，從 Cloud Shell 或 CLI 取得區域清單。</span><span class="sxs-lookup"><span data-stu-id="7f42e-143">You can get a list of regions from the Cloud Shell or CLI using the `az account list-locations` command.</span></span> |

<span data-ttu-id="7f42e-144">您可以在 [GitHub 上的 Maven 外掛程式參考](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)中找到組態選項的完整清單。</span><span class="sxs-lookup"><span data-stu-id="7f42e-144">A full list of configuration options can be found in the [Maven plugin reference on GitHub](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin).</span></span>

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="7f42e-145">將應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="7f42e-145">Deploy the app to Azure</span></span>

<span data-ttu-id="7f42e-146">一旦您已設定本文上述章節中的所有設定，您已準備好將 Web 應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="7f42e-146">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="7f42e-147">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7f42e-147">To do so, use the following steps:</span></span>

1. <span data-ttu-id="7f42e-148">如果您對 pom.xml 檔案進行任何變更，從您稍早使用的命令提示字元或終端機視窗，使用 Maven 重新建置 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="7f42e-148">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="7f42e-149">使用 Maven 將您的 Web 應用程式部署至 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="7f42e-149">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="7f42e-150">Maven 會將您的 Web 應用程式部署至 Azure；如果 Web 應用程式或 Web 應用程式方案不存在，系統會為您建立。</span><span class="sxs-lookup"><span data-stu-id="7f42e-150">Maven will deploy your web app to Azure; if the web app or web app plan does not already exist, it will be created for you.</span></span>

<span data-ttu-id="7f42e-151">網站部署完成時，您就可以使用 [Azure 入口網站]來管理。</span><span class="sxs-lookup"><span data-stu-id="7f42e-151">When your web has been deployed, you will be able to manage it through the [Azure portal].</span></span>

* <span data-ttu-id="7f42e-152">您的 Web 應用程式會列在**應用程式服務** 中：</span><span class="sxs-lookup"><span data-stu-id="7f42e-152">Your web app will be listed in **App Services**:</span></span>

   ![列在 Azure 入口網站應用程式服務中的 Web 應用程式][AP01]

* <span data-ttu-id="7f42e-154">Web 應用程式的 URL 會列在 Web 應用程式的 [概觀] 中：</span><span class="sxs-lookup"><span data-stu-id="7f42e-154">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![決定 Web 應用程式的 URL][AP02]

<span data-ttu-id="7f42e-156">使用與之前相同的 cURL 命令來確認部署是否成功；請使用入口網站中的 Web 應用程式 URL，不要使用 `localhost`。</span><span class="sxs-lookup"><span data-stu-id="7f42e-156">Verify that the deployment was successful by using the same cURL command as before, using your web app URL from the Portal instead of `localhost`.</span></span> <span data-ttu-id="7f42e-157">您應該會看到顯示下列訊息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="7f42e-157">You should see the following message displayed: **Greetings from Spring Boot!**</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7f42e-158">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7f42e-158">Next steps</span></span>

<span data-ttu-id="7f42e-159">如需本文所討論之各種技術的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="7f42e-159">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="7f42e-160">[適用於 Azure Web 應用程式的 Maven 外掛程式]</span><span class="sxs-lookup"><span data-stu-id="7f42e-160">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="7f42e-161">如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將容器化 Spring Boot 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="7f42e-161">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="7f42e-162">使用 Azure CLI 2.0 來建立 Azure 服務主體</span><span class="sxs-lookup"><span data-stu-id="7f42e-162">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="7f42e-163">Maven 設定參考</span><span class="sxs-lookup"><span data-stu-id="7f42e-163">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure 入口網站]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
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
