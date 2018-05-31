---
title: 透過 Maven 和 Azure 將 Spring Boot 應用程式部署到雲端
description: 了解如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Spring Boot 應用程式部署至雲端。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm;kevinzha
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 82cb0da3ce49fa77f888808af14455bf226d5cb0
ms.sourcegitcommit: 024b3127daf396a17bd43d57642e3534ae87f120
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/23/2018
ms.locfileid: "34462748"
---
# <a name="deploy-a-spring-boot-app-to-the-cloud-using-the-maven-plugin-for-azure-web-apps"></a><span data-ttu-id="7995a-103">使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Spring Boot 應用程式部署至雲端</span><span class="sxs-lookup"><span data-stu-id="7995a-103">Deploy a Spring Boot app to the cloud using the Maven Plugin for Azure Web Apps</span></span>

<span data-ttu-id="7995a-104">本文示範如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將範例 Spring Boot 應用程式部署至 Azure App Services。</span><span class="sxs-lookup"><span data-stu-id="7995a-104">This article demonstrates using the Maven Plugin for Azure Web Apps to deploy a sample Spring Boot application to Azure App Services.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="7995a-105">針對 [Apache Maven](http://maven.apache.org/) 的[適用於 Azure Web 應用程式的 Maven 外掛程式](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin)提供 Azure App Service 到 Maven 專案的緊密整合，並且簡化開發人員將 Web 應用程式部署至 Azure App Service 的程序。</span><span class="sxs-lookup"><span data-stu-id="7995a-105">The [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>
> 
> <span data-ttu-id="7995a-106">適用於 Azure Web 應用程式的 Maven 外掛程式目前可供預覽。</span><span class="sxs-lookup"><span data-stu-id="7995a-106">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="7995a-107">雖然未來計劃有額外功能，但是現在僅支援 FTP 發佈。</span><span class="sxs-lookup"><span data-stu-id="7995a-107">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="7995a-108">先決條件</span><span class="sxs-lookup"><span data-stu-id="7995a-108">Prerequisites</span></span>

<span data-ttu-id="7995a-109">若要完成本教學課程中的步驟，您必須具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="7995a-109">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="7995a-110">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="7995a-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="7995a-111">[Azure 命令列介面 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="7995a-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="7995a-112">最新的 [Java 開發套件 (JDK)] 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="7995a-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="7995a-113">Apache 的 [Maven] 建置工具 (第 3 版)。</span><span class="sxs-lookup"><span data-stu-id="7995a-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="7995a-114">[Git] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="7995a-114">A [Git] client.</span></span>

## <a name="clone-the-sample-spring-boot-web-app"></a><span data-ttu-id="7995a-115">複製範例 Spring Boot Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7995a-115">Clone the sample Spring Boot web app</span></span>

<span data-ttu-id="7995a-116">在本節中，您會在本機複製已完成的 Spring Boot 應用程式，並且進行測試。</span><span class="sxs-lookup"><span data-stu-id="7995a-116">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="7995a-117">開啟命令提示字元或終端機視窗，並建立本機目錄來保存您的 Spring Boot 應用程式，然後變更至該目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="7995a-117">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="7995a-118">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="7995a-118">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="7995a-119">將 [Spring Boot Getting Started] 範例專案複製到您建立的目錄中；例如：</span><span class="sxs-lookup"><span data-stu-id="7995a-119">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. <span data-ttu-id="7995a-120">將目錄變更至已完成的專案；例如：</span><span class="sxs-lookup"><span data-stu-id="7995a-120">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="7995a-121">使用 Maven 建立 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="7995a-121">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="7995a-122">建立 Web 應用程式後，使用 Maven 啟動 Web 應用程式，例如：</span><span class="sxs-lookup"><span data-stu-id="7995a-122">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="7995a-123">測試 Web 應用程式，方法是使用網頁瀏覽器在本機瀏覽它。</span><span class="sxs-lookup"><span data-stu-id="7995a-123">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="7995a-124">例如，如果您有 curl 可用，可以使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="7995a-124">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="7995a-125">您應該會看到顯示下列訊息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="7995a-125">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="7995a-126">建立 Azure 服務主體</span><span class="sxs-lookup"><span data-stu-id="7995a-126">Create an Azure service principal</span></span>

<span data-ttu-id="7995a-127">在本節中，您會建立 Azure 服務主體，Maven 外掛程式會在將您的 Web 應用程式部署至 Azure 時使用該服務主體。</span><span class="sxs-lookup"><span data-stu-id="7995a-127">In this section, you will create an Azure service principal that the Maven plugin uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="7995a-128">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="7995a-128">Open a command prompt.</span></span>

1. <span data-ttu-id="7995a-129">使用 Azure CLI 登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="7995a-129">Sign into your Azure account by using the Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="7995a-130">依照指示完成登入程序。</span><span class="sxs-lookup"><span data-stu-id="7995a-130">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="7995a-131">建立 Azure 服務主體：</span><span class="sxs-lookup"><span data-stu-id="7995a-131">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="7995a-132">其中 `uuuuuuuu` 是使用者名稱，`pppppppp` 是服務主體的密碼。</span><span class="sxs-lookup"><span data-stu-id="7995a-132">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="7995a-133">Azure 使用 JSON 回應，類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="7995a-133">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="7995a-134">當您設定 Maven 外掛程式以將您的 Web 應用程式部署至 Azure 時，您會使用此 JSON 回應中的值。</span><span class="sxs-lookup"><span data-stu-id="7995a-134">You will use the values from this JSON response when you configure the Maven plugin to deploy your web app to Azure.</span></span> <span data-ttu-id="7995a-135">`aaaaaaaa`、`uuuuuuuu`、`pppppppp` 和 `tttttttt` 是預留位置值，在此範例中使用，在您於下一節設定 Maven `settings.xml` 檔案時，更方便將這些值對應至個別元素。</span><span class="sxs-lookup"><span data-stu-id="7995a-135">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a><span data-ttu-id="7995a-136">設定 Maven 以使用您的 Azure 服務主體</span><span class="sxs-lookup"><span data-stu-id="7995a-136">Configure Maven to use your Azure service principal</span></span>

<span data-ttu-id="7995a-137">在本節中，您使用 Azure 服務主體的值，設定將您的 Web 應用程式部署至 Azure 時，Maven 使用的驗證。</span><span class="sxs-lookup"><span data-stu-id="7995a-137">In this section, you will use the values from your Azure service principal to configure the authentication that Maven uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="7995a-138">在文字編輯器中開啟 Maven `settings.xml` 檔案。</span><span class="sxs-lookup"><span data-stu-id="7995a-138">Open your Maven `settings.xml` file in a text editor.</span></span> <span data-ttu-id="7995a-139">這個檔案可能在類似下列範例的路徑中：</span><span class="sxs-lookup"><span data-stu-id="7995a-139">This file may be in a path similar to the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="7995a-140">將本教學課程上一節的 Azure 服務主體設定新增至 settings.xml 檔案中的 `<servers>` 集合；例如：</span><span class="sxs-lookup"><span data-stu-id="7995a-140">Add your Azure service principal settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="7995a-141">其中：</span><span class="sxs-lookup"><span data-stu-id="7995a-141">Where:</span></span>
   | <span data-ttu-id="7995a-142">元素</span><span class="sxs-lookup"><span data-stu-id="7995a-142">Element</span></span> | <span data-ttu-id="7995a-143">說明</span><span class="sxs-lookup"><span data-stu-id="7995a-143">Description</span></span> |
   |---|---|
   | `<id>` | <span data-ttu-id="7995a-144">指定將您的 Web 應用程式部署至 Azure 時，Maven 用來查閱安全性設定的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="7995a-144">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span> |
   | `<client>` | <span data-ttu-id="7995a-145">包含服務主體的 `appId` 值。</span><span class="sxs-lookup"><span data-stu-id="7995a-145">Contains the `appId` value from your service principal.</span></span> |
   | `<tenant>` | <span data-ttu-id="7995a-146">包含服務主體的 `tenant` 值。</span><span class="sxs-lookup"><span data-stu-id="7995a-146">Contains the `tenant` value from your service principal.</span></span> |
   | `<key>` | <span data-ttu-id="7995a-147">包含服務主體的 `password` 值。</span><span class="sxs-lookup"><span data-stu-id="7995a-147">Contains the `password` value from your service principal.</span></span> |
   | `<environment>` | <span data-ttu-id="7995a-148">定義目標 Azure 雲端環境，也就是此範例中的 `AZURE`。</span><span class="sxs-lookup"><span data-stu-id="7995a-148">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="7995a-149">(環境的完整清單可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得)。</span><span class="sxs-lookup"><span data-stu-id="7995a-149">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |

1. <span data-ttu-id="7995a-150">儲存並關閉 settings.xml 檔案。</span><span class="sxs-lookup"><span data-stu-id="7995a-150">Save and close the *settings.xml* file.</span></span>

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-to-azure"></a><span data-ttu-id="7995a-151">選擇性：將您的 Web 應用程式部署至 Azure 之前自訂 pom.xml</span><span class="sxs-lookup"><span data-stu-id="7995a-151">OPTIONAL: Customize your pom.xml before deploying your web app to Azure</span></span>

<span data-ttu-id="7995a-152">在文字編輯器中開啟 Spring Boot 應用程式的 `pom.xml` 檔案，然後找出 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。</span><span class="sxs-lookup"><span data-stu-id="7995a-152">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="7995a-153">此元素外觀會類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="7995a-153">This element should resemble the following example:</span></span>

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

<span data-ttu-id="7995a-154">您可以為 Maven 外掛程式修改數個值，這些元素的詳細描述可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得。</span><span class="sxs-lookup"><span data-stu-id="7995a-154">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="7995a-155">也就是說，有數個值值得在這篇文章中反白顯示：</span><span class="sxs-lookup"><span data-stu-id="7995a-155">That being said, there are several values that are worth highlighting in this article:</span></span>

| <span data-ttu-id="7995a-156">元素</span><span class="sxs-lookup"><span data-stu-id="7995a-156">Element</span></span> | <span data-ttu-id="7995a-157">說明</span><span class="sxs-lookup"><span data-stu-id="7995a-157">Description</span></span> |
|---|---|
| `<version>` | <span data-ttu-id="7995a-158">指定[適用於 Azure Web 應用程式的 Maven 外掛程式]版本。</span><span class="sxs-lookup"><span data-stu-id="7995a-158">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="7995a-159">請檢查 [Maven 中央存放庫](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中所列的版本，確定您使用的是最新版本。</span><span class="sxs-lookup"><span data-stu-id="7995a-159">Verify the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span> |
| `<authentication>` | <span data-ttu-id="7995a-160">指定 Azure 的驗證資訊，在此範例中包含 `<serverId>` 元素，其中包含 `azure-auth`，Maven 使用該值來查閱 Maven settings.xml 檔案 (您在本文稍早章節中定義) 中的 Azure 服務主體值。</span><span class="sxs-lookup"><span data-stu-id="7995a-160">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span> |
| `<resourceGroup>` | <span data-ttu-id="7995a-161">指定目標資源群組，也就是此範例中的 `maven-plugin`。</span><span class="sxs-lookup"><span data-stu-id="7995a-161">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="7995a-162">如果該資源群組不存在，則系統會在部署期間建立它。</span><span class="sxs-lookup"><span data-stu-id="7995a-162">The resource group is created during deployment if it does not already exist.</span></span> |
| `<appName>` | <span data-ttu-id="7995a-163">指定 Web 應用程式的目標名稱。</span><span class="sxs-lookup"><span data-stu-id="7995a-163">Specifies the target name for your web app.</span></span> <span data-ttu-id="7995a-164">在此範例中，目標名稱是 `maven-web-app-${maven.build.timestamp}`，在此範例中會附加 `${maven.build.timestamp}` 尾碼以避免發生衝突。</span><span class="sxs-lookup"><span data-stu-id="7995a-164">In this example, the target name is `maven-web-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="7995a-165">(時間戳記是選擇性的；您可以為應用程式名稱指定任何唯一的字串。)</span><span class="sxs-lookup"><span data-stu-id="7995a-165">(The timestamp is optional; you can specify any unique string for the app name.)</span></span> |
| `<region>` | <span data-ttu-id="7995a-166">指定目標區域，在此範例中是 `westus`。</span><span class="sxs-lookup"><span data-stu-id="7995a-166">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="7995a-167">(完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。)</span><span class="sxs-lookup"><span data-stu-id="7995a-167">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<javaVersion>` | <span data-ttu-id="7995a-168">指定 Web 應用程式的 Java 執行階段版本。</span><span class="sxs-lookup"><span data-stu-id="7995a-168">Specifies the Java runtime version for your web app.</span></span> <span data-ttu-id="7995a-169">(完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。)</span><span class="sxs-lookup"><span data-stu-id="7995a-169">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<deploymentType>` | <span data-ttu-id="7995a-170">指定 Web 應用程式的部署類型。</span><span class="sxs-lookup"><span data-stu-id="7995a-170">Specifies deployment type for your web app.</span></span> <span data-ttu-id="7995a-171">雖然其他部署類型的支援正在開發中，現在只有 `ftp` 受到支援。</span><span class="sxs-lookup"><span data-stu-id="7995a-171">For now, only `ftp` is supported, although support for other deployment types is in development.</span></span> |
| `<resources>` | <span data-ttu-id="7995a-172">指定當 Maven 將 Web 應用程式部署至 Azure 時使用的資源和目標目的地。</span><span class="sxs-lookup"><span data-stu-id="7995a-172">Specifies resources and target destinations which Maven uses when deploying your web app to Azure.</span></span> <span data-ttu-id="7995a-173">在此範例中，兩個 `<resource>` 元素會指定 Maven 將會部署 Web 應用程式的 JAR 檔案和 Spring Boot 專案的 web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="7995a-173">In this example, two `<resource>` elements specify that Maven will deploy the JAR file for your web app and the *web.config* file from the Spring Boot project.</span></span> |

## <a name="build-and-deploy-your-web-app-to-azure"></a><span data-ttu-id="7995a-174">建置 Web 應用程式並將其部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="7995a-174">Build and deploy your web app to Azure</span></span>

<span data-ttu-id="7995a-175">一旦您已設定本文上述章節中的所有設定，您已準備好將 Web 應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="7995a-175">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="7995a-176">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7995a-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="7995a-177">如果您對 pom.xml 檔案進行任何變更，從您稍早使用的命令提示字元或終端機視窗，使用 Maven 重新建置 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="7995a-177">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="7995a-178">使用 Maven 將您的 Web 應用程式部署至 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="7995a-178">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="7995a-179">Maven 會將您的 Web 應用程式部署至 Azure；如果 Web 應用程式不存在，系統會加以建立。</span><span class="sxs-lookup"><span data-stu-id="7995a-179">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

<span data-ttu-id="7995a-180">已部署您的網站時，您就可以使用 [Azure 入口網站]來管理它。</span><span class="sxs-lookup"><span data-stu-id="7995a-180">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="7995a-181">您的 Web 應用程式會列在**應用程式服務** 中：</span><span class="sxs-lookup"><span data-stu-id="7995a-181">Your web app will be listed in **App Services**:</span></span>

   ![列在 Azure 入口網站應用程式服務中的 Web 應用程式][AP01]

* <span data-ttu-id="7995a-183">Web 應用程式的 URL 會列在 Web 應用程式的 [概觀] 中：</span><span class="sxs-lookup"><span data-stu-id="7995a-183">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7995a-185">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7995a-185">Next steps</span></span>

<span data-ttu-id="7995a-186">如需本文所討論之各種技術的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="7995a-186">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="7995a-187">[適用於 Azure Web 應用程式的 Maven 外掛程式]</span><span class="sxs-lookup"><span data-stu-id="7995a-187">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="7995a-188">從 Azure CLI 登入 Azure</span><span class="sxs-lookup"><span data-stu-id="7995a-188">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="7995a-189">如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將容器化 Spring Boot 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="7995a-189">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="7995a-190">使用 Azure CLI 2.0 來建立 Azure 服務主體</span><span class="sxs-lookup"><span data-stu-id="7995a-190">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="7995a-191">Maven 設定參考</span><span class="sxs-lookup"><span data-stu-id="7995a-191">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

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
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[適用於 Azure Web 應用程式的 Maven 外掛程式]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
