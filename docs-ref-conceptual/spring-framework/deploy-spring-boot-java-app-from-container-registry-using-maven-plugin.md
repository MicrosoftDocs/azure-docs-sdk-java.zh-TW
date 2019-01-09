---
title: 如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Azure Container Registry 中的 Spring Boot 應用程式部署至 Azure App Service
description: 本教學課程將逐步引導您藉由使用 Maven 外掛程式，將 Azure Container Registry 中的 Spring Boot 應用程式部署到 Azure App Service。
services: container-registry
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: abe73f46e3b5a3b85a9f0272c12539d230c1a879
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991372"
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a><span data-ttu-id="cf452-103">如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將 Azure Container Registry 中的 Spring Boot 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cf452-103">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app in Azure Container Registry to Azure App Service</span></span>

<span data-ttu-id="cf452-104">本文示範如何將範例 [Spring Boot] 應用程式部署至 Azure Container Registry，然後使用適用於 Azure Web 應用程式的 Maven 外掛程式，將應用程式部署至 Azure App Services。</span><span class="sxs-lookup"><span data-stu-id="cf452-104">This article demonstrates how to deploy a sample [Spring Boot] application to Azure Container Registry, and then use the Maven Plugin for Azure Web Apps to deploy your application to Azure App Service.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="cf452-105">針對 [Apache Maven](http://maven.apache.org/) 的適用於 Azure Web 應用程式的 Maven 外掛程式提供 Azure App Service 到 Maven 專案的緊密整合，並且簡化開發人員將 Web 應用程式部署至 Azure App Service 的程序。</span><span class="sxs-lookup"><span data-stu-id="cf452-105">The Maven Plugin for Azure Web Apps for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>
> 
> <span data-ttu-id="cf452-106">適用於 Azure Web 應用程式的 Maven 外掛程式目前可供預覽。</span><span class="sxs-lookup"><span data-stu-id="cf452-106">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="cf452-107">雖然未來計劃有額外功能，但是現在僅支援 FTP 發佈。</span><span class="sxs-lookup"><span data-stu-id="cf452-107">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="cf452-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="cf452-108">Prerequisites</span></span>

<span data-ttu-id="cf452-109">若要完成本教學課程中的步驟，您必須具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="cf452-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="cf452-110">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="cf452-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="cf452-111">[Azure 命令列介面 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="cf452-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="cf452-112">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="cf452-112">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="cf452-113">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="cf452-113">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="cf452-114">Apache 的 [Maven] 建置工具 (第 3 版)。</span><span class="sxs-lookup"><span data-stu-id="cf452-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="cf452-115">[Git] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="cf452-115">A [Git] client.</span></span>
* <span data-ttu-id="cf452-116">[Docker] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="cf452-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="cf452-117">由於本教學課程的虛擬化需求，您無法遵循本文中關於虛擬機器的步驟；您必須在啟用虛擬化功能的情況下使用實體電腦。</span><span class="sxs-lookup"><span data-stu-id="cf452-117">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="cf452-118">在 Docker Web 應用程式上複製範例 Spring Boot</span><span class="sxs-lookup"><span data-stu-id="cf452-118">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="cf452-119">在本節中，您會在本機複製容器化 Spring Boot 應用程式，並且進行測試。</span><span class="sxs-lookup"><span data-stu-id="cf452-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="cf452-120">開啟命令提示字元或終端機視窗，並建立本機目錄來保存您的 Spring Boot 應用程式，然後變更至該目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="cf452-121">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="cf452-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="cf452-122">將 [Spring Boot on Docker Getting Started] 範例專案複製到您所建立的目錄中；例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-122">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone -b https://github.com/spring-guides/gs-spring-boot-docker
   ```

1. <span data-ttu-id="cf452-123">將目錄變更至已完成的專案；例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="cf452-124">使用 Maven 建立 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="cf452-125">建立 Web 應用程式後，使用 Maven 啟動 Web 應用程式，例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="cf452-126">測試 Web 應用程式，方法是使用網頁瀏覽器在本機瀏覽它。</span><span class="sxs-lookup"><span data-stu-id="cf452-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="cf452-127">例如，如果您有 curl 可用，可以使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="cf452-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="cf452-128">您應該會看到下列訊息：**Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="cf452-128">You should see the following message displayed: **Hello Docker World**</span></span>

   ![在本機瀏覽範例應用程式][SB01]

> [!NOTE]
>
> <span data-ttu-id="cf452-130">當您在本機使用 Docker 時，可能會看到錯誤，指出您無法透過連接埠 2375 連線到 localhost。</span><span class="sxs-lookup"><span data-stu-id="cf452-130">When you are using Docker locally, you may see an error which states that you cannot connect to localhost on port 2375.</span></span> <span data-ttu-id="cf452-131">如果發生這種情況，建議您在本機使用 Docker，無需使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="cf452-131">If this happens, you may need to enable using Docker locally without TLS.</span></span> <span data-ttu-id="cf452-132">若要這樣做，開啟您的 Docker 設定，然後勾選 [不使用 TLS 在 TCP://localhost:2375 上公開精靈] 選項。</span><span class="sxs-lookup"><span data-stu-id="cf452-132">To do so, open your Docker settings and check the option to **Expose daemon on TCP://localhost:2375 without TLS**.</span></span>
>
> ![在本機的 TCP 連接埠 2375 上公開 Docker 精靈][TL01]

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="cf452-134">建立 Azure 服務主體</span><span class="sxs-lookup"><span data-stu-id="cf452-134">Create an Azure service principal</span></span>

<span data-ttu-id="cf452-135">在本節中，您建立 Azure 服務主體，Maven 外掛程式會在將您的容器部署至 Azure 時使用該服務主體。</span><span class="sxs-lookup"><span data-stu-id="cf452-135">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="cf452-136">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="cf452-136">Open a command prompt.</span></span>

2. <span data-ttu-id="cf452-137">使用 Azure CLI 登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="cf452-137">Sign into your Azure account by using the Azure CLI:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="cf452-138">依照指示完成登入程序。</span><span class="sxs-lookup"><span data-stu-id="cf452-138">Follow the instructions to complete the sign-in process.</span></span>

3. <span data-ttu-id="cf452-139">建立 Azure 服務主體：</span><span class="sxs-lookup"><span data-stu-id="cf452-139">Create an Azure service principal:</span></span>
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="cf452-140">其中：</span><span class="sxs-lookup"><span data-stu-id="cf452-140">Where:</span></span>

   | <span data-ttu-id="cf452-141">參數</span><span class="sxs-lookup"><span data-stu-id="cf452-141">Parameter</span></span>  |                    <span data-ttu-id="cf452-142">說明</span><span class="sxs-lookup"><span data-stu-id="cf452-142">Description</span></span>                     |
   |------------|----------------------------------------------------|
   | `uuuuuuuu` | <span data-ttu-id="cf452-143">指定服務主體的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="cf452-143">Specifies the user name for the service principal.</span></span> |
   | `pppppppp` | <span data-ttu-id="cf452-144">指定服務主體的密碼。</span><span class="sxs-lookup"><span data-stu-id="cf452-144">Specifies the password for the service principal.</span></span>  |


4. <span data-ttu-id="cf452-145">Azure 使用 JSON 回應，類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="cf452-145">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="cf452-146">當您設定 Maven 外掛程式以將您的容器部署至 Azure 時，您會使用此 JSON 回應中的值。</span><span class="sxs-lookup"><span data-stu-id="cf452-146">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="cf452-147">`aaaaaaaa`、`uuuuuuuu`、`pppppppp` 和 `tttttttt` 是預留位置值，在此範例中使用，在您於下一節設定 Maven `settings.xml` 檔案時，更方便將這些值對應至個別元素。</span><span class="sxs-lookup"><span data-stu-id="cf452-147">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="cf452-148">使用 Azure CLI 建立 Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="cf452-148">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="cf452-149">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="cf452-149">Open a command prompt.</span></span>

1. <span data-ttu-id="cf452-150">登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="cf452-150">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="cf452-151">為您將在這篇文章中使用的 Azure 資源建立資源群組：</span><span class="sxs-lookup"><span data-stu-id="cf452-151">Create a resource group for the Azure resources you will use in this article:</span></span>
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   <span data-ttu-id="cf452-152">將此範例中的 `wingtiptoysresources` 取代為資源群組的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="cf452-152">Replace `wingtiptoysresources` in this example with a unique name for your resource group.</span></span>

1. <span data-ttu-id="cf452-153">在 Spring Boot 應用程式的資源群組中建立私用 Azure Container Registry：</span><span class="sxs-lookup"><span data-stu-id="cf452-153">Create a private Azure container registry in the resource group for your Spring Boot app:</span></span> 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="cf452-154">將此範例中的 `wingtiptoysregistry` 取代為容器登錄的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="cf452-154">Replace `wingtiptoysregistry` in this example with a unique name for your container registry.</span></span>

1. <span data-ttu-id="cf452-155">擷取容器登錄的密碼：</span><span class="sxs-lookup"><span data-stu-id="cf452-155">Retrieve the password for your container registry:</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   <span data-ttu-id="cf452-156">Azure 會回應您的密碼，例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-156">Azure will respond with your password; for example:</span></span>
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a><span data-ttu-id="cf452-157">將您的 Azure Container Registry 和 Azure 服務主體新增至您的 Maven 設定</span><span class="sxs-lookup"><span data-stu-id="cf452-157">Add your Azure container registry and Azure service principal to your Maven settings</span></span>

1. <span data-ttu-id="cf452-158">在文字編輯器中開啟您的 Maven`settings.xml` 檔案，這個檔案可能在如下列範例的路徑中：</span><span class="sxs-lookup"><span data-stu-id="cf452-158">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

2. <span data-ttu-id="cf452-159">將這篇文章上一節的 Azure Container Registry 存取設定新增至 settings.xml 檔案中的 `<servers>` 集合；例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-159">Add your Azure Container Registry access settings from the previous section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   <span data-ttu-id="cf452-160">其中：</span><span class="sxs-lookup"><span data-stu-id="cf452-160">Where:</span></span>

   |   <span data-ttu-id="cf452-161">元素</span><span class="sxs-lookup"><span data-stu-id="cf452-161">Element</span></span>    |                                 <span data-ttu-id="cf452-162">說明</span><span class="sxs-lookup"><span data-stu-id="cf452-162">Description</span></span>                                  |
   |--------------|------------------------------------------------------------------------------|
   |    `<id>`    |         <span data-ttu-id="cf452-163">包含私用 Azure Container Registry 名稱。</span><span class="sxs-lookup"><span data-stu-id="cf452-163">Contains the name of your private Azure container registry.</span></span>          |
   | `<username>` |         <span data-ttu-id="cf452-164">包含私用 Azure Container Registry 名稱。</span><span class="sxs-lookup"><span data-stu-id="cf452-164">Contains the name of your private Azure container registry.</span></span>          |
   | `<password>` | <span data-ttu-id="cf452-165">包含您在這篇文章上一節擷取的密碼。</span><span class="sxs-lookup"><span data-stu-id="cf452-165">Contains the password you retrieved in the previous section of this article.</span></span> |


3. <span data-ttu-id="cf452-166">將這篇文章稍早章節的 Azure 服務主體設定新增至 settings.xml 檔案中的 `<servers>` 集合；例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-166">Add your Azure service principal settings from an earlier section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="cf452-167">其中：</span><span class="sxs-lookup"><span data-stu-id="cf452-167">Where:</span></span>

   |     <span data-ttu-id="cf452-168">元素</span><span class="sxs-lookup"><span data-stu-id="cf452-168">Element</span></span>     |                                                                                   <span data-ttu-id="cf452-169">說明</span><span class="sxs-lookup"><span data-stu-id="cf452-169">Description</span></span>                                                                                   |
   |-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `<id>`      |                                <span data-ttu-id="cf452-170">指定將您的 Web 應用程式部署至 Azure 時，Maven 用來查閱安全性設定的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="cf452-170">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>                                |
   |   `<client>`    |                                                             <span data-ttu-id="cf452-171">包含服務主體的 `appId` 值。</span><span class="sxs-lookup"><span data-stu-id="cf452-171">Contains the `appId` value from your service principal.</span></span>                                                             |
   |   `<tenant>`    |                                                            <span data-ttu-id="cf452-172">包含服務主體的 `tenant` 值。</span><span class="sxs-lookup"><span data-stu-id="cf452-172">Contains the `tenant` value from your service principal.</span></span>                                                             |
   |     `<key>`     |                                                           <span data-ttu-id="cf452-173">包含服務主體的 `password` 值。</span><span class="sxs-lookup"><span data-stu-id="cf452-173">Contains the `password` value from your service principal.</span></span>                                                            |
   | `<environment>` | <span data-ttu-id="cf452-174">定義目標 Azure 雲端環境，也就是此範例中的 `AZURE`。</span><span class="sxs-lookup"><span data-stu-id="cf452-174">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="cf452-175">(環境的完整清單可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得)</span><span class="sxs-lookup"><span data-stu-id="cf452-175">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span> |


4. <span data-ttu-id="cf452-176">儲存並關閉 settings.xml 檔案。</span><span class="sxs-lookup"><span data-stu-id="cf452-176">Save and close the *settings.xml* file.</span></span>

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a><span data-ttu-id="cf452-177">建置您的 Docker 容器映像，並將它推送到您的 Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="cf452-177">Build your Docker container image and push it to your Azure container registry</span></span>

1. <span data-ttu-id="cf452-178">瀏覽至 Spring Boot 應用程式的已完成專案目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 pom.xml 檔案。</span><span class="sxs-lookup"><span data-stu-id="cf452-178">Navigate to the completed project directory for your Spring Boot application, (e.g. "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

2. <span data-ttu-id="cf452-179">使用本教學課程上一節的 Azure Container Registry 登入伺服器值來更新 pom.xml 檔案中的 `<properties>` 集合；例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-179">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   <span data-ttu-id="cf452-180">其中：</span><span class="sxs-lookup"><span data-stu-id="cf452-180">Where:</span></span>

   |           <span data-ttu-id="cf452-181">元素</span><span class="sxs-lookup"><span data-stu-id="cf452-181">Element</span></span>           |                                                                       <span data-ttu-id="cf452-182">說明</span><span class="sxs-lookup"><span data-stu-id="cf452-182">Description</span></span>                                                                       |
   |-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `<azure.containerRegistry>` |                                              <span data-ttu-id="cf452-183">指定私用 Azure Container Registry 名稱。</span><span class="sxs-lookup"><span data-stu-id="cf452-183">Specifies the name of your private Azure container registry.</span></span>                                               |
   |   `<docker.image.prefix>`   | <span data-ttu-id="cf452-184">指定私用 Azure Container Registry 的 URL，它是藉由將 ".azurecr.io" 附加至私用容器登錄的名稱來衍生。</span><span class="sxs-lookup"><span data-stu-id="cf452-184">Specifies the URL of your private Azure container registry, which is derived by appending ".azurecr.io" to the name of your private container registry.</span></span> |


3. <span data-ttu-id="cf452-185">確認 pom.xml 檔案中 Docker 外掛程式的 `<plugin>`，包含本教學課程上一個步驟中伺服器位址和登錄名稱的正確屬性。</span><span class="sxs-lookup"><span data-stu-id="cf452-185">Verify that `<plugin>` for the Docker plugin in your *pom.xml* file contains the correct properties for the login server address and registry name from the previous step in this tutorial.</span></span> <span data-ttu-id="cf452-186">例如︰</span><span class="sxs-lookup"><span data-stu-id="cf452-186">For example:</span></span>

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
   <span data-ttu-id="cf452-187">其中：</span><span class="sxs-lookup"><span data-stu-id="cf452-187">Where:</span></span>

   |     <span data-ttu-id="cf452-188">元素</span><span class="sxs-lookup"><span data-stu-id="cf452-188">Element</span></span>     |                                       <span data-ttu-id="cf452-189">說明</span><span class="sxs-lookup"><span data-stu-id="cf452-189">Description</span></span>                                       |
   |-----------------|-----------------------------------------------------------------------------------------|
   |  `<serverId>`   |  <span data-ttu-id="cf452-190">指定屬性，該屬性包含私用 Azure Container Registry 的名稱。</span><span class="sxs-lookup"><span data-stu-id="cf452-190">Specifies the property which contains name of your private Azure container registry.</span></span>   |
   | `<registryUrl>` | <span data-ttu-id="cf452-191">指定屬性，該屬性包含私用 Azure Container Registry 的 URL。</span><span class="sxs-lookup"><span data-stu-id="cf452-191">Specifies the property which contains the URL of your private Azure container registry.</span></span> |


4. <span data-ttu-id="cf452-192">瀏覽至您 Spring Boot 應用程式的已完成專案目錄，然後執行下列命令來重建應用程式，並將容器推送到您的 Azure Container Registry：</span><span class="sxs-lookup"><span data-stu-id="cf452-192">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure container registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

5. <span data-ttu-id="cf452-193">選擇性：瀏覽至 [Azure 入口網站]，並確認在容器登錄中有名為 **gs-spring-boot-docker** 的 Docker 容器映像。</span><span class="sxs-lookup"><span data-stu-id="cf452-193">OPTIONAL: Browse to the [Azure portal] and verify that there is Docker container image named **gs-spring-boot-docker** in your container registry.</span></span>

   ![確認在 Azure 入口網站中的容器][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a><span data-ttu-id="cf452-195">自訂 pom.xml，然後建立容器並將其部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="cf452-195">Customize your pom.xml, then build and deploy your container to Azure</span></span>

<span data-ttu-id="cf452-196">在文字編輯器中開啟 Spring Boot 應用程式的 `pom.xml` 檔案，然後找出 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。</span><span class="sxs-lookup"><span data-stu-id="cf452-196">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="cf452-197">此元素外觀會類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="cf452-197">This element should resemble the following example:</span></span>

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

<span data-ttu-id="cf452-198">您可以為 Maven 外掛程式修改數個值，這些元素的詳細描述可於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件中取得。</span><span class="sxs-lookup"><span data-stu-id="cf452-198">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="cf452-199">也就是說，有數個值值得在這篇文章中反白顯示：</span><span class="sxs-lookup"><span data-stu-id="cf452-199">That being said, there are several values that are worth highlighting in this article:</span></span>

| <span data-ttu-id="cf452-200">元素</span><span class="sxs-lookup"><span data-stu-id="cf452-200">Element</span></span> | <span data-ttu-id="cf452-201">說明</span><span class="sxs-lookup"><span data-stu-id="cf452-201">Description</span></span> |
|---|---|
| `<version>` | <span data-ttu-id="cf452-202">指定[適用於 Azure Web 應用程式的 Maven 外掛程式]版本。</span><span class="sxs-lookup"><span data-stu-id="cf452-202">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="cf452-203">您應該檢查 [Maven 中央存放庫](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中所列的版本，確定您使用最新版本。</span><span class="sxs-lookup"><span data-stu-id="cf452-203">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span> |
| `<authentication>` | <span data-ttu-id="cf452-204">指定 Azure 的驗證資訊，在此範例中包含 `<serverId>` 元素，其中包含 `azure-auth`，Maven 使用該值來查閱 Maven settings.xml 檔案 (您在本文稍早章節中定義) 中的 Azure 服務主體值。</span><span class="sxs-lookup"><span data-stu-id="cf452-204">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span> |
| `<resourceGroup>` | <span data-ttu-id="cf452-205">指定目標資源群組，也就是此範例中的 `wingtiptoysresources`。</span><span class="sxs-lookup"><span data-stu-id="cf452-205">Specifies the target resource group, which is `wingtiptoysresources` in this example.</span></span> <span data-ttu-id="cf452-206">如果該資源群組不存在，則系統會在部署期間建立它。</span><span class="sxs-lookup"><span data-stu-id="cf452-206">The resource group will be created during deployment if it does not already exist.</span></span> |
| `<appName>` | <span data-ttu-id="cf452-207">指定 Web 應用程式的目標名稱。</span><span class="sxs-lookup"><span data-stu-id="cf452-207">Specifies the target name for your web app.</span></span> <span data-ttu-id="cf452-208">在此範例中，目標名稱是 `maven-linux-app-${maven.build.timestamp}`，在此範例中會附加 `${maven.build.timestamp}` 尾碼以避免發生衝突。</span><span class="sxs-lookup"><span data-stu-id="cf452-208">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="cf452-209">(時間戳記是選擇性的；您可以為應用程式名稱指定任何唯一的字串。)</span><span class="sxs-lookup"><span data-stu-id="cf452-209">(The timestamp is optional; you can specify any unique string for the app name.)</span></span> |
| `<region>` | <span data-ttu-id="cf452-210">指定目標區域，在此範例中是 `westus`。</span><span class="sxs-lookup"><span data-stu-id="cf452-210">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="cf452-211">(完整清單位於[適用於 Azure Web 應用程式的 Maven 外掛程式]文件。)</span><span class="sxs-lookup"><span data-stu-id="cf452-211">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<containerSettings>` | <span data-ttu-id="cf452-212">指定屬性，該屬性包含容器的名稱和 URL。</span><span class="sxs-lookup"><span data-stu-id="cf452-212">Specifies the properties which contain the name and URL of your container.</span></span> |
| `<appSettings>` | <span data-ttu-id="cf452-213">指定將您的 Web 應用程式部署至 Azure 時，Maven 使用的任何唯一設定。</span><span class="sxs-lookup"><span data-stu-id="cf452-213">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="cf452-214">在此範例中，`<property>` 元素包含子元素的名稱/值組，指定應用程式的連接埠。</span><span class="sxs-lookup"><span data-stu-id="cf452-214">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span> |

> [!NOTE]
>
> <span data-ttu-id="cf452-215">在此範例中變更連接埠號碼的設定，只有在您變更預設連接埠時才需要。</span><span class="sxs-lookup"><span data-stu-id="cf452-215">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

1. <span data-ttu-id="cf452-216">如果您對 pom.xml 檔案進行任何變更，從您稍早使用的命令提示字元或終端機視窗，使用 Maven 重新建置 JAR 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-216">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="cf452-217">使用 Maven 將您的 Web 應用程式部署至 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="cf452-217">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="cf452-218">Maven 會將您的 Web 應用程式部署至 Azure；如果 Web 應用程式不存在，系統會加以建立。</span><span class="sxs-lookup"><span data-stu-id="cf452-218">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="cf452-219">如果您在 pom.xml 檔案的 `<region>` 元素中指定的區域，在您啟動部署時沒有足夠的可用伺服器，您可能會看到類似下列範例的錯誤：</span><span class="sxs-lookup"><span data-stu-id="cf452-219">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
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
> <span data-ttu-id="cf452-220">如果發生這種情況，您可以指定另一個區域，然後重新執行 Maven 命令來部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="cf452-220">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="cf452-221">已部署您的網站時，您就可以使用 [Azure 入口網站]來管理它。</span><span class="sxs-lookup"><span data-stu-id="cf452-221">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="cf452-222">您的 Web 應用程式會列在**應用程式服務** 中：</span><span class="sxs-lookup"><span data-stu-id="cf452-222">Your web app will be listed in **App Services**:</span></span>

   ![列在 Azure 入口網站應用程式服務中的 Web 應用程式][AP01]

* <span data-ttu-id="cf452-224">Web 應用程式的 URL 會列在 Web 應用程式的 [概觀] 中：</span><span class="sxs-lookup"><span data-stu-id="cf452-224">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![決定 Web 應用程式的 URL][AP02]

## <a name="next-steps"></a><span data-ttu-id="cf452-226">後續步驟</span><span class="sxs-lookup"><span data-stu-id="cf452-226">Next steps</span></span>

<span data-ttu-id="cf452-227">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="cf452-227">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cf452-228">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="cf452-228">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="cf452-229">其他資源</span><span class="sxs-lookup"><span data-stu-id="cf452-229">Additional Resources</span></span>

<span data-ttu-id="cf452-230">如需本文所討論之各種技術的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="cf452-230">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="cf452-231">[適用於 Azure Web 應用程式的 Maven 外掛程式]</span><span class="sxs-lookup"><span data-stu-id="cf452-231">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="cf452-232">從 Azure CLI 登入 Azure</span><span class="sxs-lookup"><span data-stu-id="cf452-232">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="cf452-233">使用 Azure CLI 2.0 來建立 Azure 服務主體</span><span class="sxs-lookup"><span data-stu-id="cf452-233">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="cf452-234">Maven 設定參考</span><span class="sxs-lookup"><span data-stu-id="cf452-234">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="cf452-235">[適用於 Maven 的 Docker 外掛程式]</span><span class="sxs-lookup"><span data-stu-id="cf452-235">[Docker plugin for Maven]</span></span>

<span data-ttu-id="cf452-236">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="cf452-236">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Azure 入口網站]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[適用於 Azure Web 應用程式的 Maven 外掛程式]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service/containers/tutorial-custom-docker-image
[Docker]: https://www.docker.com/
[適用於 Maven 的 Docker 外掛程式]: https://github.com/spotify/docker-maven-plugin
[Docker plugin for Maven]: https://github.com/spotify/docker-maven-plugin
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP02.png
[TL01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/TL01.png
