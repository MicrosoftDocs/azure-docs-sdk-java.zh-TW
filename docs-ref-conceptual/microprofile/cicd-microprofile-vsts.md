---
title: 使用 Azure DevOps 為 MicroProfile 應用程式設定 CI/CD
description: 了解如何使用 Azure DevOps 設定 CI/CD 發行週期，並將 MicroProfile 應用程式部署至用於容器的 Azure Web App
services: Azure DevOps
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/14/2018
ms.devlang: Java
ms.service: Azure DevOps
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: c2b6bf3370982d26d8d23fede370e0105a70b734
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506329"
---
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a><span data-ttu-id="c87fe-103">使用 Azure DevOps 為 MicroProfile 應用程式設定 CI/CD</span><span class="sxs-lookup"><span data-stu-id="c87fe-103">CI/CD for MicroProfile applications using Azure DevOps</span></span>

<span data-ttu-id="c87fe-104">本教學課程將示範 Java EE 開發人員如何輕鬆地使用 Azure DevOps (先前稱為 VSTS) 設定 CI/CD 發行週期，藉此將他們的 [MicroProfile](http://microprofile.io) 應用程式部署至用於容器的 Web App。</span><span class="sxs-lookup"><span data-stu-id="c87fe-104">This tutorial will show how Java EE developers can easily setup a CI/CD release cycle to deploy their [MicroProfile](http://microprofile.io) applications to an Azure Web App for Containers using Azure DevOps (formally known as VSTS).</span></span>  <span data-ttu-id="c87fe-105">在此範例中，我們使用的 MicroProfile 應用程式會使用 [Payara Micro](https://www.payara.fish/payara_micro) 作為基礎映像。</span><span class="sxs-lookup"><span data-stu-id="c87fe-105">In this example, we’ll be using a MicroProfile application that uses a [Payara Micro](https://www.payara.fish/payara_micro) as a base image.</span></span>   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
<span data-ttu-id="c87fe-106">我們將以建置 Docker 映像作為開頭來進行 Azure DevOps 容器化程序，並將容器映像推送至 Azure 容器登錄。</span><span class="sxs-lookup"><span data-stu-id="c87fe-106">We will start the Azure DevOps containerize process by building a Docker image and pushing the container image to an Azure Container Register.</span></span>  <span data-ttu-id="c87fe-107">然後藉由 Azure DevOps 發行管線將容器映像部署至 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c87fe-107">Then complete with a Azure DevOps release pipeline to deploy the container image to a Web App.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c87fe-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="c87fe-108">Prerequisites</span></span>
- <span data-ttu-id="c87fe-109">從 [Github](https://github.com/Azure-Samples/microprofile-hello-azure) 複製並儲存 Git URL</span><span class="sxs-lookup"><span data-stu-id="c87fe-109">Copy and save the Git url from [Github](https://github.com/Azure-Samples/microprofile-hello-azure)</span></span>
- <span data-ttu-id="c87fe-110">註冊或登入您的 [Azure DevOps](https://dev.azure.com) 帳戶</span><span class="sxs-lookup"><span data-stu-id="c87fe-110">Register or Log into your [Azure DevOps](https://dev.azure.com) account</span></span>
- <span data-ttu-id="c87fe-111">建立新的 [Azure DevOps 專案](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav)，並使用上述的 Git URL 來**匯入存放庫**</span><span class="sxs-lookup"><span data-stu-id="c87fe-111">Create a new [Azure DevOps project](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav) and use the above Git url to **import a repository**</span></span>
- <span data-ttu-id="c87fe-112">建立 [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR)</span><span class="sxs-lookup"><span data-stu-id="c87fe-112">Create an [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR)</span></span>
- <span data-ttu-id="c87fe-113">建立用於容器的 Azure Web App</span><span class="sxs-lookup"><span data-stu-id="c87fe-113">Create an Azure Web App for Container</span></span>
> [!NOTE]
>
> <span data-ttu-id="c87fe-114">佈建 Web 應用程式執行個體時，選取容器設定中的 [快速入門]</span><span class="sxs-lookup"><span data-stu-id="c87fe-114">Select "Quickstart" in the Container Settings when provisioning the Web App instance</span></span>


## <a name="create-a-build-definition"></a><span data-ttu-id="c87fe-115">建立組建定義</span><span class="sxs-lookup"><span data-stu-id="c87fe-115">Create a Build definition</span></span>

<span data-ttu-id="c87fe-116">每當 Java EE 應用程式的來源應用程式中有認可時，Azure DevOps 中的組建定義就會自動執行組建中所有工作。</span><span class="sxs-lookup"><span data-stu-id="c87fe-116">The build definition in Azure DevOps automatically executes all the tasks in the build each time there’s a commit in Java EE application source application.</span></span>  <span data-ttu-id="c87fe-117">在此範例中，Azure DevOps 將使用 Maven 來建置 Java MicroProfile 專案。</span><span class="sxs-lookup"><span data-stu-id="c87fe-117">In this example, Azure DevOps will use Maven to build the Java MicroProfile project.</span></span>

1. <span data-ttu-id="c87fe-118">在 Azure DevOps 專案頁面的頂端按一下 [組建與發行] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="c87fe-118">Click on the "Build and Release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="c87fe-119">然後，選取 [組建] 連結</span><span class="sxs-lookup"><span data-stu-id="c87fe-119">Then, select the **Builds** link</span></span> 

<img src="media/VSTS/Buid-and-Release1.png">

2. <span data-ttu-id="c87fe-120">按一下 [新增管線] 按鈕，然後按一下 [繼續] 來開始定義您的組建工作</span><span class="sxs-lookup"><span data-stu-id="c87fe-120">Click on the **New Pipeline** button, and then **Continue** to start defining your build tasks</span></span>
3. <span data-ttu-id="c87fe-121">從範本清單選取 [Maven]，然後按一下 [套用] 按鈕來建立您的 Java 專案</span><span class="sxs-lookup"><span data-stu-id="c87fe-121">Select "Maven" from the list of templates, then click on the **Apply** button to build your Java project</span></span>
4. <span data-ttu-id="c87fe-122">使用 [代理程式集區] 欄位的下拉式功能表來選取 [託管的 Linux 預覽] 選項。</span><span class="sxs-lookup"><span data-stu-id="c87fe-122">Use the drop-down menu for the Agent pool field to select **Hosted Linux Preview** option.</span></span>
> [!NOTE]
>
> <span data-ttu-id="c87fe-123">這會告訴 Azure DevOps 要使用哪個組建伺服器。</span><span class="sxs-lookup"><span data-stu-id="c87fe-123">This informs Azure DevOps which build server to use.</span></span>  <span data-ttu-id="c87fe-124">您可以使用您私人的自訂組建伺服器</span><span class="sxs-lookup"><span data-stu-id="c87fe-124">You can use your private customized build server</span></span>

5. <span data-ttu-id="c87fe-125">若要設定您的組建以進行持續整合，請選取 [觸發程序] 索引標籤，然後核取 [啟用持續整合] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="c87fe-125">To configure your build for continuous integration, select the **Triggers** tab and check the **Enable continuous integration** checkbox.</span></span>  

<img src="media/VSTS/Build-Triggers2.png"> 
 
6. <span data-ttu-id="c87fe-126">選取 [工作] 索引標籤可返回主要的組建管線頁面</span><span class="sxs-lookup"><span data-stu-id="c87fe-126">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
7. <span data-ttu-id="c87fe-127">使用 [儲存與佇列] 下拉式功能表來選取 [儲存] 選項</span><span class="sxs-lookup"><span data-stu-id="c87fe-127">Use the **Save & queue** drop-down menu to select the **Save** option</span></span>
 

## <a name="create-a-docker-build-image"></a><span data-ttu-id="c87fe-128">建立 Docker 組建映像</span><span class="sxs-lookup"><span data-stu-id="c87fe-128">Create a Docker Build Image</span></span>

<span data-ttu-id="c87fe-129">在此工作中，Azure DevOps 會使用 Dockerfile 和來自 Payara Micro 的基礎映像來建立 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="c87fe-129">In this task, Azure DevOps uses a Dockerfile with a base image from Payara Micro to create a Docker image.</span></span>  

1. <span data-ttu-id="c87fe-130">選取 [工作] 索引標籤可返回主要的組建管線頁面</span><span class="sxs-lookup"><span data-stu-id="c87fe-130">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
2. <span data-ttu-id="c87fe-131">按一下 [+] 圖示，即可將新工作新增至組建定義</span><span class="sxs-lookup"><span data-stu-id="c87fe-131">Click on the **+** icon to add new task to the build definition</span></span>
 
<img src="media/VSTS/Tasks-add4.png">
 
3. <span data-ttu-id="c87fe-132">從範本清單中選取 [Docker]，然後按一下 [新增] 按鈕</span><span class="sxs-lookup"><span data-stu-id="c87fe-132">Select "Docker" from the list of templates, then click on the **Add** button</span></span>
4. <span data-ttu-id="c87fe-133">在 [顯示名稱] 欄位中輸入描述性名稱</span><span class="sxs-lookup"><span data-stu-id="c87fe-133">Enter a description name for the **Display name** field</span></span>
5. <span data-ttu-id="c87fe-134">確認 [容器登錄類型] 的下拉式功能表中已選取 [Azure Container Registry]。</span><span class="sxs-lookup"><span data-stu-id="c87fe-134">Verify that **Azure Container Registry** is selected in the drop-down menu for **Container registry type**.</span></span>
> [!NOTE]
>
>  <span data-ttu-id="c87fe-135">如果您使用 Docker 中樞或另一個登錄，請改為選取 [容器登錄]。</span><span class="sxs-lookup"><span data-stu-id="c87fe-135">If you are using Docker Hub or another registry, select "Container Registry" instead.</span></span>  <span data-ttu-id="c87fe-136">然後按一下 [+ 新增] 按鈕，為其提供認證和連線資訊。</span><span class="sxs-lookup"><span data-stu-id="c87fe-136">Then click on the "+ New" button to provide the credentials and connection information for it.</span></span> <span data-ttu-id="c87fe-137">然後跳至命令區段以繼續。</span><span class="sxs-lookup"><span data-stu-id="c87fe-137">Then skip to the Commands section to continue.</span></span>

6. <span data-ttu-id="c87fe-138">使用 [Azure 訂用帳戶] 下拉式功能表來選擇您的 Azure 訂用帳戶識別碼。</span><span class="sxs-lookup"><span data-stu-id="c87fe-138">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>  <span data-ttu-id="c87fe-139">然後按一下 [授權] 按鈕</span><span class="sxs-lookup"><span data-stu-id="c87fe-139">Then click on the **Authorize** button</span></span>
7. <span data-ttu-id="c87fe-140">在 [Azure 容器登錄] 下拉式功能表中，選取您在 Azure 中建立的登錄名稱。</span><span class="sxs-lookup"><span data-stu-id="c87fe-140">In the **Azure container registry** drop-down menu, select registry name you created in Azure.</span></span>
8. <span data-ttu-id="c87fe-141">確認 [命令] 下拉式功能表中已選取 [組建] 選項。</span><span class="sxs-lookup"><span data-stu-id="c87fe-141">Verify that **build** option is selected in the **Command** drop-down menu.</span></span>
9. <span data-ttu-id="c87fe-142">針對 [Dockerfile]，按一下文字方塊旁邊的路徑瀏覽圖示，從 GitHub 專案中選取 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="c87fe-142">For the **Dockerfile**, click on the path navigation icon next to the textbox to select the Dockerfile from the github project.</span></span>  <span data-ttu-id="c87fe-143">然後按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c87fe-143">Then click the **OK** button.</span></span>

<img src="media/VSTS/Dockerfile5.png">

10. <span data-ttu-id="c87fe-144">在 [映像名稱] 底下，核取 [包含最新標記] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="c87fe-144">Under the **Image name**, check the **Include latest tag** checkbox.</span></span> 
11. <span data-ttu-id="c87fe-145">使用 [儲存與佇列] 下拉式功能表來選取 [儲存] 選項。</span><span class="sxs-lookup"><span data-stu-id="c87fe-145">Use the **Save & queue** drop-down menu to select the **Save** option.</span></span>

## <a name="push-docker-image-to-acr"></a><span data-ttu-id="c87fe-146">將 Docker 映像推送至 ACR</span><span class="sxs-lookup"><span data-stu-id="c87fe-146">Push Docker Image to ACR</span></span>

<span data-ttu-id="c87fe-147">在此工作中，Azure DevOps 會將 Docker 映像推送至 Azure Container Registry。</span><span class="sxs-lookup"><span data-stu-id="c87fe-147">In this task, Azure DevOps will push the docker image to your Azure Container Registry.</span></span>  <span data-ttu-id="c87fe-148">這將用來執行作為容器化 Java Web 應用程式的 MicroProfile API 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c87fe-148">This will be used to run the MicroProfile API application as a containerized Java web app.</span></span>

1. <span data-ttu-id="c87fe-149">由於我們將在 Azure DevOps 中使用 Docker，因此請重複上方**建立 Docker 組建映像**一節中的步驟 1-7 來建立新的 Docker 範本。</span><span class="sxs-lookup"><span data-stu-id="c87fe-149">Since we are using Docker in Azure DevOps, create a new Docker template by repeating steps 1 - 7 above in the **Create a Docker Build Image** section.</span></span>
2. <span data-ttu-id="c87fe-150">在 [命令] 下拉式功能表中選取 [推送]。</span><span class="sxs-lookup"><span data-stu-id="c87fe-150">Select **push** in the **Command** drop-down menu.</span></span>
3. <span data-ttu-id="c87fe-151">按一下 [儲存與佇列] 索引標籤，然後選取 [儲存與佇列] 選項。</span><span class="sxs-lookup"><span data-stu-id="c87fe-151">Click on the **Save & queue** tab, then select **Save & queue** option.</span></span>
4. <span data-ttu-id="c87fe-152">確認快顯視窗上的代理程式集區中已選取 [託管的 Linux 預覽]。</span><span class="sxs-lookup"><span data-stu-id="c87fe-152">Verify that the **Hosted Linux Preview** is select for the Agent pool on the pop-up window.</span></span>  <span data-ttu-id="c87fe-153">然後按一下 [儲存與佇列] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c87fe-153">Then click on the **Save & queue** button.</span></span>
5. <span data-ttu-id="c87fe-154">按一下組建編號，以確認 Java 專案的組建管線已成功完成。</span><span class="sxs-lookup"><span data-stu-id="c87fe-154">Click on the build number to verify that the build pipeline for the Java project completed successfully.</span></span>

<img src="media/VSTS/Build-Number6.png">
 

## <a name="create-a-release-definition-for-a-java-app"></a><span data-ttu-id="c87fe-155">建立 Java 應用程式的發行定義</span><span class="sxs-lookup"><span data-stu-id="c87fe-155">Create a release definition for a Java app</span></span>

<span data-ttu-id="c87fe-156">一旦組建程序順利完成，Azure DevOps 中的發行管線就會自動觸發目標環境 (例如 Azure) 的組建成品部署。</span><span class="sxs-lookup"><span data-stu-id="c87fe-156">The release pipeline in Azure DevOps automatically triggers the deployment of build artifacts to a target environment like Azure as soon as the Build process completes successfully.</span></span>   <span data-ttu-id="c87fe-157">您可以針對開發、測試、預備或生產環境建立發行管線。</span><span class="sxs-lookup"><span data-stu-id="c87fe-157">The release pipeline can be created for dev, test, staging or production environments.</span></span>

1. <span data-ttu-id="c87fe-158">在 Azure DevOps 專案頁面的頂端按一下 [組建與發行] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="c87fe-158">Click on the "Build and release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="c87fe-159">然後，選取 [發行] 連結。</span><span class="sxs-lookup"><span data-stu-id="c87fe-159">Then, select the **Releases** link.</span></span>

<img src="media/VSTS/Release-new-pipeline7.png">
 
2. <span data-ttu-id="c87fe-160">按一下 [新增管線] 按鈕</span><span class="sxs-lookup"><span data-stu-id="c87fe-160">Click on the "New pipeline\*\* button</span></span>
3. <span data-ttu-id="c87fe-161">在範本清單中選取 [將 Java 應用程式部署至 Azure App Service]，然後按一下 [套用] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c87fe-161">Select the **Deploy a Java app to Azure App Service** in the list of templates, then click on the **Apply** button.</span></span>

<img src="media/VSTS/deploy-java-template8.png">
 
4. <span data-ttu-id="c87fe-162">設定**階段名稱** (例如開發、測試、預備或生產)。</span><span class="sxs-lookup"><span data-stu-id="c87fe-162">Set a **Stage name** (e.g Dev, Test, Staging or Production).</span></span>  <span data-ttu-id="c87fe-163">然後按一下 [X] 按鈕來關閉快顯視窗</span><span class="sxs-lookup"><span data-stu-id="c87fe-163">Then click on the **X** button to close the pop-up window</span></span>
5. <span data-ttu-id="c87fe-164">在成品區段中按一下 [+ 新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c87fe-164">Click on the **+ Add** button in the Artifacts section.</span></span>  <span data-ttu-id="c87fe-165">這會將組建定義中的成品連結至此發行定義。</span><span class="sxs-lookup"><span data-stu-id="c87fe-165">This will link artifacts from the build definition to this release definition.</span></span>  
6. <span data-ttu-id="c87fe-166">使用 [來源 (組建管線)] 的下拉式功能表來選取您的組建定義。</span><span class="sxs-lookup"><span data-stu-id="c87fe-166">Use the drop-down menu for the **Source (build pipeline)** to select your build definition.</span></span> <span data-ttu-id="c87fe-167">然後按一下 [新增] 按鈕以繼續。</span><span class="sxs-lookup"><span data-stu-id="c87fe-167">Then click the **Add** button to continue.</span></span>

<img src="media/VSTS/add-artifact9.png">
 
7. <span data-ttu-id="c87fe-168">按一下管線上的 [工作] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="c87fe-168">Click on the **Tasks** tab on the pipeline.</span></span>  <span data-ttu-id="c87fe-169">然後，選取您的階段名稱。</span><span class="sxs-lookup"><span data-stu-id="c87fe-169">Then, select your stage name.</span></span>
 
<img src="media/VSTS/release-stage10.png">

8. <span data-ttu-id="c87fe-170">使用 [Azure 訂用帳戶] 下拉式功能表來選擇您的 Azure 訂用帳戶識別碼。</span><span class="sxs-lookup"><span data-stu-id="c87fe-170">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>
9. <span data-ttu-id="c87fe-171">從 [應用程式類型] 下拉式功能表中選取 [Linux 應用程式]</span><span class="sxs-lookup"><span data-stu-id="c87fe-171">Select **Linux App** from the **App type** drop-down menu</span></span>
10. <span data-ttu-id="c87fe-172">在 [應用程式服務名稱] 下拉式功能表中，選取先前所建立用於容器的 Web APP 執行個體名稱</span><span class="sxs-lookup"><span data-stu-id="c87fe-172">Select the name of the Web App for Container instance you created above in the **App service name** drop-down menu</span></span>
11. <span data-ttu-id="c87fe-173">在 [登錄或命名空間] 欄位中輸入您的 Azure 容器登錄名稱。</span><span class="sxs-lookup"><span data-stu-id="c87fe-173">Enter the name of your azure container registry in the **Registry or Namespaces** field.</span></span>  <span data-ttu-id="c87fe-174">例如 **myregistry.azure.io**</span><span class="sxs-lookup"><span data-stu-id="c87fe-174">E.g **myregistry.azure.io**</span></span>
12. <span data-ttu-id="c87fe-175">在 [存放庫] 欄位中輸入登錄名稱</span><span class="sxs-lookup"><span data-stu-id="c87fe-175">Enter the registry name in the **Repository** field</span></span>
13. <span data-ttu-id="c87fe-176">按一下 [部署 Azure App Service]。</span><span class="sxs-lookup"><span data-stu-id="c87fe-176">Click on **Deploy Azure App Service**.</span></span>  <span data-ttu-id="c87fe-177">在 [標記] 文字方塊中輸入容器映像的標記</span><span class="sxs-lookup"><span data-stu-id="c87fe-177">Enter the tag for the container image in the **Tag** textbox</span></span> 
14. <span data-ttu-id="c87fe-178">按一下 [在代理程式上執行]。</span><span class="sxs-lookup"><span data-stu-id="c87fe-178">Click on **Run on agent**.</span></span>  <span data-ttu-id="c87fe-179">在代理程式集區的下拉式功能表中選取 [託管的 Linux 預覽]</span><span class="sxs-lookup"><span data-stu-id="c87fe-179">Select **Hosted Linux Preview** in the Agent pool drop-down menu</span></span> 

## <a name="setup-environment-variables"></a><span data-ttu-id="c87fe-180">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="c87fe-180">Setup Environment Variables</span></span>

1. <span data-ttu-id="c87fe-181">按一下 [變數] 索引標籤。然後按一下 [+ 新增] 按鈕，以定義您的環境變數</span><span class="sxs-lookup"><span data-stu-id="c87fe-181">Click on the **Variables** tab.  Then click on the **+ Add** button to define your environment variables</span></span>
2. <span data-ttu-id="c87fe-182">為您的容器登錄 URL、使用者名稱和密碼新增變數名稱和值。</span><span class="sxs-lookup"><span data-stu-id="c87fe-182">Add the variable name and values for your container registry url, username and password.</span></span>   <span data-ttu-id="c87fe-183">基於安全考量，請按一下上鎖圖示來隱藏密碼值。</span><span class="sxs-lookup"><span data-stu-id="c87fe-183">For security, click on the lock icon to keep the password value hidden.</span></span>

<span data-ttu-id="c87fe-184">例如︰</span><span class="sxs-lookup"><span data-stu-id="c87fe-184">For example:</span></span>
- <span data-ttu-id="c87fe-185">registry.password</span><span class="sxs-lookup"><span data-stu-id="c87fe-185">registry.password</span></span>
- <span data-ttu-id="c87fe-186">registry.url</span><span class="sxs-lookup"><span data-stu-id="c87fe-186">registry.url</span></span>
- <span data-ttu-id="c87fe-187">registry.username</span><span class="sxs-lookup"><span data-stu-id="c87fe-187">registry.username</span></span>

<img src="media/VSTS/environment-variables12.png">

3. <span data-ttu-id="c87fe-188">按一下 [工作] 索引標籤可返回主要的發行管線定義頁面</span><span class="sxs-lookup"><span data-stu-id="c87fe-188">Click on the **Tasks** tab to return to the main release pipeline definition page</span></span>
4. <span data-ttu-id="c87fe-189">按一下 [部署 Azure App Service]。</span><span class="sxs-lookup"><span data-stu-id="c87fe-189">Click on **Deploy Azure App Service**.</span></span> 
5. <span data-ttu-id="c87fe-190">展開 [應用程式和組態設定] 區段，然後按一下 [應用程式設定] 欄位的瀏覽路徑來新增環境變數，即可在部署期間連線到容器登錄。</span><span class="sxs-lookup"><span data-stu-id="c87fe-190">Expand the **Application and Configuration Settings** section, then click on the navigation path for the **App Settings** field to add environments variable to connect to the container registry during deployment.</span></span>
6. <span data-ttu-id="c87fe-191">按一下 [+ 新增] 按鈕，以定義下列應用程式設定，並指派環境變數</span><span class="sxs-lookup"><span data-stu-id="c87fe-191">Click on the \*\* + Add\*\* button to define the following app settings and assign the environment variables</span></span>
- <span data-ttu-id="c87fe-192">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span><span class="sxs-lookup"><span data-stu-id="c87fe-192">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span></span>
- <span data-ttu-id="c87fe-193">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span><span class="sxs-lookup"><span data-stu-id="c87fe-193">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span></span>
- <span data-ttu-id="c87fe-194">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span><span class="sxs-lookup"><span data-stu-id="c87fe-194">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span></span>

<img src="media/VSTS/environment-variables14.png">
 
7. <span data-ttu-id="c87fe-195">按一下 [確定] 按鈕以繼續</span><span class="sxs-lookup"><span data-stu-id="c87fe-195">Click on the **OK** button to continue</span></span>

## <a name="setup-continious-deployment--deploy-java-application"></a><span data-ttu-id="c87fe-196">設定持續部署與部署 Java 應用程式</span><span class="sxs-lookup"><span data-stu-id="c87fe-196">Setup Continious Deployment & Deploy Java Application</span></span>

1. <span data-ttu-id="c87fe-197">若要啟用持續部署，請按一下 [管線] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="c87fe-197">To enable continuous deployment, click the **Pipelines** tab</span></span>
2. <span data-ttu-id="c87fe-198">在 [成品] 區段中，按一下閃電圖示。</span><span class="sxs-lookup"><span data-stu-id="c87fe-198">In the Artifacts section, click on the lightening icon.</span></span>  <span data-ttu-id="c87fe-199">然後將 [持續部署觸發程序] 設定為 [已啟用]。</span><span class="sxs-lookup"><span data-stu-id="c87fe-199">Then set the **Continuous deployment trigger** to Enabled.</span></span>

<img src="media/VSTS/release-enable-CD.png">
 
3. <span data-ttu-id="c87fe-200">按一下 [儲存] 按鈕，然後按一下 [確定] 按鈕</span><span class="sxs-lookup"><span data-stu-id="c87fe-200">Click on the **Save** button, then the **OK** button</span></span> 
4. <span data-ttu-id="c87fe-201">按一下 [+ 發行] 下拉式功能表，然後選取 [建立發行] 連結</span><span class="sxs-lookup"><span data-stu-id="c87fe-201">Click on the **+ Release** drop-down menu, then select **Create a release** link</span></span>
5. <span data-ttu-id="c87fe-202">使用 [觸發程序階段從自動變更為手動] 下拉式功能表，選取您階段名稱的核取方塊</span><span class="sxs-lookup"><span data-stu-id="c87fe-202">Use the **Stages for a trigger change from automated to manual** drop-down menu to select the checkbox for your stage name</span></span>
6. <span data-ttu-id="c87fe-203">按一下 [建立] 按鈕以繼續</span><span class="sxs-lookup"><span data-stu-id="c87fe-203">Click the **Create** button to continue</span></span>
7. <span data-ttu-id="c87fe-204">按一下發行編號。</span><span class="sxs-lookup"><span data-stu-id="c87fe-204">Click on the release number.</span></span>  <span data-ttu-id="c87fe-205">然後將滑鼠游標停留在階段名稱上，並按一下 [部署] 按鈕</span><span class="sxs-lookup"><span data-stu-id="c87fe-205">Then hover your mouse cursor over the stage name and click on the **Deploy** button</span></span>
8. <span data-ttu-id="c87fe-206">接著，在快顯視窗中按一下 [部署] 按鈕，以啟動部署至 Azure 的部署程序</span><span class="sxs-lookup"><span data-stu-id="c87fe-206">The click on the **Deploy** button on the pop-up window to start the deployment process to Azure</span></span>


## <a name="test-the-java-web-application"></a><span data-ttu-id="c87fe-207">測試 Java Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c87fe-207">Test the Java Web Application</span></span>
1. <span data-ttu-id="c87fe-208">在 Web 瀏覽器中執行 Web 應用程式 URL：</span><span class="sxs-lookup"><span data-stu-id="c87fe-208">Run the web app url in web browser:</span></span>  
<span data-ttu-id="c87fe-209">https://{your-app-service-name}.azurewebsites.net/api/hello</span><span class="sxs-lookup"><span data-stu-id="c87fe-209">https://{your-app-service-name}.azurewebsites.net/api/hello</span></span>

 
<img src="media/VSTS/web-app16.png">

2. <span data-ttu-id="c87fe-210">網頁應該會顯示 **Hello Azure!**</span><span class="sxs-lookup"><span data-stu-id="c87fe-210">The web page should say **Hello Azure!**</span></span>
 
<img src="media/VSTS/web-api17.png">
