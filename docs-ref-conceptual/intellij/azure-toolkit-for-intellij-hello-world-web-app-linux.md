---
title: 使用適用於 IntelliJ 的 Azure 工具組，在雲端中部署 Linux 容器中執行的 Hello World Web 應用程式
description: 使用適用於 IntelliJ 的 Azure 工具組，在 Linux 容器中執行基本的 Hello World Web 應用程式並部署到雲端。
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/20/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: fdff8dc2bd7a29473314d5c0bc99b7bcda369156
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55648722"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="9e5c2-103">使用適用於 IntelliJ 的 Azure 工具組，在雲端中將 Hello World Web 應用程式部署到 Linux 容器</span><span class="sxs-lookup"><span data-stu-id="9e5c2-103">Deploy a Hello World web app to a Linux container in the cloud using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="9e5c2-104">[Docker] 容器是部署 Web 應用程式的常用方法。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="9e5c2-105">藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="9e5c2-106">適用於 IntelliJ 的 Azure 工具組在部署容器至 Microsoft Azure 方面新增了功能，藉此為 Java 開發人員簡化部署程序。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="9e5c2-107">本文示範建立基本的 Hello World Web 應用程式，並使用適用於 IntelliJ 的 Azure 工具組在 Linux 容器中發佈該 Web 應用程式所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* <span data-ttu-id="9e5c2-108">[Docker] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="9e5c2-109">為了完成本教學課程中的步驟，您需要設定 [Docker] 才能在沒有 TLS 的連接埠 2375 上公開精靈。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="9e5c2-110">您可以在安裝 Docker 時或透過 Docker 設定功能表進行設定。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![Docker 設定功能表][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="9e5c2-112">建立新的 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="9e5c2-112">Create a new web app project</span></span>

1. <span data-ttu-id="9e5c2-113">使用[適用於 IntelliJ 的 Azure 工具組登入指示](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions)文章中的步驟，來啟動 IntelliJ 並登入您的 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-113">Start IntelliJ and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions) article.</span></span>

1. <span data-ttu-id="9e5c2-114">按一下 [檔案] 功能表，按一下 [新增]，然後按一下 [專案]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-114">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![建立新專案][file-new-project]

1. <span data-ttu-id="9e5c2-116">在 **新增專案** 對話方塊中，選取 **Maven** ，然後選取 **maven-archetype-webapp** ，再按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-116">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![選擇 Maven archetype webapp][maven-archetype-webapp]
   
1. <span data-ttu-id="9e5c2-118">指定 Web 應用程式的 [GroupId] 和 [ArtifactId]，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-118">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="9e5c2-120">自訂任何 Maven 設定或接受預設值，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-120">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![指定 Maven 設定][maven-options]

1. <span data-ttu-id="9e5c2-122">指定專案名稱和位置，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-122">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定專案名稱][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="9e5c2-124">建立 Azure Container Registry 以用作私人 Docker 登錄</span><span class="sxs-lookup"><span data-stu-id="9e5c2-124">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="9e5c2-125">下列步驟會逐步引導您使用 Azure 入口網站來建立 Azure Container Registry。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-125">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="9e5c2-126">如果您想要使用 Azure CLI 而不是 Azure 入口網站，請遵循下列[使用 Azure CLI 2.0 建立私人 Docker 容器登錄][Create Docker Registry using Azure CLI]中的步驟。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-126">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="9e5c2-127">瀏覽至 [Azure 入口網站]並登入。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-127">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="9e5c2-128">一旦已在 Azure 入口網站登入您的帳戶後，就可以遵循[使用 Azure 入口網站建立私人 Docker 容器登錄]文章中的步驟，為便於了解，會在下列步驟中加以釋義。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-128">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="9e5c2-129">依序按一下功能表的 [+ 建立資源] 圖示、[容器]，以及 [Container Registry]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-129">Click the menu icon for **+ Create a resource**, then click **Containers**, and then click **Container Registry**.</span></span>
   
   ![建立新的 Azure Container Registry][create-container-registry-01]

1. <span data-ttu-id="9e5c2-131">當 [建立容器登錄] 頁面顯示時，輸入您的 [登錄名稱] 和 [資源群組]，針對 [管理使用者] 選擇 [啟用]，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-131">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![設定 Azure Container Registry 設定][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="9e5c2-133">在 Docker 容器中部署 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9e5c2-133">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="9e5c2-134">在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [新增 Docker 支援]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-134">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="9e5c2-135">這會自動建立具有預設設定的 Docker 檔案。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-135">This will automatically create a Docker file with a default configuration.</span></span>

   ![新增 Docker 支援][add-docker-support]

1. <span data-ttu-id="9e5c2-137">新增 Docker 支援之後，請在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [在用於容器的 Web App 上執行]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-137">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Run on Web App for Containers**.</span></span>

   ![在用於容器的 Web App 上執行][run-on-web-app-for-containers]

