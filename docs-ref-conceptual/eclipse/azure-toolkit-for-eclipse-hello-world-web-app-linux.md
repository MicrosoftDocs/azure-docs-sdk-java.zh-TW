---
title: 使用 Azure Toolkit for Eclipse，在雲端中部署 Linux 容器中執行的 Hello World Web 應用程式
description: 使用 Azure Toolkit for Eclipse，在 Linux 容器中執行基本的 Hello World Web 應用程式並部署到雲端。
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
ms.openlocfilehash: 799f21a282956f9a88aa35743157fc1292197569
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55649244"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="a3a04-103">使用 Azure Toolkit for Eclipse 將 Hello World Web 應用程式部署到雲端的 Linux 容器</span><span class="sxs-lookup"><span data-stu-id="a3a04-103">Deploy a Hello World web app to a Linux container in the cloud using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="a3a04-104">[Docker] 容器是部署 Web 應用程式的常用方法。</span><span class="sxs-lookup"><span data-stu-id="a3a04-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="a3a04-105">藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="a3a04-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="a3a04-106">Azure Toolkit for Eclipse 在部署容器至 Microsoft Azure 方面新增了功能，藉此為 Java 開發人員簡化部署程序。</span><span class="sxs-lookup"><span data-stu-id="a3a04-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="a3a04-107">本文示範建立基本的 Hello World Web 應用程式，並使用 Azure Toolkit for Eclipse 在 Linux 容器中發佈該 Web 應用程式所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="a3a04-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]
* <span data-ttu-id="a3a04-108">[Docker] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="a3a04-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="a3a04-109">為了完成本教學課程中的步驟，您需要設定 [Docker] 才能在沒有 TLS 的連接埠 2375 上公開精靈。</span><span class="sxs-lookup"><span data-stu-id="a3a04-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="a3a04-110">您可以在安裝 Docker 時或透過 Docker 設定功能表進行設定。</span><span class="sxs-lookup"><span data-stu-id="a3a04-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![Docker 設定功能表][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="a3a04-112">建立新的 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="a3a04-112">Create a new web app project</span></span>

1. <span data-ttu-id="a3a04-113">使用 [[Azure Toolkit for Eclipse 登入指示](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)] 文章中的步驟，來啟動 Eclipse 並登入您的 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="a3a04-113">Start Eclipse and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions) article.</span></span>

1. <span data-ttu-id="a3a04-114">依序按一下 [檔案] 功能表、[新增] 和 [動態 Web 專案]。</span><span class="sxs-lookup"><span data-stu-id="a3a04-114">Click the **File** menu, then click **New**, and then click **Dynamic Web Project**.</span></span>
   
   ![建立新專案][file-new-project]

