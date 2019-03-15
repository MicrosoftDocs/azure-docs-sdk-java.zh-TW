---
title: 使用 Eclipse 建立 Azure 的 Hello World Web 應用程式
description: 本教學課程將示範如何使用適用於 Eclipse 的 Azure 工具組來建立 Azure Hello World Web 應用程式。
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: c98f966eb17e3fbde877451c8f8fefb21e6bf686
ms.sourcegitcommit: dca98b953fa3149fb2e6aa49e27e843b6df0c6c2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2019
ms.locfileid: "57786887"
---
# <a name="create-a-hello-world-web-app-for-azure-using-eclipse"></a><span data-ttu-id="4136b-103">使用 Eclipse 建立 Azure 的 Hello World Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="4136b-103">Create a Hello World web app for Azure using Eclipse</span></span>

<span data-ttu-id="4136b-104">本教學課程示範如何使用 [適用於 Eclipse 的 Azure 工具組]，建立基本的 Hello World 應用程式，並部署到 Azure 作為 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4136b-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for Eclipse].</span></span>

> [!NOTE]
>
> <span data-ttu-id="4136b-105">如需使用[適用於 IntelliJ 的 Azure 工具組]的此文章版本，請參閱[使用 IntelliJ 建立 Azure 的 Hello World Web 應用程式][intellij-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="4136b-105">For a version of this article that uses the [Azure Toolkit for IntelliJ], see [Create a Hello World web app for Azure using IntelliJ][intellij-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="4136b-106">適用於 Eclipse 的 Azure 工具組在 2017 年 8 月更新，有不同的工作流程。</span><span class="sxs-lookup"><span data-stu-id="4136b-106">The Azure Toolkit for Eclipse was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="4136b-107">本文說明使用適用於 Eclipse 的 Azure 工具組版本 3.0.7 (或更新版本) 來建立 Hello World Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4136b-107">This article illustrates creating a Hello World web app by using version 3.0.7 (or later) of the Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="4136b-108">如果您使用 3.0.6 版 (或舊版) 工具組，您必須依照[使用舊版工具組在 Eclipse 中建立 Azure 的 Hello World Web 應用程式][Legacy Version]中的步驟。</span><span class="sxs-lookup"><span data-stu-id="4136b-108">If you are using the version 3.0.6 (or earlier) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in Eclipse using the legacy toolkit][Legacy Version].</span></span>
> 

<span data-ttu-id="4136b-109">當您完成本教學課程，在網頁瀏覽器中檢視您的應用程式時，看起來會如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="4136b-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World 應用程式預覽][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="4136b-111">建立新的 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="4136b-111">Create a new web app project</span></span>

1. <span data-ttu-id="4136b-112">啟動 Eclipse 並登入您的 Azure 帳戶，方法是使用 [適用於 Eclipse 的 Azure 工具組的 Azure 登入指示][eclipse-sign-in-instructions] 文章中的指示。</span><span class="sxs-lookup"><span data-stu-id="4136b-112">Start Eclipse, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for Eclipse][eclipse-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="4136b-113">依序按一下 [檔案]、[新增] 和 [動態 Web 專案]。</span><span class="sxs-lookup"><span data-stu-id="4136b-113">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="4136b-114">(如果在按一下 [檔案] 和 [新增] 後沒有看到 [動態 Web 專案] 列為可用的專案，請執行下列動作：依序按一下 [檔案]、[新增]、[專案...]，展開 [Web]，按一下 [動態 Web 專案]，然後按 [下一步]。)</span><span class="sxs-lookup"><span data-stu-id="4136b-114">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

   ![建立新的動態 Web 專案][file-new-dynamic-web-project]

2. <span data-ttu-id="4136b-116">基於本教學課程的目的，將專案命名為 **MyWebApp**。</span><span class="sxs-lookup"><span data-stu-id="4136b-116">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="4136b-117">您的畫面將出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4136b-117">Your screen will appear similar to the following:</span></span>
   
   ![新增動態 Web 專案屬性][dynamic-web-project-properties]

3. <span data-ttu-id="4136b-119">按一下 [完成] 。</span><span class="sxs-lookup"><span data-stu-id="4136b-119">Click **Finish**.</span></span>

4. <span data-ttu-id="4136b-120">在 Eclipse 的 [專案總管] 檢視中，展開 [MyWebApp]。</span><span class="sxs-lookup"><span data-stu-id="4136b-120">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="4136b-121">在 [WebContent] 上按一下滑鼠右鍵、按一下 [新增]，然後按一下 [JSP 檔案]。</span><span class="sxs-lookup"><span data-stu-id="4136b-121">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

   ![建立新的 JSP 檔案][create-new-jsp-file]

5. <span data-ttu-id="4136b-123">在 [新增 JSP 檔案] 對話方塊中，將檔案命名為 **index.jsp**、將父資料夾保留為 **MyWebApp/WebContent**，然後按 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="4136b-123">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

   ![[新增 JSP 檔案] 對話方塊][new-jsp-file-dialog]

6. <span data-ttu-id="4136b-125">在 [選取 JSP 範本] 對話方塊中，基於本教學課程的目的，選取 [新增 JSP 檔案 (html)]，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="4136b-125">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

   ![選取 JSP 範本][select-jsp-template]

7. <span data-ttu-id="4136b-127">當 index.jsp 檔案在 Eclipse 中開啟時，新增文字以動態顯示 **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="4136b-127">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="4136b-128">(在現有的 `<body>` 元素內加入)。</span><span class="sxs-lookup"><span data-stu-id="4136b-128">within the existing `<body>` element.</span></span> <span data-ttu-id="4136b-129">您已更新的 `<body>` 內容看起來應該與下列範例類似：</span><span class="sxs-lookup"><span data-stu-id="4136b-129">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="4136b-130">儲存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="4136b-130">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="4136b-131">將 Web 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="4136b-131">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="4136b-132">在 Eclipse 的 [專案總管] 畫面，以滑鼠右鍵按一下您的專案，並選擇 [Azure]，然後選擇 [發行至 Azure Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="4136b-132">Within Eclipse's Project Explorer view, right-click your project, choose **Azure**, and then choose **Publish as Azure Web App**.</span></span>
   
   ![發佈為 Azure Web 應用程式][publish-as-azure-web-app]

1. <span data-ttu-id="4136b-134">當 [部署 Web 應用程式] 對話方塊出現時，您可以選擇下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="4136b-134">When the **Deploy Web App** dialog box appears, you can choose one of the following options:</span></span>

   * <span data-ttu-id="4136b-135">如果有的話，請選取現有的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4136b-135">Select an existing web app if one exists.</span></span>

      ![選取 App Service][select-app-service]

   * <span data-ttu-id="4136b-137">按一下 [建立新的 Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="4136b-137">Click **Create New Web App**.</span></span>

      ![建立應用程式服務][create-app-service]

      <span data-ttu-id="4136b-139">在 [建立 App Service] 對話方塊中指定 Web 應用程式的必要資訊，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="4136b-139">Specify the requisite information for your web app in the **Create App Service** dialog box, and then click **Create**.</span></span>

      <span data-ttu-id="4136b-140">您可以在此設定執行階段環境、應用程式設定、服務方案以及資源群組。</span><span class="sxs-lookup"><span data-stu-id="4136b-140">Here you can configure the runtime environment, app settings, service plan and resource group.</span></span>

      ![建立 App Service 對話方塊][create-app-service-dialog]

1. <span data-ttu-id="4136b-142">選取您的 Web 應用程式，然後按一下 [部署]。</span><span class="sxs-lookup"><span data-stu-id="4136b-142">Select your web app and then click **Deploy**.</span></span>

   ![部署 App Service][deploy-app-service]

1. <span data-ttu-id="4136b-144">當系統成功地部署您的 Web 應用程式之後，工具組會在 **Azure 活動記錄** 標籤下顯示**已發佈**狀態，並提供已部署 Web 應用程式的超連結。</span><span class="sxs-lookup"><span data-stu-id="4136b-144">The toolkit will display a **Published** status under the **Azure Activity Log** tab when it has successfully deployed your web app, which is a hyperlink for the URL of your deployed web app.</span></span>

   ![發佈狀態][publish-status]

1. <span data-ttu-id="4136b-146">您可以使用狀態訊息中提供的連結，瀏覽至您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4136b-146">You can browse to your web app using the link provided in the status message.</span></span>

   ![瀏覽 Web 應用程式][browse-web-app]

1. <span data-ttu-id="4136b-148">您的 Web 發佈至 Azure 之後，您可以在其上按一下滑鼠右鍵並選取內容功能表中的其中一個選項，來管理您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4136b-148">After you have published your web to Azure, you can manage your app by right-clicking on it and selecting one of the options on the context menu.</span></span> <span data-ttu-id="4136b-149">例如，您可以**啟動**、**停止**或**刪除** Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4136b-149">For example, you can **Start**, **Stop**, or **Delete** your web app.</span></span>

   ![管理 App Service][manage-app-service]

## <a name="next-steps"></a><span data-ttu-id="4136b-151">後續步驟</span><span class="sxs-lookup"><span data-stu-id="4136b-151">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="4136b-152">如需建立 Azure Web Apps 的詳細資訊，請參閱 [Web 應用程式概觀]。</span><span class="sxs-lookup"><span data-stu-id="4136b-152">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[適用於 Eclipse 的 Azure 工具組]: azure-toolkit-for-eclipse.md
[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[適用於 IntelliJ 的 Azure 工具組]: ../intellij/azure-toolkit-for-intellij.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Web 應用程式概觀]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version.md

<!-- IMG List -->

[browse-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/browse-web-app.png
[file-new-dynamic-web-project]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/file-new-dynamic-web-project.png
[dynamic-web-project-properties]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/dynamic-web-project-properties.png
[create-new-jsp-file]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-new-jsp-file.png
[new-jsp-file-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/new-jsp-file-dialog.png
[select-jsp-template]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-jsp-template.png
[publish-as-azure-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-as-azure-web-app.png
[deploy-web-app-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-web-app-dialog.png
[select-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-app-service.png
[create-app-service-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service-dialog.png
[publish-status]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-status.png
[create-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service.png
[deploy-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-app-service.png
[manage-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/manage-app-service.png
