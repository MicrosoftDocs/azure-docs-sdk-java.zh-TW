---
title: "在 IntelliJ 中建立基本的 Azure Web 應用程式"
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
ms.date: 10/19/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: 323ce74f2d4dad10460a0c2bc0fb59f2bbdbbf3c
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a><span data-ttu-id="b7e0b-103">在 IntelliJ 中建立基本的 Azure Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b7e0b-103">Create a basic Azure web app in IntelliJ</span></span>

<span data-ttu-id="b7e0b-104">本教學課程示範如何使用 [Azure Toolkit for IntelliJ (適用於 IntelliJ 的 Azure 工具組)]，建立基本的 Hello World 應用程式，並做為 Web 應用程式部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for IntelliJ].</span></span>

> [!NOTE]
> 
> <span data-ttu-id="b7e0b-105">適用於 IntelliJ 的 Azure 工具組在 2017 年 8 月更新，有不同的工作流程。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-105">The Azure Toolkit for IntelliJ was updated in August 2017, with a different workflow.</span></span> <span data-ttu-id="b7e0b-106">考量到這點，本文包含兩個不同的部分：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-106">With that in mind, this article contains two different sections:</span></span>
>
> * <span data-ttu-id="b7e0b-107">第一部分說明如何使用適用於 IntelliJ 的 Azure 工具組 2017 年 8 月 (含) 以後版本的來建立 Hello World Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-107">The first section illustrates creating a Hello World web app by using the August 2017 (or later) version of the Azure Toolkit for IntelliJ.</span></span>
>
> * <span data-ttu-id="b7e0b-108">本文第二個部分將示範如何使用工具組的 2017 年 4 月 (含) 以前版本建立 Hello World web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-108">The second section of this article demonstrates creating a Hello World web app by using the April 2017 (or earlier) version of the toolkit.</span></span>
> 

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-hello-world-web-app-by-using-the-version-307-or-later-toolkit"></a><span data-ttu-id="b7e0b-109">使用 3.0.7 版或以後版本的工具組來建立 Hello World Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b7e0b-109">Create a Hello World web app by using the version 3.0.7 or later toolkit</span></span>

### <a name="create-a-new-web-app-project"></a><span data-ttu-id="b7e0b-110">建立新的 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="b7e0b-110">Create a new web app project</span></span>

1. <span data-ttu-id="b7e0b-111">使用 [適用於 IntelliJ 的 Azure 工具組登入指示] 文章中的步驟來啟動 IntelliJ 和登入您的 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-111">Start IntelliJ and sign-in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="b7e0b-112">按一下 [檔案] 功能表，按一下 [新增]，然後按一下 [專案]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-112">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![建立新專案][file-new-project]

1. <span data-ttu-id="b7e0b-114">在 **新增專案** 對話方塊中，選取 **Maven** ，然後選取 **maven-archetype-webapp** ，再按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-114">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![選擇 Maven archetype webapp][maven-archetype-webapp]
   
1. <span data-ttu-id="b7e0b-116">指定 Web 應用程式的 [GroupId] 和 [ArtifactId]，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-116">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="b7e0b-118">自訂任何 Maven 設定或接受預設值，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-118">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![指定 Maven 設定][maven-options]

1. <span data-ttu-id="b7e0b-120">指定專案名稱和位置，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-120">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定專案名稱][project-name]

1. <span data-ttu-id="b7e0b-122">在 IntelliJ 的 [專案總管] 檢視中，依序展開 [src]、[main] 和 [webapp]，然後按兩下 [index.jsp]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-122">Within IntelliJ's Project Explorer view, expand **src**, then **main**, then **webapp**, and then double-click **index.jsp**.</span></span>
   
   ![開啟索引頁面][open-index-page]

1. <span data-ttu-id="b7e0b-124">當 index.jsp 檔案在 IntelliJ 中開啟時，加入文字以動態顯示 **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="b7e0b-124">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="b7e0b-125">(在現有 `<body>` 元素內)。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-125">within the existing `<body>` element.</span></span> <span data-ttu-id="b7e0b-126">您已更新的 `<body>` 內容看起來應該與下列範例類似：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-126">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![編輯索引頁面][edit-index-page]

1. <span data-ttu-id="b7e0b-128">儲存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-128">Save index.jsp.</span></span>

