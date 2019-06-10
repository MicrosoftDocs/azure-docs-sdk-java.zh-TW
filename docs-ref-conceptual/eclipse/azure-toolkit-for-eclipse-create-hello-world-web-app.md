---
title: 使用 Eclipse 建立 Azure App Service 的 Hello World Web 應用程式
description: 本教學課程將示範如何使用適用於 Eclipse 的 Azure 工具組來建立 Azure Hello World Web 應用程式。
services: app-service
keywords: java, eclipse, web 應用程式, azure app service, hello world, 快速入門
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
ms.openlocfilehash: 7e88298afaf0b4601d85d6063b7096c79e677421
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625909"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a><span data-ttu-id="6b2bb-104">使用 Eclipse 建立 Azure App Service 的 Hello World Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="6b2bb-104">Create a Hello World web app for Azure App Service using Eclipse</span></span>

<span data-ttu-id="6b2bb-105">使用開放原始碼的[適用於 Eclipse 的 Azure 工具組](https://marketplace.eclipse.org/content/azure-toolkit-eclipse)外掛程式建立基本的 Hello World 應用程式，並將其部署至 Azure App Service 作為 Web 應用程式，只需短短幾分鐘即可完成。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-105">Using open sourced [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) plugin, creating and deploying a basic Hello World application to Azure App Service as a web app can be done in a few minutes.</span></span>

> [!NOTE]
>
> <span data-ttu-id="6b2bb-106">如果您偏好使用 IntelliJ IDEA，請參閱我們[適用於 IntelliJ 的類似教學課程][intellij-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-106">If you prefer using IntelliJ IDEA, check out our [similar tutorial for IntelliJ][intellij-hello-world].</span></span>
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> <span data-ttu-id="6b2bb-107">在完成本教學課程之後，別忘了清除資源。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-107">Don't forget to clean up the resources after you complete this tutorial.</span></span> <span data-ttu-id="6b2bb-108">如此，執行此指南時就不會超過您的免費帳戶配額。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-108">In that case, running this guide will not exceed your free account quota.</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a><span data-ttu-id="6b2bb-109">安裝和登入</span><span class="sxs-lookup"><span data-stu-id="6b2bb-109">Installation and sign-in</span></span>

1. <span data-ttu-id="6b2bb-110">將下列按鈕拖曳到您執行中的 Eclipse 工作區，以安裝「適用於 Eclipse 的 Azure 工具組」外掛程式 ([其他安裝選項](azure-toolkit-for-eclipse-installation.md))。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-110">Drag the following button to your running Eclipse workspace to install the Azure Toolkit for Eclipse plugin ([other installation options](azure-toolkit-for-eclipse-installation.md)).</span></span>

    <span data-ttu-id="6b2bb-111">[![拖曳到您執行中的 Eclipse* 工作區。*需要 Eclipse Marketplace 用戶端](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "拖曳到您執行中的 Eclipse* 工作區。*需要 Eclipse Marketplace 用戶端")</span><span class="sxs-lookup"><span data-stu-id="6b2bb-111">[![Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client")</span></span>

1. <span data-ttu-id="6b2bb-112">若要登入您的 Azure 帳戶，請依序按一下 [工具]  、[Azure]  以及 [登入]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-112">To sign in to your Azure account, click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="6b2bb-113">![Azure 登入的 Eclipse 功能表][I01]</span><span class="sxs-lookup"><span data-stu-id="6b2bb-113">![Eclipse Menu for Azure Sign In][I01]</span></span>

1. <span data-ttu-id="6b2bb-114">在 [Azure 登入]  視窗中選取 [裝置登入]  ，然後按一下 [登入]  ([其他登入選項](azure-toolkit-for-eclipse-sign-in-instructions.md))。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-114">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in** ([other sign-in options](azure-toolkit-for-eclipse-sign-in-instructions.md)).</span></span>

   ![選取了 [裝置登入] 的 [Azure 登入] 視窗][I02]

1. <span data-ttu-id="6b2bb-116">按一下 [Azure 裝置登入]  對話方塊中的 [複製並開啟]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-116">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![[Azure 登入] 對話方塊視窗][I03]

1. <span data-ttu-id="6b2bb-118">在瀏覽器中，貼上您的裝置程式碼 (您在上一個步驟中按一下 [複製並開啟]  時所複製)，然後按 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-118">In the browser, paste your device code (which has been copied when you clicked **Copy&Open** in last step) and then click **Next**.</span></span>

   ![裝置登入瀏覽器][I04]

1. <span data-ttu-id="6b2bb-120">最後，在 [選取訂用帳戶]  對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-120">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][I05]

## <a name="creating-web-app-project"></a><span data-ttu-id="6b2bb-122">建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="6b2bb-122">Creating web app project</span></span>

1. <span data-ttu-id="6b2bb-123">依序按一下 [檔案]  、[新增]  和 [動態 Web 專案]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-123">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="6b2bb-124">(如果在按一下 [檔案]  和 [新增]  後沒有看到 [動態 Web 專案]  列為可用的專案，請執行下列動作：依序按一下 [檔案]  、[新增]  、[專案...]  ，展開 [Web]  ，按一下 [動態 Web 專案]  ，然後按 [下一步]  。)</span><span class="sxs-lookup"><span data-stu-id="6b2bb-124">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

   ![建立新的動態 Web 專案][file-new-dynamic-web-project]

2. <span data-ttu-id="6b2bb-126">基於本教學課程的目的，將專案命名為 **MyWebApp**。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-126">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="6b2bb-127">您的畫面將出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6b2bb-127">Your screen will appear similar to the following:</span></span>
   
   ![新增動態 Web 專案屬性][dynamic-web-project-properties]

3. <span data-ttu-id="6b2bb-129">按一下 [完成]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-129">Click **Finish**.</span></span>

4. <span data-ttu-id="6b2bb-130">在 Eclipse 的 [專案總管]  檢視中，展開 [MyWebApp]。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-130">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="6b2bb-131">在 [WebContent]  上按一下滑鼠右鍵、按一下 [新增]  ，然後按一下 [JSP 檔案]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-131">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

   ![建立新的 JSP 檔案][create-new-jsp-file]

5. <span data-ttu-id="6b2bb-133">在 [新增 JSP 檔案]  對話方塊中，將檔案命名為 **index.jsp**、將父資料夾保留為 **MyWebApp/WebContent**，然後按 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-133">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

   ![[新增 JSP 檔案] 對話方塊][new-jsp-file-dialog]

6. <span data-ttu-id="6b2bb-135">在 [選取 JSP 範本]  對話方塊中，基於本教學課程的目的，選取 [新增 JSP 檔案 (html)]  ，然後按一下 [完成]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-135">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

   ![選取 JSP 範本][select-jsp-template]

7. <span data-ttu-id="6b2bb-137">當 index.jsp 檔案在 Eclipse 中開啟時，新增文字以動態顯示 **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="6b2bb-137">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="6b2bb-138">(在現有的 `<body>` 元素內加入)。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-138">within the existing `<body>` element.</span></span> <span data-ttu-id="6b2bb-139">您已更新的 `<body>` 內容看起來應該與下列範例類似：</span><span class="sxs-lookup"><span data-stu-id="6b2bb-139">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="6b2bb-140">儲存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-140">Save index.jsp.</span></span>

## <a name="deploying-web-app-to-azure"></a><span data-ttu-id="6b2bb-141">將 Web 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="6b2bb-141">Deploying web app to Azure</span></span>

1. <span data-ttu-id="6b2bb-142">在 Eclipse 的 [專案總管] 畫面，以滑鼠右鍵按一下您的專案，並選擇 [Azure]  ，然後選擇 [發行至 Azure Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-142">Within Eclipse's Project Explorer view, right-click your project, choose **Azure**, and then choose **Publish as Azure Web App**.</span></span>
   
   ![發佈為 Azure Web 應用程式][publish-as-azure-web-app]

1. <span data-ttu-id="6b2bb-144">當 [部署 Web 應用程式]  對話方塊出現時，您可以選擇下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="6b2bb-144">When the **Deploy Web App** dialog box appears, you can choose one of the following options:</span></span>

   * <span data-ttu-id="6b2bb-145">如果有的話，請選取現有的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-145">Select an existing web app if one exists.</span></span>

      ![選取 App Service][select-app-service]

   * <span data-ttu-id="6b2bb-147">按一下 [建立新的 Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-147">Click **Create New Web App**.</span></span>

      ![建立應用程式服務][create-app-service]

      <span data-ttu-id="6b2bb-149">在 [建立 App Service]  對話方塊中指定 Web 應用程式的必要資訊，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-149">Specify the requisite information for your web app in the **Create App Service** dialog box, and then click **Create**.</span></span>

      <span data-ttu-id="6b2bb-150">您可以在此設定執行階段環境、應用程式設定、服務方案以及資源群組。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-150">Here you can configure the runtime environment, app settings, service plan and resource group.</span></span>

      ![建立 App Service 對話方塊][create-app-service-dialog]

1. <span data-ttu-id="6b2bb-152">選取您的 Web 應用程式，然後按一下 [部署]  。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-152">Select your web app and then click **Deploy**.</span></span>

   ![部署 App Service][deploy-app-service]

1. <span data-ttu-id="6b2bb-154">當系統成功地部署您的 Web 應用程式之後，工具組會在 **Azure 活動記錄** 標籤下顯示**已發佈**狀態，並提供已部署 Web 應用程式的超連結。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-154">The toolkit will display a **Published** status under the **Azure Activity Log** tab when it has successfully deployed your web app, which is a hyperlink for the URL of your deployed web app.</span></span>

   ![發佈狀態][publish-status]

1. <span data-ttu-id="6b2bb-156">您可以使用狀態訊息中提供的連結，瀏覽至您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-156">You can browse to your web app using the link provided in the status message.</span></span>

   ![瀏覽 Web 應用程式][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="cleaning-up-resources"></a><span data-ttu-id="6b2bb-158">清除資源</span><span class="sxs-lookup"><span data-stu-id="6b2bb-158">Cleaning up resources</span></span>

1. <span data-ttu-id="6b2bb-159">您的 Web 應用程式發佈至 Azure 之後，您可以在 Azure 總管中按一下滑鼠右鍵並選取操作功能表中的一個選項，來管理您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-159">After you have published your web app to Azure, you can manage it by right-clicking in Azure Explorer and selecting one of the options in the context menu.</span></span> <span data-ttu-id="6b2bb-160">例如，您可以在此處**刪除** Web 應用程式，以清除本教學課程的資源。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-160">For example, you can **Delete** your web app here to clean up the resource for this tutorial.</span></span>

   ![管理 App Service][manage-app-service]

## <a name="next-steps"></a><span data-ttu-id="6b2bb-162">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6b2bb-162">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="6b2bb-163">如需建立 Azure Web Apps 的詳細資訊，請參閱 [Web 應用程式概觀]。</span><span class="sxs-lookup"><span data-stu-id="6b2bb-163">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Web 應用程式概觀]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version.md

<!-- IMG List -->
[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

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
