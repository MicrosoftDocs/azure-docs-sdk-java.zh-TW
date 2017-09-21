---
title: "使用適用於 IntelliJ 的 Azure 工具組，在 Linux 容器中執行 Hello World Web 應用程式"
description: "了解如何在 Linux 容器中建立基本的 Hello World Web 應用程式，並使用適用於 IntelliJ 的 Azure 工具組將其發佈至 Azure。"
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: b6b8db6c7713157a35b6a113d7ec69b4419386d4
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2017
---
# <a name="run-a-hello-world-web-app-in-a-linux-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="a648b-103">使用適用於 IntelliJ 的 Azure 工具組，在 Linux 容器中執行 Hello World Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a648b-103">Run a Hello World web app in a Linux container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="a648b-104">[Docker] 容器是部署 Web 應用程式的常用方法。</span><span class="sxs-lookup"><span data-stu-id="a648b-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="a648b-105">藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="a648b-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="a648b-106">適用於 IntelliJ 的 Azure 工具組在部署容器至 Microsoft Azure 方面新增了功能，藉此為 Java 開發人員簡化部署程序。</span><span class="sxs-lookup"><span data-stu-id="a648b-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="a648b-107">本文示範建立基本的 Hello World Web 應用程式，並使用適用於 IntelliJ 的 Azure 工具組在 Linux 容器中發佈該 Web 應用程式所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="a648b-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* <span data-ttu-id="a648b-108">[Docker] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="a648b-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="a648b-109">為了完成本教學課程中的步驟，您需要設定 [Docker] 才能在沒有 TLS 的連接埠 2375 上公開精靈。</span><span class="sxs-lookup"><span data-stu-id="a648b-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="a648b-110">您可以在安裝 Docker 時或透過 Docker 設定功能表進行設定。</span><span class="sxs-lookup"><span data-stu-id="a648b-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![Docker 設定功能表][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="a648b-112">建立新的 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="a648b-112">Create a new web app project</span></span>

1. <span data-ttu-id="a648b-113">使用 [適用於 IntelliJ 的 Azure 工具組登入指示] 文章中的步驟來啟動 IntelliJ 並登入您的 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="a648b-113">Start IntelliJ and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="a648b-114">按一下 [檔案] 功能表，按一下 [新增]，然後按一下 [專案]。</span><span class="sxs-lookup"><span data-stu-id="a648b-114">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![建立新專案][file-new-project]

1. <span data-ttu-id="a648b-116">在 **新增專案** 對話方塊中，選取 **Maven** ，然後選取 **maven-archetype-webapp** ，再按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="a648b-116">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![選擇 Maven archetype webapp][maven-archetype-webapp]
   
1. <span data-ttu-id="a648b-118">指定 Web 應用程式的 [GroupId] 和 [ArtifactId]，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="a648b-118">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="a648b-120">自訂任何 Maven 設定或接受預設值，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="a648b-120">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![指定 Maven 設定][maven-options]

