---
title: "使用 IntelliJ 建立 Azure 的 Hello World Web 應用程式"
description: "本教學課程將示範如何使用適用於 IntelliJ 的 Azure 工具組來建立 Azure 的 Hello World Web 應用程式。"
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/15/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: 900e64667bcc2eed79fe80954c118d54a9ef957f
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2017
---
# <a name="create-a-hello-world-web-app-for-azure-using-intellij"></a><span data-ttu-id="b6550-103">使用 IntelliJ 建立 Azure 的 Hello World Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b6550-103">Create a Hello World web app for Azure using IntelliJ</span></span>

<span data-ttu-id="b6550-104">本教學課程示範如何使用 [Azure Toolkit for IntelliJ (適用於 IntelliJ 的 Azure 工具組)]，建立基本的 Hello World 應用程式，並做為 Web 應用程式部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="b6550-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for IntelliJ].</span></span>

> [!NOTE]
>
> <span data-ttu-id="b6550-105">如需使用[適用於 Eclipse 的 Azure 工具組]的此文章版本，請參閱[使用 Eclipse 建立 Azure 的 Hello World Web 應用程式][eclipse-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="b6550-105">For a version of this article that uses the [Azure Toolkit for Eclipse], see [Create a Hello World web app for Azure using Eclipse][eclipse-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="b6550-106">適用於 IntelliJ 的 Azure 工具組在 2017 年 8 月更新，有不同的工作流程。</span><span class="sxs-lookup"><span data-stu-id="b6550-106">The Azure Toolkit for IntelliJ was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="b6550-107">本文說明使用適用於 IntelliJ 的 Azure 工具組版本 3.0.7 (或更新版本) 來建立 Hello World Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b6550-107">This article illustrates creating a Hello World web app by using version 3.0.7 (or later) of the Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="b6550-108">如果您使用 3.0.6 版 (或舊版) 工具組，您必須依照[使用舊版工具組在 IntelliJ 中建立 Azure 的 Hello World Web 應用程式][Legacy Version]中的步驟。</span><span class="sxs-lookup"><span data-stu-id="b6550-108">If you are using the version 3.0.6 (or earlier) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in IntelliJ using the legacy toolkit][Legacy Version].</span></span>
> 

<span data-ttu-id="b6550-109">當您完成本教學課程，在網頁瀏覽器中檢視您的應用程式時，看起來會如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="b6550-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World 應用程式預覽][browse-web-app]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="b6550-111">建立新的 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="b6550-111">Create a new web app project</span></span>

1. <span data-ttu-id="b6550-112">使用[適用於 IntelliJ 的 Azure 工具組的 Azure 登入指示][intelliJ-sign-in-instructions]文章中的指示，來啟動 IntelliJ 並登入您的 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="b6550-112">Start IntelliJ, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for IntelliJ][intelliJ-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="b6550-113">按一下 [檔案] 功能表，按一下 [新增]，然後按一下 [專案]。</span><span class="sxs-lookup"><span data-stu-id="b6550-113">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![建立新專案][file-new-project]

1. <span data-ttu-id="b6550-115">在 **新增專案** 對話方塊中，選取 **Maven** ，然後選取 **maven-archetype-webapp** ，再按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b6550-115">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![選擇 Maven archetype webapp][maven-archetype-webapp]
   
1. <span data-ttu-id="b6550-117">指定 Web 應用程式的 [GroupId] 和 [ArtifactId]，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b6550-117">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="b6550-119">自訂任何 Maven 設定或接受預設值，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b6550-119">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![指定 Maven 設定][maven-options]

1. <span data-ttu-id="b6550-121">指定專案名稱和位置，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="b6550-121">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定專案名稱][project-name]

1. <span data-ttu-id="b6550-123">在 IntelliJ 的 [專案總管] 檢視中，依序展開 [src]、[main] 和 [webapp]，然後按兩下 [index.jsp]。</span><span class="sxs-lookup"><span data-stu-id="b6550-123">Within IntelliJ's Project Explorer view, expand **src**, then **main**, then **webapp**, and then double-click **index.jsp**.</span></span>
   
   ![開啟索引頁面][open-index-page]

1. <span data-ttu-id="b6550-125">當 index.jsp 檔案在 IntelliJ 中開啟時，加入文字以動態顯示 **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="b6550-125">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="b6550-126">(在現有 `<body>` 元素內)。</span><span class="sxs-lookup"><span data-stu-id="b6550-126">within the existing `<body>` element.</span></span> <span data-ttu-id="b6550-127">您已更新的 `<body>` 內容看起來應該與下列範例類似：</span><span class="sxs-lookup"><span data-stu-id="b6550-127">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![編輯索引頁面][edit-index-page]

1. <span data-ttu-id="b6550-129">儲存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="b6550-129">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="b6550-130">將 Web 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="b6550-130">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="b6550-131">在 IntelliJ 的 [專案總管] 畫面，以滑鼠右鍵按一下您的專案，並選擇 [Azure]，然後選擇 [在 Web 應用程式上執行]。</span><span class="sxs-lookup"><span data-stu-id="b6550-131">Within IntelliJ's Project Explorer view, right-click your project, choose **Azure**, and then choose **Run on Web App**.</span></span>
   
   ![[在 Web 應用程式上執行] 功能表][run-on-web-app-menu]

1. <span data-ttu-id="b6550-133">在 [在 Web 應用程式上執行] 對話方塊中，可以選擇下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="b6550-133">In the Run on Web App dialog box, you can choose one of the following options:</span></span>

   * <span data-ttu-id="b6550-134">請選擇現有 Web 應用程式 (如果有)，然後按一下 [執行]。</span><span class="sxs-lookup"><span data-stu-id="b6550-134">Choose an existing web app (if one exists), and then click **Run**.</span></span>

      ![[在 Web 應用程式上執行] 對話方塊][run-on-web-app-dialog]

   * <span data-ttu-id="b6550-136">按一下 [建立新的 Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="b6550-136">Click **Create New Web App**.</span></span> <span data-ttu-id="b6550-137">如果您選擇建立新的 Web 應用程式，請指定 Web 應用程式的必要資訊，然後按一下 [執行]。</span><span class="sxs-lookup"><span data-stu-id="b6550-137">If you choose to create a new web app, specify the requisite information for your web app, and then click **Run**.</span></span>

      ![建立新的 Web 應用程式][create-new-web-app-dialog]

1. <span data-ttu-id="b6550-139">工具組成功部署 Web 應用程式時，會顯示狀態訊息，也會顯示已部署的 Web 應用程式本身的 URL。</span><span class="sxs-lookup"><span data-stu-id="b6550-139">The toolkit will display a status message when it has successfully deployed your web app, which will also display the URL of your deployed web app.</span></span>

   ![部署成功][successfully-deployed]

1. <span data-ttu-id="b6550-141">您可以使用狀態訊息中提供的連結，瀏覽至您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b6550-141">You can browse to your web app using the link provided in the status message.</span></span>

   ![瀏覽 Web 應用程式][browse-web-app]

1. <span data-ttu-id="b6550-143">發佈 Web 應用程式之後，系統會將您的設定儲存為預設值，您可以按一下工具列上的綠色箭號圖示，在 Azure 上執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b6550-143">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="b6550-144">您可以透過按一下 Web 應用程式的下拉式功能表，然後按一下 [編輯組態]，修改您的設定。</span><span class="sxs-lookup"><span data-stu-id="b6550-144">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![[編輯組態] 功能表][edit-configuration-menu]

1. <span data-ttu-id="b6550-146">顯示 [執行/偵錯組態] 對話方塊時，您可以修改任何預設設定，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b6550-146">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![[編輯組態] 對話方塊][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="b6550-148">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b6550-148">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="b6550-149">如需建立 Azure Web Apps 的詳細資訊，請參閱 [Web 應用程式概觀]。</span><span class="sxs-lookup"><span data-stu-id="b6550-149">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for IntelliJ (適用於 IntelliJ 的 Azure 工具組)]: azure-toolkit-for-intellij.md
[適用於 Eclipse 的 Azure 工具組]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web 應用程式概觀]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[run-on-web-app-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[run-on-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