### <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="b7e0b-129">將 Web 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="b7e0b-129">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="b7e0b-130">在 IntelliJ 的 [專案總管] 畫面，以滑鼠右鍵按一下您的專案，並選擇 [Azure]，然後選擇 [在 Web 應用程式上執行]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-130">Within IntelliJ's Project Explorer view, right-click your project, choose **Azure**, and then choose **Run on Web App**.</span></span>
   
   ![[在 Web 應用程式上執行] 功能表][run-on-web-app-menu]

1. <span data-ttu-id="b7e0b-132">在 [在 Web 應用程式上執行] 對話方塊中，可以選擇下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-132">In the Run on Web App dialog box, you can choose one of the following options:</span></span>

   * <span data-ttu-id="b7e0b-133">請選擇現有 Web 應用程式 (如果有)，然後按一下 [執行]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-133">Choose an existing web app (if one exists), and then click **Run**.</span></span>

      ![[在 Web 應用程式上執行] 對話方塊][run-on-web-app-dialog]

   * <span data-ttu-id="b7e0b-135">按一下 [建立新的 Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-135">Click **Create New Web App**.</span></span> <span data-ttu-id="b7e0b-136">如果您選擇建立新的 Web 應用程式，請指定 Web 應用程式的必要資訊，然後按一下 [執行]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-136">If you choose to create a new web app, specify the requisite information for your web app, and then click **Run**.</span></span>

      ![建立新的 Web 應用程式][create-new-web-app-dialog]

1. <span data-ttu-id="b7e0b-138">工具組成功部署 Web 應用程式時，會顯示狀態訊息，也會顯示已部署的 Web 應用程式本身的 URL。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-138">The toolkit will display a status message when it has successfully deployed your web app, which will also display the URL of your deployed web app.</span></span>

   ![部署成功][successfully-deployed]

1. <span data-ttu-id="b7e0b-140">您可以使用狀態訊息中提供的連結，瀏覽至您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-140">You can browse to your web app using the link provided in the status message.</span></span>

   ![瀏覽 Web 應用程式][browse-web-app]

1. <span data-ttu-id="b7e0b-142">發佈 Web 應用程式之後，系統會將您的設定儲存為預設值，您可以按一下工具列上的綠色箭號圖示，在 Azure 上執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-142">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="b7e0b-143">您可以透過按一下 Web 應用程式的下拉式功能表，然後按一下 [編輯組態]，修改您的設定。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-143">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![[編輯組態] 功能表][edit-configuration-menu]

1. <span data-ttu-id="b7e0b-145">顯示 [執行/偵錯組態] 對話方塊時，您可以修改任何預設設定，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-145">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![[編輯組態] 對話方塊][edit-configuration-dialog]

<hr />

## <a name="create-a-hello-world-web-app-by-using-the-version-306-or-earlier-toolkit"></a><span data-ttu-id="b7e0b-147">使用 3.0.6 版 (或) 以前版本的工具組建立 Hello World Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b7e0b-147">Create a Hello World web app by using the version 3.0.6 or earlier toolkit</span></span>

### <a name="create-a-new-web-app-project"></a><span data-ttu-id="b7e0b-148">建立新的 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="b7e0b-148">Create a new web app project</span></span>

1. <span data-ttu-id="b7e0b-149">啟動 IntelliJ，按一下 [檔案] 功能表，然後依序按一下 [新增] 及 [專案]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-149">Start IntelliJ and click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![專案 > 新增專案][02]

2. <span data-ttu-id="b7e0b-151">在 [新增專案] 對話方塊中，依序選取 [Java]、[Web 應用程式] 及 [新增]，以新增專案 SDK。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-151">In the **New Project** dialog box, select **Java**, then **Web Application**, and then click **New** to add a Project SDK.</span></span>
   
   ![New Project Dialog][03a]
   
3. <span data-ttu-id="b7e0b-153">在 [選取 JDK 的主目錄] 對話方塊中，選取 JDK 安裝所在的資料夾，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-153">In the Select Home Directory for JDK dialog box, select the folder where your JDK is installed, and then click **OK**.</span></span> <span data-ttu-id="b7e0b-154">在 [新增專案] 對話方塊中，按 [下一步] 繼續。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-154">Click **Next** in the New Project dialog box to continue.</span></span>
   
   ![指定 JDK 主目錄][03b]

4. <span data-ttu-id="b7e0b-156">基於本教學課程的目的，將專案命名為 **Java-Web-App-On-Azure**，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-156">For purposes of this tutorial, name the project **Java-Web-App-On-Azure**, and then click **Finish**.</span></span>
   
   ![New Project Dialog][04]

5. <span data-ttu-id="b7e0b-158">在 IntelliJ 的 [專案總管] 檢視中，依序展開 [Java-Web-App-On-Azure] 和 [web]，然後按兩下 [index.jsp]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-158">Within IntelliJ's Project Explorer view, expand **Java-Web-App-On-Azure**, then expand **web**, and then double-click **index.jsp**.</span></span>
   
   ![開啟索引頁面][05c]

6. <span data-ttu-id="b7e0b-160">當 index.jsp 檔案在 IntelliJ 中開啟時，加入文字以動態顯示 **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="b7e0b-160">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="b7e0b-161">(在現有 `<body>` 元素內)。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-161">within the existing `<body>` element.</span></span> <span data-ttu-id="b7e0b-162">您已更新的 `<body>` 內容看起來應該與下列範例類似：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-162">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

7. <span data-ttu-id="b7e0b-163">儲存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-163">Save index.jsp.</span></span>

### <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="b7e0b-164">將 Web 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="b7e0b-164">Deploy your web app to Azure</span></span>
<span data-ttu-id="b7e0b-165">您有數種方式可以將 Java Web 應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-165">There are several ways by which you can deploy a Java web app to Azure.</span></span> <span data-ttu-id="b7e0b-166">本教學課程說明其中一個最簡單的方式：將您的應用程式部署至 Azure Web 應用程式容器，無需特殊的專案類型或額外的工具。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-166">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="b7e0b-167">Azure 會為您提供 JDK 及 Web 容器軟體，因此您不需要自己上傳；只需要您的 Java Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-167">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="b7e0b-168">如此一來，您的應用程式發行程序只需數秒，連一分鐘都不用。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-168">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

<span data-ttu-id="b7e0b-169">發佈您的應用程式之前，必須先設定模組設定。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-169">Before you publish your application, you first need to configure your module settings.</span></span> <span data-ttu-id="b7e0b-170">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-170">To do so, use the following steps:</span></span>

1. <span data-ttu-id="b7e0b-171">在 IntelliJ 的專案總管中，以滑鼠右鍵按一下 [Java-Web-App-On-Azure]  專案。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-171">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="b7e0b-172">當操作功能表出現時，按一下 [開啟模組設定]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-172">When the context menu appears, click **Open Module Settings**.</span></span>

   ![開啟模組設定][05a]

2. <span data-ttu-id="b7e0b-174">當 [專案結構] 對話方塊出現時：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-174">When the Project Structure dialog box appears:</span></span>

   <span data-ttu-id="b7e0b-175">a.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-175">a.</span></span> <span data-ttu-id="b7e0b-176">按一下 [專案設定] 清單中的 [構件]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-176">Click **Artifacts** in the list of **Project Settings**.</span></span>

   <span data-ttu-id="b7e0b-177">b.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-177">b.</span></span> <span data-ttu-id="b7e0b-178">變更 [名稱] 方塊中的構件名稱，使其不包含空格或特殊字元；這是必要動作，因為此名稱將用於統一資源識別項 (URI) 中。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-178">Change the artifact name in the **Name** box so that it doesn't contain whitespace or special characters; this is necessary since the name will be used in the Uniform Resource Identifier (URI).</span></span>

   <span data-ttu-id="b7e0b-179">c.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-179">c.</span></span> <span data-ttu-id="b7e0b-180">將 [類型] 變更為 [Web 應用程式：封存]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-180">Change the **Type** to **Web Application: Archive**.</span></span>

   <span data-ttu-id="b7e0b-181">d.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-181">d.</span></span> <span data-ttu-id="b7e0b-182">按一下 [確定] 以關閉 [專案結構] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-182">Click **OK** to close the Project Structure dialog box.</span></span>

   ![開啟模組設定][05b]

<span data-ttu-id="b7e0b-184">完成設定您的模組設定時，您可以使用下列步驟來將應用程式發佈到 Azure：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-184">When you have configured your module settings, you can publish your application to Azure by using the following steps:</span></span>

1. <span data-ttu-id="b7e0b-185">在 IntelliJ 的專案總管中，以滑鼠右鍵按一下 [Java-Web-App-On-Azure]  專案。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-185">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="b7e0b-186">操作功能表顯示時，選取 [Azure]，然後按一下 [發佈為 Azure Web 應用程式]</span><span class="sxs-lookup"><span data-stu-id="b7e0b-186">When the context menu appears, select **Azure**, and then click **Publish as Azure Web App...**</span></span>
   
   ![Azure 發佈操作功能表][06]

2. <span data-ttu-id="b7e0b-188">如果尚未從 IntelliJ 登入 Azure，系統會提示您登入 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-188">If you have not already signed in to Azure from IntelliJ, you will be prompted to sign in to your Azure account.</span></span> <span data-ttu-id="b7e0b-189">(如果您有多個 Azure 帳戶，登入程序期間的某些提示即使內容相同也可能會出現多次。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-189">(If you have multiple Azure accounts, some of the prompts during the sign-in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="b7e0b-190">發生此情況時，請遵循登入指示繼續。)</span><span class="sxs-lookup"><span data-stu-id="b7e0b-190">When this happens, continue to follow the sign-in instructions.)</span></span>
   
   ![Azure 登入對話方塊][07]