1. <span data-ttu-id="a648b-122">指定專案名稱和位置，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="a648b-122">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定專案名稱][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="a648b-124">建立 Azure Container Registry 以用作私人 Docker 登錄</span><span class="sxs-lookup"><span data-stu-id="a648b-124">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="a648b-125">下列步驟會逐步引導您使用 Azure 入口網站來建立 Azure Container Registry。</span><span class="sxs-lookup"><span data-stu-id="a648b-125">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="a648b-126">如果您想要使用 Azure CLI 而不是 Azure 入口網站，請遵循下列[使用 Azure CLI 2.0 建立私人 Docker 容器登錄][Create Docker Registry using Azure CLI]中的步驟。</span><span class="sxs-lookup"><span data-stu-id="a648b-126">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="a648b-127">瀏覽至 [Azure 入口網站]並登入。</span><span class="sxs-lookup"><span data-stu-id="a648b-127">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="a648b-128">一旦您已在 Azure 入口網站登入您的帳戶後，就可以遵循[使用 Azure 入口網站建立私人 Docker 容器登錄]文章中的步驟，為便於了解，會在下列步驟中加以釋義。</span><span class="sxs-lookup"><span data-stu-id="a648b-128">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="a648b-129">依序按一下功能表的 [+ 新增] 圖示、[容器]，以及 [Azure Container Registry]。</span><span class="sxs-lookup"><span data-stu-id="a648b-129">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![建立新的 Azure Container Registry][AR01]

1. <span data-ttu-id="a648b-131">當 Azure Container Registry 範本的資訊頁面顯示時，按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a648b-131">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![建立新的 Azure Container Registry][AR02]

1. <span data-ttu-id="a648b-133">當 [建立容器登錄] 頁面顯示時，輸入您的 [登錄名稱] 和 [資源群組]，針對 [管理使用者] 選擇 [啟用]，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a648b-133">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![設定 Azure Container Registry 設定][AR03]

1. <span data-ttu-id="a648b-135">一旦建立容器登錄之後，瀏覽至 Azure 入口網站中的容器登錄，然後按一下 [存取金鑰]。</span><span class="sxs-lookup"><span data-stu-id="a648b-135">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="a648b-136">將後續步驟的使用者名稱和密碼記下。</span><span class="sxs-lookup"><span data-stu-id="a648b-136">Take note of the username and password for the next steps.</span></span>

   ![Azure Container Registry 存取金鑰][AR04]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="a648b-138">在 Docker 容器中部署 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a648b-138">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="a648b-139">在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [新增 Docker 支援]。</span><span class="sxs-lookup"><span data-stu-id="a648b-139">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="a648b-140">這會自動建立具有預設設定的 Docker 檔案。</span><span class="sxs-lookup"><span data-stu-id="a648b-140">This will automatically create a Docker file with a default configuration.</span></span>

   ![新增 Docker 支援][add-docker-support]

1. <span data-ttu-id="a648b-142">新增 Docker 支援之後，請在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [在 Web 應用程式 (Linux) 上執行]。</span><span class="sxs-lookup"><span data-stu-id="a648b-142">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Run on Web App (Linux)**.</span></span>

   ![在 Web 應用程式 (Linux) 上執行][run-on-web-app-linux]

1. <span data-ttu-id="a648b-144">顯示 [在 Web 應用程式 (Linux) 上執行] 對話方塊時，請填寫必要資訊：</span><span class="sxs-lookup"><span data-stu-id="a648b-144">When the **Run on Web App (Linux)** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="a648b-145">[名稱]：這會指定將顯示於 Azure 工具組的易記名稱。</span><span class="sxs-lookup"><span data-stu-id="a648b-145">**Name**: This specifies the friendly name which is displayed in the Azure Toolkit.</span></span> 

   * <span data-ttu-id="a648b-146">[伺服器 URL]：這會指定本文先前章節中容器登錄的 URL；這通常會使用下列語法："*registry*.azurecr.io"。</span><span class="sxs-lookup"><span data-stu-id="a648b-146">**Server URL**: This specifies the URL for your container registry from the previous section of this article; typically this will use the following syntax: "*registry*.azurecr.io".</span></span> 

   * <span data-ttu-id="a648b-147">[使用者名稱] 和 [密碼]：指定本文先前章節中容器登錄的存取金鑰。</span><span class="sxs-lookup"><span data-stu-id="a648b-147">**Username** and **Password**: Specifies the access keys for your container registry from the previous section of this article.</span></span> 

   * <span data-ttu-id="a648b-148">[映像與標記]：指定容器映像名稱；這通常會使用下列語法："*registry*.azurecr.io/*appname*:latest"，其中：</span><span class="sxs-lookup"><span data-stu-id="a648b-148">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="a648b-149">registry 是本文先前章節中的容器登錄</span><span class="sxs-lookup"><span data-stu-id="a648b-149">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="a648b-150">appname 是 Web 應用程式的名稱</span><span class="sxs-lookup"><span data-stu-id="a648b-150">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="a648b-151">[使用現有的 Web 應用程式] 或 [建立新的 Web 應用程式]：指定您是否要將容器部署至現有的 Web 應用程式或建立新的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a648b-151">**Use Existing Web App** or **Create New Web App**: Specifies whether you will deploy your container to an existing web app or create a new web app.</span></span> 

   * <span data-ttu-id="a648b-152">[資源群組]：指定您是否要使用現有的資源群組或建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="a648b-152">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="a648b-153">[App Service 方案]：指定您是否要使用現有的 App Service 方案或建立新的 App Service 方案。</span><span class="sxs-lookup"><span data-stu-id="a648b-153">**App Service Plan**: Specifies whether you willuse an existing or create a new app service plan.</span></span> 

1. <span data-ttu-id="a648b-154">完成以上所列設定時，請按一下 [執行]。</span><span class="sxs-lookup"><span data-stu-id="a648b-154">When you have finished configuring the settings listed above, click **Run**.</span></span>

   ![建立 Web 應用程式][create-web-app]

1. <span data-ttu-id="a648b-156">發佈 Web 應用程式之後，系統會將您的設定儲存為預設值，您可以按一下工具列上的綠色箭號圖示，在 Azure 上執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a648b-156">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="a648b-157">您可以按一下 Web 應用程式的下拉式功能表，然後按一下 [編輯組態]，修改這些設定。</span><span class="sxs-lookup"><span data-stu-id="a648b-157">You can modify these settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![[編輯組態] 功能表][edit-configuration-menu]

1. <span data-ttu-id="a648b-159">顯示 [執行/偵錯組態] 對話方塊時，您可以修改任何預設設定，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a648b-159">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![[編輯組態] 對話方塊][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="a648b-161">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a648b-161">Next steps</span></span>

<span data-ttu-id="a648b-162">如需 Docker 的其他資源，請參閱 [Docker 的官方網站][Docker]。</span><span class="sxs-lookup"><span data-stu-id="a648b-162">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure 入口網站]: https://portal.azure.com/
[使用 Azure 入口網站建立私人 Docker 容器登錄]: /azure/container-registry/container-registry-get-started-portal
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[AR01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR01.png
[AR02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR02.png
[AR03]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR03.png
[AR04]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR04.png

[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[create-web-app]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-web-app.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