1. <span data-ttu-id="9e5c2-139">顯示 [在用於容器的 Web App 上執行] 對話方塊時，請填寫必要資訊：</span><span class="sxs-lookup"><span data-stu-id="9e5c2-139">When the **Run on Web App for Containers** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="9e5c2-140">**名稱**：這會指定將顯示於 Azure 工具組的易記名稱。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-140">**Name**: This specifies the friendly name which is displayed in the Azure Toolkit.</span></span> 

   * <span data-ttu-id="9e5c2-141">**Container Registry**：從您在本文的上一節內建立的下拉式選單中，選取容器登錄。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-141">**Container Registry**: Choose the container registry from the drop-down menu that you created in the previous section of this article.</span></span> <span data-ttu-id="9e5c2-142">系統會自動填入 [伺服器 URL]、[使用者名稱]和 [密碼] 欄位。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-142">The fields for **Server URL**, **Username**, and **Password** will be automatically populated.</span></span>

   * <span data-ttu-id="9e5c2-143">**映像與標記**：指定容器映像名稱；這通常會使用下列語法："*registry*.azurecr.io/*appname*:latest"，其中：</span><span class="sxs-lookup"><span data-stu-id="9e5c2-143">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="9e5c2-144">registry 是本文先前章節中的容器登錄</span><span class="sxs-lookup"><span data-stu-id="9e5c2-144">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="9e5c2-145">appname 是 Web 應用程式的名稱</span><span class="sxs-lookup"><span data-stu-id="9e5c2-145">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="9e5c2-146">**使用現有的 Web 應用程式**或**建立新的 Web 應用程式**：指定是否要將容器部署至現有的 Web 應用程式或建立新的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-146">**Use Existing Web App** or **Create New Web App**: Specifies whether you will deploy your container to an existing web app or create a new web app.</span></span> <span data-ttu-id="9e5c2-147">您指定的**應用程式名稱**將為 Web 應用程式建立 URL；例如：*wingtiptoys.azurewebsites.net* 。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-147">The **App name** that you specify will create the URL for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   * <span data-ttu-id="9e5c2-148">**資源群組**：指定是否要使用現有的資源群組或建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-148">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="9e5c2-149">**App Service 方案**：指定是否要使用現有的應用程式服務方案或建立新的應用程式服務方案。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-149">**App Service Plan**: Specifies whether you will use an existing or create a new app service plan.</span></span> 

   ![在用於容器的 Web App 上執行][run-on-web-app-linux]

1. <span data-ttu-id="9e5c2-151">完成以上所列設定時，請按一下 [執行]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-151">When you have finished configuring the settings listed above, click **Run**.</span></span> <span data-ttu-id="9e5c2-152">成功部署 Web 應用程式後，狀態會顯示在 [執行] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-152">When your web app has been successfully deployed, the status will be displayed in the **Run** window.</span></span>

   ![已成功部署 Web 應用程式][successfully-deployed]

1. <span data-ttu-id="9e5c2-154">發佈 Web 應用程式之後，您可以瀏覽至稍早為 Web 應用程式指定的 URL；例如：*wingtiptoys.azurewebsites.net*。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-154">After your web app has been published, you can browse to the URL that specifed earlier for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   ![瀏覽至您的 Web 應用程式][browsing-to-web-app]

## <a name="optional-modify-your-web-app-publish-settings"></a><span data-ttu-id="9e5c2-156">選用：修改您的 Web 應用程式發佈設定</span><span class="sxs-lookup"><span data-stu-id="9e5c2-156">Optional: Modify your web app publish settings</span></span>

1. <span data-ttu-id="9e5c2-157">發佈 Web 應用程式之後，系統會將設定儲存為預設值，您可以按一下工具列上的綠色箭號圖示，在 Azure 上執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-157">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="9e5c2-158">您可以按一下 Web 應用程式的下拉式功能表，然後按一下 [編輯組態]，修改這些設定。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-158">You can modify these settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![[編輯組態] 功能表][edit-configuration-menu]

1. <span data-ttu-id="9e5c2-160">顯示 [執行/偵錯組態] 對話方塊時，您可以修改任何預設設定，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-160">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![[編輯組態] 對話方塊][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="9e5c2-162">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9e5c2-162">Next steps</span></span>

<span data-ttu-id="9e5c2-163">如需 Docker 的其他資源，請參閱 [Docker 的官方網站][Docker]。</span><span class="sxs-lookup"><span data-stu-id="9e5c2-163">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Azure 入口網站]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[使用 Azure 入口網站建立私人 Docker 容器登錄]: /azure/container-registry/container-registry-get-started-portal
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-intellij-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