3. <span data-ttu-id="b7e0b-192">在您成功登入 Azure 帳戶後，[管理訂用帳戶] 對話方塊將會顯示與您的認證相關聯的訂用帳戶清單。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-192">After you have successfully signed in to your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="b7e0b-193">(如果列出多個訂用帳戶，而您只想使用其中幾個帳戶，您可以選擇取消選取不想要使用的訂用帳戶。)當您選取訂用帳戶之後，按一下 [關閉] 。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-193">(If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the subscriptions you don't want to use.) When you have selected your subscriptions, click **Close**.</span></span>
   
   ![管理訂用帳戶][08]

4. <span data-ttu-id="b7e0b-195">當 [部署至 Azure Web 應用程式容器]  對話方塊出現時，它會顯示您先前建立的所有 Web 應用程式容器；如果您尚未建立任何容器，清單將會是空白的。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-195">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![應用程式容器][09]

5. <span data-ttu-id="b7e0b-197">如果您之前尚未建立 Azure Web 應用程式容器，或您想要將應用程式發佈到新的容器中，請使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-197">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="b7e0b-198">否則，請選取現有的 Web 應用程式容器，並跳至以下的步驟 6。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-198">Otherwise, select an existing Web App Container and skip to step 6 below.</span></span>
   
   <span data-ttu-id="b7e0b-199">a.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-199">a.</span></span> <span data-ttu-id="b7e0b-200">按一下 **+** 記號。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-200">Click the **+** sign.</span></span>
      
      ![新增應用程式容器][10]

   <span data-ttu-id="b7e0b-202">b.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-202">b.</span></span> <span data-ttu-id="b7e0b-203">[新增 Web 應用程式容器]  對話方塊會隨即顯示，此對話方塊將用來進行接下來的幾個步驟。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-203">The **New Web App Container** dialog box will be displayed, which will be used for the next several steps.</span></span>
      
      ![新增應用程式容器][11a]
   
   <span data-ttu-id="b7e0b-205">c.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-205">c.</span></span> <span data-ttu-id="b7e0b-206">為您的 Web 應用程式容器輸入 **DNS 標籤** ，這會為您在 Azure 中的 Web 應用程式構成主機 URL 的分葉 DNS 標籤。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-206">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="b7e0b-207">請注意，名稱必須可用，且符合 Azure Web 應用程式命名需求。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-207">Note that the name must be available and conform to Azure Web App naming requirements.</span></span>

   <span data-ttu-id="b7e0b-208">d.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-208">d.</span></span> <span data-ttu-id="b7e0b-209">在 [Web 容器]  下拉式功能表中，為您的應用程式選取適當的軟體。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-209">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="b7e0b-210">目前，您可以從 Tomcat 8、Tomcat 7 或 Jetty 9 選擇。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-210">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="b7e0b-211">所選軟體最新發行的版本由 Azure 提供，會在最新發行的 JDK 8 (由 Oracle 建立並由 Azure 提供) 中運作。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-211">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>

   <span data-ttu-id="b7e0b-212">e.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-212">e.</span></span> <span data-ttu-id="b7e0b-213">在 [訂用帳戶]  下拉式選單中，選取您希望此部署使用的訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-213">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="b7e0b-214">f.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-214">f.</span></span> <span data-ttu-id="b7e0b-215">在 [資源群組]  下拉式功能表中，選取您要與 Web 應用程式相關聯的資源群組。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-215">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="b7e0b-216">(Azure 資源群組可讓您將相關的資源分在同一組，方便一次刪除。)</span><span class="sxs-lookup"><span data-stu-id="b7e0b-216">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="b7e0b-217">您可以選取現有的資源群組 (如果有)，並略過下方步驟 g，或使用以下步驟建立新的資源群組：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-217">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="b7e0b-218">在 [資源群組] 下拉式功能表中，選取 [&lt;&lt; 建立新的資源群組 &gt;&gt;]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-218">Select **&lt;&lt; Create new Resource Group &gt;&gt;** in the **Resource Group** drop-down menu.</span></span>
      * <span data-ttu-id="b7e0b-219">[新增資源群組]  對話方塊會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-219">The **New Resource Group** dialog box will be displayed:</span></span>
        
         ![新增資源群組][12]

      * <span data-ttu-id="b7e0b-221">在 [名稱] 文字方塊中，為新的資源群組指定名稱。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-221">In the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="b7e0b-222">在 [區域] 下拉式功能表中，為資源群組選取適當的 Azure 資料中心位置。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-222">In the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="b7e0b-223">按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-223">Click **OK**.</span></span>

   <span data-ttu-id="b7e0b-224">g.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-224">g.</span></span> <span data-ttu-id="b7e0b-225">[App Service 方案]  下拉式功能表會列出與您選取之資源群組相關聯的應用程式服務方案。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-225">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="b7e0b-226">(App Service 方案會指定特定資訊，例如您 Web 應用程式的位置、定價層以及計算執行個體大小。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-226">(An App Service Plan specifies information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="b7e0b-227">單一 App Service 方案可用於多個 Web Apps，這也就是要與特定 Web 應用程式部署分開維護的原因。)</span><span class="sxs-lookup"><span data-stu-id="b7e0b-227">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
      <span data-ttu-id="b7e0b-228">您可以選取現有的 App Service 方案 (如果有)，並略過下方步驟 h，或使用以下步驟建立新的 App Service 方案：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-228">You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following steps to create a new App Service Plan:</span></span>
      
      * <span data-ttu-id="b7e0b-229">在 [App Service 方案] 下拉式功能表中，選取 [&lt;&lt; 建立新的 App Service 方案 &gt;&gt;]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-229">Select **&lt;&lt; Create new App Service Plan &gt;&gt;** in the **App Service Plan** drop-down menu.</span></span>
      * <span data-ttu-id="b7e0b-230">[新增 App Service 方案]  對話方塊會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-230">The **New App Service Plan** dialog box will be displayed:</span></span>
        
         ![新增 App Service 方案][13]

      * <span data-ttu-id="b7e0b-232">在 [名稱] 文字方塊中，為新的 App Service 方案指定名稱。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-232">In the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="b7e0b-233">在 [位置] 下拉式功能表中，為該方案選取適當的 Azure 資料中心位置。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-233">In the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="b7e0b-234">在 [定價層] 下拉式功能表中，為方案選取適當的價格。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-234">In the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="b7e0b-235">針對測試用途，您可以選擇 [免費] 。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-235">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="b7e0b-236">在 [執行個體大小] 下拉式功能表中，為方案選取適當的執行個體大小。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-236">In the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="b7e0b-237">針對測試用途，您可以選擇 [小型] 。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-237">For testing purposes you can choose **Small**.</span></span>
      * <span data-ttu-id="b7e0b-238">按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-238">Click **OK**.</span></span>

   <span data-ttu-id="b7e0b-239">h.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-239">h.</span></span> <span data-ttu-id="b7e0b-240">(選擇性) 根據預設，Azure 會自動將最新的 Java 8 散發套件部署到 Web 應用程式容器，成為您的 JVM。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-240">(Optional) By default, a recent distribution of Java 8 will be automatically deployed as your JVM by Azure to your web app container.</span></span> <span data-ttu-id="b7e0b-241">不過，您可以選取不同的 JVM 版本和散發套件。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-241">However, you can select a different version and distribution of the JVM.</span></span> <span data-ttu-id="b7e0b-242">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-242">To do so, use the following steps:</span></span>
      
      * <span data-ttu-id="b7e0b-243">在 [新增 Web 應用程式容器] 對話方塊中，按一下 [JDK] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-243">Click the **JDK** tab in the **New Web App Container** dialog box.</span></span>
      * <span data-ttu-id="b7e0b-244">您可以選擇下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-244">You can choose from one of the following options:</span></span>
        
         * <span data-ttu-id="b7e0b-245">部署 Azure 所提供的預設 JDK</span><span class="sxs-lookup"><span data-stu-id="b7e0b-245">Deploy the default JDK which is offered by Azure</span></span>
         * <span data-ttu-id="b7e0b-246">從 Azure 提供的其他 JDK 下接式清單中部署協力廠商 JDK</span><span class="sxs-lookup"><span data-stu-id="b7e0b-246">Deploy a 3rd party JDK from a drop-down list of additional JDKs which are available on Azure</span></span>
         * <span data-ttu-id="b7e0b-247">部署自訂 JDK，這必須封裝為 ZIP 檔案，而且公開可用或位於您的 Azure 儲存體帳戶中</span><span class="sxs-lookup"><span data-stu-id="b7e0b-247">Deploy a custom JDK, which must be packaged as a ZIP file and either publicly available or in your Azure storage account</span></span>
        
      ![新增應用程式容器 JDK 索引標籤][11b]

   <span data-ttu-id="b7e0b-249">i.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-249">i.</span></span> <span data-ttu-id="b7e0b-250">一旦您完成所有上述步驟之後，[New Web App Container] \(新增 Web 應用程式容器) 對話方塊看起來應該如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-250">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![新增應用程式容器][14]
   
   <span data-ttu-id="b7e0b-252">j.</span><span class="sxs-lookup"><span data-stu-id="b7e0b-252">j.</span></span> <span data-ttu-id="b7e0b-253">按一下 [確定]  來完成建立新的 Web 應用程式容器。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-253">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="b7e0b-254">等待數秒鐘，讓 Web 應用程式容器清單重新整理；接著，您應該會在清單中看到新建立的 Web 應用程式容器已被選取。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-254">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

6. <span data-ttu-id="b7e0b-255">現在您已準備好將 Web 應用程式初始部署至 Azure；按一下 [確定]  ，將您的 Java 應用程式部署至選取的 Web 應用程式容器。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-255">You are now ready to complete the initial deployment of your Web App to Azure; click **OK** to deploy your Java application to the selected Web App container.</span></span> <span data-ttu-id="b7e0b-256">根據預設，您的應用程式將會部署為應用程式伺服器的子目錄。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-256">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="b7e0b-257">如果您想要部署為根應用程式，請選取 [部署到根目錄] 核取方塊，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-257">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>
   
   ![部署至 Azure][15]

7. <span data-ttu-id="b7e0b-259">接下來，您應該會看到 [Azure 活動記錄檔]  檢視，它會指出 Web 應用程式的部署狀態。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-259">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![進度指示器][16]
   
   <span data-ttu-id="b7e0b-261">將您的 Web 應用程式部署至 Azure 的程序，應該只需幾秒鐘即可完成。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-261">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="b7e0b-262">當您的應用程式就緒時，您會看見在 [狀態]  欄中出現名稱為**已發佈**的連結。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-262">When your application is ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="b7e0b-263">當您按一下連結時，它會帶您到已部署的 Web 應用程式首頁，或者您也可以使用下一節中的步驟，以瀏覽至您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-263">When you click the link, it will take you to your deployed Web App's home page, or you can use the steps in the following section to browse to your web app.</span></span>

### <a name="browsing-to-your-web-app-on-azure"></a><span data-ttu-id="b7e0b-264">瀏覽至您在 Azure 上的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b7e0b-264">Browsing to your Web App on Azure</span></span>
<span data-ttu-id="b7e0b-265">若要瀏覽至您在 Azure 上的 Web 應用程式，您可以使用 [Azure 總管]  檢視。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-265">To browse to your Web App on Azure, you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="b7e0b-266">如果 [Azure 總管] 檢視尚未開啟，您可以依序按一下 IntelliJ 中的 [檢視] 功能表、[工具視窗]和 [服務總管] 來開啟。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-266">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="b7e0b-267">如果您之前尚未登入，系統將會提示您登入。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-267">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="b7e0b-268">顯示 [Azure 總管] 檢視之後，按照下列這些步驟來瀏覽至您的 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-268">When the **Azure Explorer** view is displayed, follow these steps to browse to your Web App:</span></span> 

1. <span data-ttu-id="b7e0b-269">展開 [Azure]  節點。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-269">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="b7e0b-270">展開 [Web Apps]  節點。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-270">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="b7e0b-271">以滑鼠右鍵按一下所需的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-271">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="b7e0b-272">操作功能表出現時，按一下 [在瀏覽器中開啟] 。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-272">When the context menu appears, click **Open in Browser**.</span></span>
   
   ![瀏覽 Web 應用程式][17]

### <a name="updating-your-web-app"></a><span data-ttu-id="b7e0b-274">更新 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b7e0b-274">Updating your web app</span></span>
<span data-ttu-id="b7e0b-275">更新現有執行中的 Azure Web 應用程式是一項快速又簡單的程序，而且您有兩個更新選項：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-275">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="b7e0b-276">您可以更新現有 Java Web 應用程式的部署。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-276">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="b7e0b-277">您可以將其他的 Java 應用程式發佈到相同的 Web 應用程式容器。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-277">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="b7e0b-278">在任一種情況中，程序都是相同的，而且只需幾秒鐘：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-278">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="b7e0b-279">在 IntelliJ 專案總管中，以滑鼠右鍵按一下您要更新或新增到現有 Web 應用程式容器的 Java 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-279">In the IntelliJ project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>
2. <span data-ttu-id="b7e0b-280">操作功能表顯示時，選取 [Azure]，然後選取 [發佈為 Azure Web 應用程式...]</span><span class="sxs-lookup"><span data-stu-id="b7e0b-280">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="b7e0b-281">由於您之前已經登入，因此會看到您現有 Web 應用程式容器的清單。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-281">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="b7e0b-282">選取您要發佈或重新發佈 Java 應用程式的 Web 應用程式容器，然後按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-282">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="b7e0b-283">幾秒鐘之後，[Azure 活動記錄檔] 檢視將會將您已更新的部署顯示為 [已發佈]，而您將可以在網頁瀏覽器中確認已更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-283">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

### <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="b7e0b-284">啟動、停止或重新啟動現有的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b7e0b-284">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="b7e0b-285">若要啟動或停止現有的 Azure Web 應用程式容器 (包括其中所有已部署的 Java 應用程式)，您可以使用 [Azure 總管]  檢視。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-285">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="b7e0b-286">如果 [Azure 總管] 檢視尚未開啟，您可以依序按一下 IntelliJ 中的 [檢視] 功能表、[工具視窗]和 [服務總管] 來開啟。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-286">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="b7e0b-287">如果您之前尚未登入，系統將會提示您登入。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-287">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="b7e0b-288">顯示 [Azure 總管] 之後，按照下列這些步驟來啟動或停止您的 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="b7e0b-288">When the **Azure Explorer** view is displayed, follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="b7e0b-289">展開 [Azure]  節點。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-289">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="b7e0b-290">展開 [Web Apps]  節點。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-290">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="b7e0b-291">以滑鼠右鍵按一下所需的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-291">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="b7e0b-292">當操作功能表出現時，按一下 [啟動]、[停止] 或 [重新啟動]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-292">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="b7e0b-293">請注意，功能表選項是內容感知的，因此您只能停止執行中的 Web 應用程式，或是啟動目前尚未執行的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-293">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![停止 Web 應用程式][18]

## <a name="next-steps"></a><span data-ttu-id="b7e0b-295">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b7e0b-295">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="b7e0b-296">如需建立 Azure Web Apps 的詳細資訊，請參閱 [Web 應用程式概觀]。</span><span class="sxs-lookup"><span data-stu-id="b7e0b-296">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for IntelliJ (適用於 IntelliJ 的 Azure 工具組)]: azure-toolkit-for-intellij.md
[Web 應用程式概觀]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/18-Stop-Web-App.png

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