1. <span data-ttu-id="a3a04-116">在 [新增動態 Web 專案] 對話方塊中，指定專案名稱和位置，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="a3a04-116">In the **New Dynamic Web Project** dialog box, specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定專案名稱][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="a3a04-118">建立 Azure Container Registry 以用作私人 Docker 登錄</span><span class="sxs-lookup"><span data-stu-id="a3a04-118">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="a3a04-119">下列步驟會逐步引導您使用 Azure 入口網站來建立 Azure Container Registry。</span><span class="sxs-lookup"><span data-stu-id="a3a04-119">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="a3a04-120">如果您想要使用 Azure CLI 而不是 Azure 入口網站，請遵循下列[使用 Azure CLI 2.0 建立私人 Docker 容器登錄][Create Docker Registry using Azure CLI]中的步驟。</span><span class="sxs-lookup"><span data-stu-id="a3a04-120">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="a3a04-121">瀏覽至 [Azure 入口網站]並登入。</span><span class="sxs-lookup"><span data-stu-id="a3a04-121">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="a3a04-122">一旦已在 Azure 入口網站登入您的帳戶後，就可以遵循[使用 Azure 入口網站建立私人 Docker 容器登錄]文章中的步驟，為便於了解，會在下列步驟中加以釋義。</span><span class="sxs-lookup"><span data-stu-id="a3a04-122">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="a3a04-123">依序按一下功能表的 [+ 建立資源] 圖示、[容器]，以及 [Container Registry]。</span><span class="sxs-lookup"><span data-stu-id="a3a04-123">Click the menu icon for **+ Create a resource**, then click **Containers**, and then click **Container Registry**.</span></span>
   
   ![建立新的 Azure Container Registry][create-container-registry-01]

1. <span data-ttu-id="a3a04-125">當 [建立容器登錄] 頁面顯示時，輸入您的 [登錄名稱] 和 [資源群組]，針對 [管理使用者] 選擇 [啟用]，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a3a04-125">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![設定 Azure Container Registry 設定][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="a3a04-127">在 Docker 容器中部署 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a3a04-127">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="a3a04-128">在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [新增 Docker 支援]。</span><span class="sxs-lookup"><span data-stu-id="a3a04-128">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="a3a04-129">這會自動建立具有預設設定的 Docker 檔案。</span><span class="sxs-lookup"><span data-stu-id="a3a04-129">This will automatically create a Docker file with a default configuration.</span></span>

   ![新增 Docker 支援][add-docker-support]

1. <span data-ttu-id="a3a04-131">新增 Docker 支援之後，請在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [發佈至用於容器的 Web App]。</span><span class="sxs-lookup"><span data-stu-id="a3a04-131">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Publish to Web App for Containers**.</span></span>

   ![發佈至用於容器的 Web App][run-on-web-app-for-containers]

1. <span data-ttu-id="a3a04-133">顯示 [在用於容器的 Web App 上執行] 對話方塊時，請填寫必要資訊：</span><span class="sxs-lookup"><span data-stu-id="a3a04-133">When the **Run on Web App for Containers** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="a3a04-134">**Docker 檔案**：這會指定將 Docker 支援新增至專案時所建立的 Docker 檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="a3a04-134">**Docker File**: This specifies the path to the Docker file that was created when you added Docker support to your project.</span></span> 

   * <span data-ttu-id="a3a04-135">**Container Registry**：從您在本文的上一節內建立的下拉式選單中，選取容器登錄。</span><span class="sxs-lookup"><span data-stu-id="a3a04-135">**Container Registry**: Choose the container registry from the drop-down menu that you created in the previous section of this article.</span></span> <span data-ttu-id="a3a04-136">系統會自動填入 [伺服器 URL]、[使用者名稱]和 [密碼] 欄位。</span><span class="sxs-lookup"><span data-stu-id="a3a04-136">The fields for **Server URL**, **Username**, and **Password** will be automatically populated.</span></span>

   * <span data-ttu-id="a3a04-137">**映像與標記**：指定容器映像名稱；這通常會使用下列語法："*registry*.azurecr.io/*appname*:latest"，其中：</span><span class="sxs-lookup"><span data-stu-id="a3a04-137">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="a3a04-138">registry 是本文先前章節中的容器登錄</span><span class="sxs-lookup"><span data-stu-id="a3a04-138">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="a3a04-139">appname 是 Web 應用程式的名稱</span><span class="sxs-lookup"><span data-stu-id="a3a04-139">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="a3a04-140">**用於容器的 Web App**：選擇 [使用現有的] 或 [建立新的] 來指定是否要將容器部署至現有的 Web 應用程式或建立新的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a3a04-140">**Web App for Container**: Choose **Use Existing** or **Create New** to specify whether you will deploy your container to an existing web app or create a new web app.</span></span>  <span data-ttu-id="a3a04-141">您指定的**應用程式名稱**將為 Web 應用程式建立 URL；例如：*wingtiptoys.azurewebsites.net* 。</span><span class="sxs-lookup"><span data-stu-id="a3a04-141">The **App name** that you specify will create the URL for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   * <span data-ttu-id="a3a04-142">**資源群組**：指定是否要使用現有的資源群組或建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="a3a04-142">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="a3a04-143">**App Service 方案**：指定是否要使用現有的應用程式服務方案或建立新的應用程式服務方案。</span><span class="sxs-lookup"><span data-stu-id="a3a04-143">**App Service Plan**: Specifies whether you will use an existing or create a new app service plan.</span></span> 

   ![在用於容器的 Web App 上執行][run-on-web-app-linux]

1. <span data-ttu-id="a3a04-145">完成以上所列設定時，請按一下 [確定] 以發佈 Web 應用程式至 Azure。</span><span class="sxs-lookup"><span data-stu-id="a3a04-145">When you have finished configuring the settings listed above, click **OK** to publish your web app to Azure.</span></span>

1. <span data-ttu-id="a3a04-146">發佈 Web 應用程式之後，您可以瀏覽至稍早為 Web 應用程式指定的 URL；例如：*wingtiptoys.azurewebsites.net*。</span><span class="sxs-lookup"><span data-stu-id="a3a04-146">After your web app has been published, you can browse to the URL that specifed earlier for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   ![瀏覽至您的 Web 應用程式][browsing-to-web-app]

## <a name="next-steps"></a><span data-ttu-id="a3a04-148">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a3a04-148">Next steps</span></span>

<span data-ttu-id="a3a04-149">如需 Docker 的其他資源，請參閱 [Docker 的官方網站][Docker]。</span><span class="sxs-lookup"><span data-stu-id="a3a04-149">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

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

[add-docker-support]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-eclipse-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/file-new-project.png
[project-name]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-linux.png
