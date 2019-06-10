---
title: 使用 IntelliJ 建立 Azure App Service 的 Hello World Web 應用程式
description: 本教學課程將示範如何使用適用於 IntelliJ 的 Azure 工具組來建立 Azure 的 Hello World Web 應用程式。
services: app-service
keywords: java, IntelliJ, web 應用程式, azure app service, hello world, 快速入門
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: ae0749ce1ddab971f1a83e2e5e58492fd8ccb287
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65626108"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a><span data-ttu-id="ab0a9-104">使用 IntelliJ 建立 Azure App Service 的 Hello World Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="ab0a9-104">Create a Hello World web app for Azure App Service using IntelliJ</span></span>

<span data-ttu-id="ab0a9-105">使用開放原始碼的[適用於 IntelliJ 的 Azure 工具組](https://plugins.jetbrains.com/plugin/8053)外掛程式建立基本的 Hello World 應用程式，並將其部署至 Azure App Service 作為 Web 應用程式，只需短短幾分鐘即可完成。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-105">Using open sourced [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) plugin, creating and deploying a basic Hello World application to Azure App Service as a web app can be done in a few minutes.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ab0a9-106">如果您偏好使用 Eclipse，請參閱我們[適用於 Eclipse 的類似教學課程][eclipse-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-106">If you prefer using Eclipse, check out our [similar tutorial for Eclipse][eclipse-hello-world].</span></span>
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> <span data-ttu-id="ab0a9-107">在完成本教學課程之後，別忘了清除資源。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-107">Don't forget to clean up the resources after you complete this tutorial.</span></span> <span data-ttu-id="ab0a9-108">如此，執行此指南時就不會超過您的免費帳戶配額。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-108">In that case, running this guide will not exceed your free account quota.</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a><span data-ttu-id="ab0a9-109">安裝和登入</span><span class="sxs-lookup"><span data-stu-id="ab0a9-109">Installation and Sign-in</span></span>

1. <span data-ttu-id="ab0a9-110">在 IntelliJ IDEA 的 [設定/喜好設定] 對話方塊 (Ctrl+Alt+S) 中，選取 [外掛程式]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-110">In IntelliJ IDEA's Settings/Preferences dialog (Ctrl+Alt+S), select **Plugins**.</span></span> <span data-ttu-id="ab0a9-111">然後，在 **Marketplace** 中尋找**適用於 IntelliJ 的 Azure 工具組**，然後按一下 [安裝]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-111">Then, find the **Azure Toolkit for IntelliJ** in the **Marketplace** and click **Install**.</span></span> <span data-ttu-id="ab0a9-112">安裝之後，按一下 [重新啟動]  以啟動此外掛程式。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-112">After installed, click **Restart** to activate the plugin.</span></span> 

   ![Marketplace 中適用於 IntelliJ 的 Azure 工具組外掛程式][marketplace]

2. <span data-ttu-id="ab0a9-114">若要登入您的 Azure 帳戶，請開啟 **Azure 總管**資訊看板，然後按一下頂端列中的 [Azure 登入]  圖示 (或從 IDEA 功能表**工具/Azure/Azure 登入**點選)。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-114">To sign in to your Azure account, open sidebar **Azure Explorer**, and then click the **Azure Sign In** icon in the bar on top (or from IDEA menu **Tools/Azure/Azure Sign in**).</span></span>

   ![IntelliJ 的 [Azure 登入] 命令][I01]

3. <span data-ttu-id="ab0a9-116">在 [Azure 登入]  視窗中選取 [裝置登入]  ，然後按一下 [登入]  ([其他登入選項](azure-toolkit-for-intellij-sign-in-instructions.md))。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-116">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in** ([other sign in options](azure-toolkit-for-intellij-sign-in-instructions.md)).</span></span>

   ![選取了 [裝置登入] 的 [Azure 登入] 視窗][I02]

4. <span data-ttu-id="ab0a9-118">按一下 [Azure 裝置登入]  對話方塊中的 [複製並開啟]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-118">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![[Azure 登入] 對話方塊視窗][I03]

5. <span data-ttu-id="ab0a9-120">在瀏覽器中，貼上您的裝置程式碼 (您在上一個步驟中按一下 [複製並開啟]  時所複製)，然後按 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-120">In the browser, paste your device code (which has been copied when you click **Copy&Open** in last step) and then click **Next**.</span></span>

   ![裝置登入瀏覽器][I04]

6. <span data-ttu-id="ab0a9-122">在 [選取訂用帳戶]  對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-122">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][I05]

## <a name="creating-web-app-project"></a><span data-ttu-id="ab0a9-124">建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="ab0a9-124">Creating web app project</span></span>

1. <span data-ttu-id="ab0a9-125">在 IntelliJ 中，依序按一下 [檔案]  功能表、[新增]  和 [專案]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-125">In IntelliJ, click the **File** menu, then click **New**, and then click **Project**.</span></span>

   ![建立新專案][file-new-project]

2. <span data-ttu-id="ab0a9-127">在 **新增專案** 對話方塊中，選取 **Maven** ，然後選取 **maven-archetype-webapp** ，再按一下 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-127">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>

   ![選擇 Maven archetype Webapp][maven-archetype-webapp]

3. <span data-ttu-id="ab0a9-129">指定 Web 應用程式的 [GroupId]  和 [ArtifactId]  ，然後按一下 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-129">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>

   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

4. <span data-ttu-id="ab0a9-131">自訂任何 Maven 設定或接受預設值，然後按一下 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-131">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>

   ![指定 Maven 設定][maven-options]

5. <span data-ttu-id="ab0a9-133">指定專案名稱和位置，然後按一下 [完成]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-133">Specify your project name and location, and then click **Finish**.</span></span>

   ![指定專案名稱][project-name]

6. <span data-ttu-id="ab0a9-135">在 [專案總管] 檢視下開啟檔案 **src/main/webapp/index.jsp**，並依照下列方式加以編輯，然後**儲存變更**：</span><span class="sxs-lookup"><span data-stu-id="ab0a9-135">Under Project Explorer view, open and edit the file **src/main/webapp/index.jsp** as following and **save the changes**:</span></span>

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```

   ![編輯索引頁面][edit-index-page]

## <a name="deploying-web-app-to-azure"></a><span data-ttu-id="ab0a9-137">將 Web 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="ab0a9-137">Deploying web app to Azure</span></span>

1. <span data-ttu-id="ab0a9-138">在 [專案總管] 檢視中，以滑鼠右鍵按一下您的專案，並展開 **Azure**，然後按一下 [部署至 Azure]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-138">Under Project Explorer view, right-click your project, expand **Azure**, then click **Deploy to Azure**.</span></span>

   ![部署至 Azure 功能表][deploy-to-azure-menu]

1. <span data-ttu-id="ab0a9-140">在 [部署至 Azure] 對話方塊中，您可以直接將應用程式部署至現有的 Tomcat WebApp (如果已經有的話)，否則您應先新建一個。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-140">In the Deploy to Azure dialog box, you can directly deploy the application to an existing Tomcat webapp if you already have one, otherwise you should create a new one first.</span></span>
   1. <span data-ttu-id="ab0a9-141">按一下 [沒有可用的 WebApp，請按一下以建立一個新的]  連結以建立新的 Web 應用程式，如果您的訂用帳戶中有現有的 Web 應用程式，則可以從 WebApp 下拉式清單中選擇 [建立新的 WebApp]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-141">Click the link **No Available webapp, click to create a new one** to crete a new web app, you could choose **Create New WebApp** from WebApp dropdown if there are existing webapps in your subscription.</span></span>

      ![[部署至 Azure] 對話方塊][deploy-to-azure-dialog]

   1. <span data-ttu-id="ab0a9-143">在快顯對話方塊中，選擇 **TOMCAT 8.5-jre8** 作為 Web 容器，並指定其他必要的資訊，然後按一下 [確定]  以建立 WebApp。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-143">In the pop-up dialog box, chose **TOMCAT 8.5-jre8** as Web Container and specify other required information, then click **OK** to create the webapp.</span></span>

      ![建立新的 Web 應用程式][create-new-web-app-dialog]

   1. <span data-ttu-id="ab0a9-145">從 WebApp 下拉式清單中選擇 Web 應用程式，然後按一下 [執行]  。(如果您想要部署至現有的 WebApp，可以從這裡開始)</span><span class="sxs-lookup"><span data-stu-id="ab0a9-145">Choose the web app from WebApp drop down, and then click **Run**.(You could start from here if you want deploy to an existing webapp)</span></span>

      ![部署至現有的 WebApp][deploy-to-existing-webapp]

1. <span data-ttu-id="ab0a9-147">工具組成功部署 Web 應用程式時會顯示狀態訊息，以及已部署的 Web 應用程式本身的 URL。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-147">The toolkit will display a status message when it has successfully deployed your web app, along with the URL of your deployed web app if succeed.</span></span>

   ![部署成功][successfully-deployed]

1. <span data-ttu-id="ab0a9-149">您可以使用狀態訊息中提供的連結，瀏覽至您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-149">You can browse to your web app using the link provided in the status message.</span></span>

   ![瀏覽 Web 應用程式][browse-web-app]

## <a name="managing-deploy-configurations"></a><span data-ttu-id="ab0a9-151">管理部署組態</span><span class="sxs-lookup"><span data-stu-id="ab0a9-151">Managing deploy configurations</span></span>

1. <span data-ttu-id="ab0a9-152">發佈 Web 應用程式之後，系統會將您的設定儲存為預設值，您可以按一下工具列上的綠色箭號圖示以執行部署。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-152">After you have published your web app, your settings will be saved as the default, and you can run the deployment by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="ab0a9-153">您可以透過按一下 Web 應用程式的下拉式功能表，然後按一下 [編輯組態]  ，修改您的設定。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-153">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![[編輯組態] 功能表][edit-configuration-menu]

1. <span data-ttu-id="ab0a9-155">顯示 [執行/偵錯組態]  對話方塊時，您可以修改任何預設設定，然後按一下 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-155">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![[編輯組態] 對話方塊][edit-configuration-dialog]

## <a name="cleaning-up-resources"></a><span data-ttu-id="ab0a9-157">清除資源</span><span class="sxs-lookup"><span data-stu-id="ab0a9-157">Cleaning up resources</span></span>

1. <span data-ttu-id="ab0a9-158">在 Azure 總管中刪除 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="ab0a9-158">Deleting Web Apps in Azure Explorer</span></span>

     ![清除資源][clean-resources]

## <a name="next-steps"></a><span data-ttu-id="ab0a9-160">後續步驟</span><span class="sxs-lookup"><span data-stu-id="ab0a9-160">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="ab0a9-161">如需建立 Azure Web Apps 的詳細資訊，請參閱 [Web 應用程式概觀]。</span><span class="sxs-lookup"><span data-stu-id="ab0a9-161">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web 應用程式概觀]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->
[marketplace]:./media/azure-toolkit-for-intellij-create-hello-world-web-app/marketplace.png
[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[deploy-to-azure-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[deploy-to-azure-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[deploy-to-existing-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/deploy-to-existing-webapp.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
[clean-resources]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/clean-resource.png
[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png
