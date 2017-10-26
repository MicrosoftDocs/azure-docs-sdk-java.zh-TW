---
title: "使用 Eclipse 建立基本的 Azure Web 應用程式"
description: "本教學課程將示範如何使用適用於 Eclipse 的 Azure 工具組來建立 Azure Hello World Web 應用程式。"
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 10/19/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: dcab31ad99fb79a91374d95c2b8b0d9d10346f5f
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a><span data-ttu-id="df3aa-103">使用 Eclipse 建立基本的 Azure Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="df3aa-103">Create a basic Azure web app using Eclipse</span></span>
<span data-ttu-id="df3aa-104">本教學課程示範如何使用 [適用於 Eclipse 的 Azure 工具組]，建立基本的 Hello World 應用程式，並部署到 Azure 做為 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="df3aa-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a Web App by using the [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="df3aa-105">以下所示的基本 JSP 範例乃力求簡潔，不過只要與 Azure 部署相關，類似的步驟皆適用於 Java servlet。</span><span class="sxs-lookup"><span data-stu-id="df3aa-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="df3aa-106">當您完成本教學課程，在網頁瀏覽器中檢視您的應用程式時，看起來會如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="df3aa-106">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World 應用程式預覽][01]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-create-a-hello-world-application"></a><span data-ttu-id="df3aa-108">建立 Hello World 應用程式</span><span class="sxs-lookup"><span data-stu-id="df3aa-108">To create a Hello World application</span></span>
<span data-ttu-id="df3aa-109">首先，我們將從建立 Java 專案開始。</span><span class="sxs-lookup"><span data-stu-id="df3aa-109">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="df3aa-110">啟動 Eclipse，於功能表上依序按一下 [檔案]、[新增] 及 [動態 Web 專案]。</span><span class="sxs-lookup"><span data-stu-id="df3aa-110">Start Eclipse, and at the menu click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="df3aa-111">(如果在按一下 [檔案] 和 [新增] 後沒有看到 [動態 Web 專案] 列為可用的專案，請執行下列動作：依序按一下 [檔案]、[新增]、[專案...]，展開 [Web]，按一下 [動態 Web 專案]，然後按 [下一步]。)</span><span class="sxs-lookup"><span data-stu-id="df3aa-111">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

2. <span data-ttu-id="df3aa-112">基於本教學課程的目的，將專案命名為 **MyWebApp**。</span><span class="sxs-lookup"><span data-stu-id="df3aa-112">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="df3aa-113">您的畫面將出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="df3aa-113">Your screen will appear similar to the following:</span></span>
   
   ![建立新的動態 Web 專案][02]

3. <span data-ttu-id="df3aa-115">按一下 [完成] 。</span><span class="sxs-lookup"><span data-stu-id="df3aa-115">Click **Finish**.</span></span>

4. <span data-ttu-id="df3aa-116">在 Eclipse 的 [專案總管] 檢視中，展開 [MyWebApp]。</span><span class="sxs-lookup"><span data-stu-id="df3aa-116">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="df3aa-117">在 [WebContent] 上按一下滑鼠右鍵、按一下 [新增]，然後按一下 [JSP 檔案]。</span><span class="sxs-lookup"><span data-stu-id="df3aa-117">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

5. <span data-ttu-id="df3aa-118">在 [新增 JSP 檔案] 對話方塊中，將檔案命名為 **index.jsp**、將父資料夾保留為 **MyWebApp/WebContent**，然後按 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="df3aa-118">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

6. <span data-ttu-id="df3aa-119">在 [選取 JSP 範本] 對話方塊中，基於本教學課程的目的，選取 [新增 JSP 檔案 (html)]，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="df3aa-119">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

7. <span data-ttu-id="df3aa-120">當 index.jsp 檔案在 Eclipse 中開啟時，新增文字以動態顯示 **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="df3aa-120">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="df3aa-121">(在現有 `<body>` 元素內)。</span><span class="sxs-lookup"><span data-stu-id="df3aa-121">within the existing `<body>` element.</span></span> <span data-ttu-id="df3aa-122">您已更新的 `<body>` 內容看起來應該與下列範例類似：</span><span class="sxs-lookup"><span data-stu-id="df3aa-122">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="df3aa-123">儲存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="df3aa-123">Save index.jsp.</span></span>

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a><span data-ttu-id="df3aa-124">將應用程式部署至 Azure Web 應用程式容器</span><span class="sxs-lookup"><span data-stu-id="df3aa-124">To deploy your application to an Azure Web App Container</span></span>
<span data-ttu-id="df3aa-125">您有數種方式可以將 Java Web 應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="df3aa-125">There are several ways by which you can deploy a Java web application to Azure.</span></span> <span data-ttu-id="df3aa-126">本教學課程說明其中一個最簡單的方式：將您的應用程式部署至 Azure Web 應用程式容器，無需特殊的專案類型或額外的工具。</span><span class="sxs-lookup"><span data-stu-id="df3aa-126">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="df3aa-127">Azure 會為您提供 JDK 及 Web 容器軟體，因此您不需要自己上傳；只需要您的 Java Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="df3aa-127">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="df3aa-128">如此一來，您的應用程式發行程序只需數秒，連一分鐘都不用。</span><span class="sxs-lookup"><span data-stu-id="df3aa-128">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="df3aa-129">在 Eclipse 的專案總管中，以滑鼠右鍵按一下 [MyWebApp] 。</span><span class="sxs-lookup"><span data-stu-id="df3aa-129">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>

2. <span data-ttu-id="df3aa-130">在操作功能表中，選取 [Azure]，然後按一下 [發佈為 Azure Web 應用程式...]</span><span class="sxs-lookup"><span data-stu-id="df3aa-130">In the context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
   ![發佈為 Azure Web 應用程式][03]
   
   <span data-ttu-id="df3aa-132">或者，您也可以在專案總管中選取 Web 應用程式專案時，按一下工具列的 [發佈] 下拉式按鈕，然後從這裡選取 [發佈為 Azure Web 應用程式]：</span><span class="sxs-lookup"><span data-stu-id="df3aa-132">Alternatively, while your web application project is selected in the Project Explorer, you can click the **Publish** dropdown button on the toolbar and select **Publish as Azure Web App** from there:</span></span>
   
   ![發佈為 Azure Web 應用程式][14]

3. <span data-ttu-id="df3aa-134">如果尚未從 Eclipse 登入 Azure，系統會提示您登入 Azure 帳戶︰</span><span class="sxs-lookup"><span data-stu-id="df3aa-134">If you have not already signed into Azure from Eclipse, you will be prompted to sign into your Azure account:</span></span>
   
   ![[Azure 登入] 對話方塊][04]
   
   <span data-ttu-id="df3aa-136">如果您有多個 Azure 帳戶，登入程序期間的某些提示即使內容相同也可能會出現多次。</span><span class="sxs-lookup"><span data-stu-id="df3aa-136">If you have multiple Azure accounts, some of the prompts during the sign in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="df3aa-137">發生此情況時，請遵循登入指示繼續。</span><span class="sxs-lookup"><span data-stu-id="df3aa-137">When this happens, continue following the sign in instructions.</span></span>

4. <span data-ttu-id="df3aa-138">在您成功登入 Azure 帳戶後，[管理訂用帳戶]  對話方塊將會顯示與您的認證相關聯的訂用帳戶清單。</span><span class="sxs-lookup"><span data-stu-id="df3aa-138">After you have successfully signed into your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="df3aa-139">如果列出多個訂用帳戶，而您只想使用其中幾個帳戶，您可以選擇取消選取要使用的訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="df3aa-139">If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the ones you do want to use.</span></span> <span data-ttu-id="df3aa-140">當您選取訂用帳戶之後，按一下 [關閉] 。</span><span class="sxs-lookup"><span data-stu-id="df3aa-140">When you have selected your subscriptions, click **Close**.</span></span>
   
   ![[管理訂用帳戶] 對話方塊][05]

5. <span data-ttu-id="df3aa-142">當 [部署至 Azure Web 應用程式容器]  對話方塊出現時，它會顯示您先前建立的所有 Web 應用程式容器；如果您尚未建立任何容器，清單將會是空白的。</span><span class="sxs-lookup"><span data-stu-id="df3aa-142">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![[部署至 Azure Web 應用程式容器] 對話方塊][06]

6. <span data-ttu-id="df3aa-144">如果您之前尚未建立 Azure Web 應用程式容器，或您想要將應用程式發佈到新的容器中，請使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="df3aa-144">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="df3aa-145">否則，請選取現有的 Web 應用程式容器，並跳至以下的步驟 7。</span><span class="sxs-lookup"><span data-stu-id="df3aa-145">Otherwise, select an existing Web App Container and skip to step 7 below.</span></span>
   
   <span data-ttu-id="df3aa-146">a.</span><span class="sxs-lookup"><span data-stu-id="df3aa-146">a.</span></span> <span data-ttu-id="df3aa-147">按一下 [完成] </span><span class="sxs-lookup"><span data-stu-id="df3aa-147">Click **New...**</span></span>
      
      ![[部署至 Azure Web 應用程式容器] 對話方塊][15]

   <span data-ttu-id="df3aa-149">b.</span><span class="sxs-lookup"><span data-stu-id="df3aa-149">b.</span></span> <span data-ttu-id="df3aa-150">[新增 Web 應用程式容器]  對話方塊會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="df3aa-150">The **New Web App Container** dialog box will be displayed:</span></span>
      
      ![[新增 Web 應用程式容器] 對話方塊][07a]

   <span data-ttu-id="df3aa-152">c.</span><span class="sxs-lookup"><span data-stu-id="df3aa-152">c.</span></span> <span data-ttu-id="df3aa-153">為您的 Web 應用程式容器輸入 **DNS 標籤** ，這會為您在 Azure 中的 Web 應用程式構成主機 URL 的分葉 DNS 標籤。</span><span class="sxs-lookup"><span data-stu-id="df3aa-153">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="df3aa-154">(請注意，名稱必須可用，且符合 Azure Web 應用程式命名需求。)</span><span class="sxs-lookup"><span data-stu-id="df3aa-154">(Note that the name must be available and conform to Azure Web App naming requirements.)</span></span>

   <span data-ttu-id="df3aa-155">d.</span><span class="sxs-lookup"><span data-stu-id="df3aa-155">d.</span></span> <span data-ttu-id="df3aa-156">在 [Web 容器]  下拉式功能表中，為您的應用程式選取適當的軟體。</span><span class="sxs-lookup"><span data-stu-id="df3aa-156">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="df3aa-157">目前，您可以從 Tomcat 8、Tomcat 7 或 Jetty 9 選擇。</span><span class="sxs-lookup"><span data-stu-id="df3aa-157">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="df3aa-158">所選軟體最新發行的版本由 Azure 提供，會在最新發行的 JDK 8 (由 Oracle 建立並由 Azure 提供) 中運作。</span><span class="sxs-lookup"><span data-stu-id="df3aa-158">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>

   <span data-ttu-id="df3aa-159">e.</span><span class="sxs-lookup"><span data-stu-id="df3aa-159">e.</span></span> <span data-ttu-id="df3aa-160">在 [訂用帳戶]  下拉式選單中，選取您希望此部署使用的訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="df3aa-160">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="df3aa-161">f.</span><span class="sxs-lookup"><span data-stu-id="df3aa-161">f.</span></span> <span data-ttu-id="df3aa-162">在 [資源群組]  下拉式功能表中，選取您要與 Web 應用程式相關聯的資源群組。</span><span class="sxs-lookup"><span data-stu-id="df3aa-162">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="df3aa-163">(Azure 資源群組可讓您將相關的資源分在同一組，方便一次刪除。)</span><span class="sxs-lookup"><span data-stu-id="df3aa-163">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="df3aa-164">您可以選取現有的資源群組 (如果有)，並略過下方步驟 g，或使用以下步驟建立新的資源群組：</span><span class="sxs-lookup"><span data-stu-id="df3aa-164">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following these steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="df3aa-165">按一下 [完成] </span><span class="sxs-lookup"><span data-stu-id="df3aa-165">Click **New...**</span></span>
      * <span data-ttu-id="df3aa-166">[新增資源群組]  對話方塊會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="df3aa-166">The **New Resource Group** dialog box will be displayed:</span></span>
        
          ![[新增資源群組] 對話方塊][08]
      * <span data-ttu-id="df3aa-168">在 [名稱]  文字方塊中，為新的資源群組指定名稱。</span><span class="sxs-lookup"><span data-stu-id="df3aa-168">In the the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="df3aa-169">在 [區域]  下拉式功能表中，為資源群組選取適當的 Azure 資料中心位置。</span><span class="sxs-lookup"><span data-stu-id="df3aa-169">In the the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="df3aa-170">選擇性︰根據預設，Azure 會自動將最新的 Java 8 散發套件部署到 Web 應用程式容器，成為 JVM。</span><span class="sxs-lookup"><span data-stu-id="df3aa-170">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically to your web app container as your JVM.</span></span> <span data-ttu-id="df3aa-171">不過，如果 Web 應用程式需要的話，您也可以指定 JVM 的其他版本和散發套件。</span><span class="sxs-lookup"><span data-stu-id="df3aa-171">However, you can specify a different version and distribution of the JVM if your Web App requires it.</span></span> <span data-ttu-id="df3aa-172">若要指定 Web 應用程式的 JDK，請按一下 [JDK]  索引標籤，然後選取下列選項之一︰</span><span class="sxs-lookup"><span data-stu-id="df3aa-172">To specify the JDK for your Web App, click the **JDK** tab, and select one of the following options:</span></span>
         * <span data-ttu-id="df3aa-173">**部署 Azure Web Apps 服務提供的預設 JDK**︰這個選項會部署最新的 Java 8 散發套件。</span><span class="sxs-lookup"><span data-stu-id="df3aa-173">**Deploy the default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java 8.</span></span>
         * <span data-ttu-id="df3aa-174">**部署 Azure 提供的第三方 JDK**：這個選項可讓您從 Microsoft Azure 提供的 JDK 清單中選擇。</span><span class="sxs-lookup"><span data-stu-id="df3aa-174">**Deploy a 3rd party JDK available on Azure**: This option allows you to choose from the list of JDKs which are provided by Microsoft Azure.</span></span>
         * <span data-ttu-id="df3aa-175">**從這個下載位置部署自己的 JDK：**這個選項可讓您指定自己的 JDK 散發套件。您必須將它封裝為 ZIP 檔案，再上傳到公開使用的下載位置，或您擁有存取權限的 Azure 儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="df3aa-175">**Deploy my own JDK from this download location**: This option allows you to specify your own JDK distribution, which must be packaged as a ZIP file and uploaded to either a publicly available download location or an Azure storage account for which you have access.</span></span>
          
         ![[新增 Web 應用程式容器] 對話方塊][07b]

   <span data-ttu-id="df3aa-177">g.</span><span class="sxs-lookup"><span data-stu-id="df3aa-177">g.</span></span> <span data-ttu-id="df3aa-178">按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="df3aa-178">Click **OK**.</span></span>

   <span data-ttu-id="df3aa-179">h.</span><span class="sxs-lookup"><span data-stu-id="df3aa-179">h.</span></span> <span data-ttu-id="df3aa-180">[App Service 方案]  下拉式功能表會列出與您選取之資源群組相關聯的應用程式服務方案。</span><span class="sxs-lookup"><span data-stu-id="df3aa-180">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="df3aa-181">(App Service 方案會指定如 Web 應用程式的位置、定價層及計算執行個體大小等資訊。</span><span class="sxs-lookup"><span data-stu-id="df3aa-181">(App Service Plans specify information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="df3aa-182">單一 App Service 方案可用於多個 Web Apps，這也就是要與特定 Web 應用程式部署分開維護的原因。)</span><span class="sxs-lookup"><span data-stu-id="df3aa-182">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following these steps to create a new App Service Plan:
      
      * <span data-ttu-id="df3aa-183">按一下 [完成] </span><span class="sxs-lookup"><span data-stu-id="df3aa-183">Click **New...**</span></span>
      * <span data-ttu-id="df3aa-184">[新增 App Service 方案]  對話方塊會隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="df3aa-184">The **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![[新增 App Service 方案] 對話方塊][09]
      * <span data-ttu-id="df3aa-186">在 [名稱]  文字方塊中，為新的 App Service 方案指定名稱。</span><span class="sxs-lookup"><span data-stu-id="df3aa-186">In the the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="df3aa-187">在 [位置]  下拉式功能表中，為該方案選取適當的 Azure 資料中心位置。</span><span class="sxs-lookup"><span data-stu-id="df3aa-187">In the the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="df3aa-188">在 [定價層]  下拉式功能表中，為方案選取適當的價格。</span><span class="sxs-lookup"><span data-stu-id="df3aa-188">In the the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="df3aa-189">針對測試用途，您可以選擇 [免費] 。</span><span class="sxs-lookup"><span data-stu-id="df3aa-189">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="df3aa-190">在 [執行個體大小]  下拉式功能表中，為方案選取適當的執行個體大小。</span><span class="sxs-lookup"><span data-stu-id="df3aa-190">In the the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="df3aa-191">針對測試用途，您可以選擇 [小型] 。</span><span class="sxs-lookup"><span data-stu-id="df3aa-191">For testing purposes you can choose **Small**.</span></span>

   <span data-ttu-id="df3aa-192">i.</span><span class="sxs-lookup"><span data-stu-id="df3aa-192">i.</span></span> <span data-ttu-id="df3aa-193">一旦您完成所有上述步驟之後，[New Web App Container] \(新增 Web 應用程式容器) 對話方塊看起來應該如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="df3aa-193">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![[新增 Web 應用程式容器] 對話方塊][10]

   <span data-ttu-id="df3aa-195">j.</span><span class="sxs-lookup"><span data-stu-id="df3aa-195">j.</span></span> <span data-ttu-id="df3aa-196">按一下 [確定]  來完成建立新的 Web 應用程式容器。</span><span class="sxs-lookup"><span data-stu-id="df3aa-196">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="df3aa-197">等待數秒鐘，讓 Web 應用程式容器清單重新整理；接著，您應該會在清單中看到新建立的 Web 應用程式容器已被選取。</span><span class="sxs-lookup"><span data-stu-id="df3aa-197">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

7. <span data-ttu-id="df3aa-198">您現在已經準備好，可以完成將 Web 應用程式部署至 Azure 的初始部署：</span><span class="sxs-lookup"><span data-stu-id="df3aa-198">You are now ready to complete the initial deployment of your Web App to Azure:</span></span>
   
   ![[部署至 Azure Web 應用程式容器] 對話方塊][11]
   
   <span data-ttu-id="df3aa-200">按一下 [確定]  來將您的 Java 應用程式部署至選取的 Web 應用程式容器。</span><span class="sxs-lookup"><span data-stu-id="df3aa-200">Click **OK** to deploy your Java application to the selected Web App container.</span></span>
   
   <span data-ttu-id="df3aa-201">根據預設，您的應用程式將會部署為應用程式伺服器的子目錄。</span><span class="sxs-lookup"><span data-stu-id="df3aa-201">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="df3aa-202">如果您想要部署為根應用程式，請選取 [部署到根目錄] 核取方塊，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="df3aa-202">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>

8. <span data-ttu-id="df3aa-203">接下來，您應該會看到 [Azure 活動記錄檔]  檢視，它會指出 Web 應用程式的部署狀態。</span><span class="sxs-lookup"><span data-stu-id="df3aa-203">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![Azure 活動記錄檔][12]
   
   <span data-ttu-id="df3aa-205">將您的 Web 應用程式部署至 Azure 的程序，應該只需幾秒鐘即可完成。</span><span class="sxs-lookup"><span data-stu-id="df3aa-205">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="df3aa-206">當您的應用程式就緒時，您會在 [狀態]  in the **已發佈** 的連結。</span><span class="sxs-lookup"><span data-stu-id="df3aa-206">When your application ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="df3aa-207">當您按一下連結時，就會將您帶至已部署的 Web 應用程式首頁。</span><span class="sxs-lookup"><span data-stu-id="df3aa-207">When you click the link, it will take you to your deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="df3aa-208">更新 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="df3aa-208">Updating your web app</span></span>
<span data-ttu-id="df3aa-209">更新現有執行中的 Azure Web 應用程式是一項快速又簡單的程序，而且您有兩個更新選項：</span><span class="sxs-lookup"><span data-stu-id="df3aa-209">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="df3aa-210">您可以更新現有 Java Web 應用程式的部署。</span><span class="sxs-lookup"><span data-stu-id="df3aa-210">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="df3aa-211">您可以將其他的 Java 應用程式發佈到相同的 Web 應用程式容器。</span><span class="sxs-lookup"><span data-stu-id="df3aa-211">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="df3aa-212">在任一種情況中，程序都是相同的，而且只需幾秒鐘：</span><span class="sxs-lookup"><span data-stu-id="df3aa-212">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="df3aa-213">在 Eclipse 專案總管中，以右鍵按一下您要更新或新增到現有 Web 應用程式容器的 Java 應用程式。</span><span class="sxs-lookup"><span data-stu-id="df3aa-213">In the Eclipse project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>

2. <span data-ttu-id="df3aa-214">操作功能表顯示時，選取 [Azure]，然後選取 [發佈為 Azure Web 應用程式...]</span><span class="sxs-lookup"><span data-stu-id="df3aa-214">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>

3. <span data-ttu-id="df3aa-215">由於您之前已經登入，因此會看到您現有 Web 應用程式容器的清單。</span><span class="sxs-lookup"><span data-stu-id="df3aa-215">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="df3aa-216">選取您要發佈或重新發佈 Java 應用程式的 Web 應用程式容器，然後按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="df3aa-216">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="df3aa-217">幾秒鐘之後，[Azure 活動記錄檔] 檢視將會將您已更新的部署顯示為 [已發佈]，而您將可以在網頁瀏覽器中確認已更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="df3aa-217">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="df3aa-218">啟動、停止或重新啟動現有的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="df3aa-218">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="df3aa-219">若要啟動或停止現有的 Azure Web 應用程式容器 (包括其中所有已部署的 Java 應用程式)，您可以使用 [Azure 總管]  檢視。</span><span class="sxs-lookup"><span data-stu-id="df3aa-219">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="df3aa-220">如果 **Azure Explorer** 檢視尚未開啟，您可以依序按一下 Eclipse 中的 [視窗] 功能表、[顯示檢視]、[其他...]、[Azure]，然後按一下 [Azure Explorer] 來開啟。</span><span class="sxs-lookup"><span data-stu-id="df3aa-220">If the **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="df3aa-221">如果您之前尚未登入，系統將會提示您登入。</span><span class="sxs-lookup"><span data-stu-id="df3aa-221">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="df3aa-222">顯示 [Azure 總管]  之後，使用下列這些步驟來啟動或停止您的 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="df3aa-222">When the **Azure Explorer** view is displayed, use follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="df3aa-223">展開 [Azure]  節點。</span><span class="sxs-lookup"><span data-stu-id="df3aa-223">Expand the **Azure** node.</span></span>

2. <span data-ttu-id="df3aa-224">展開 [Web Apps]  節點。</span><span class="sxs-lookup"><span data-stu-id="df3aa-224">Expand the **Web Apps** node.</span></span> 

3. <span data-ttu-id="df3aa-225">以滑鼠右鍵按一下所需的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="df3aa-225">Right-click the desired Web App.</span></span>

4. <span data-ttu-id="df3aa-226">當操作功能表出現時，按一下 [啟動]、[停止] 或 [重新啟動]。</span><span class="sxs-lookup"><span data-stu-id="df3aa-226">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="df3aa-227">請注意，功能表選項是內容感知的，因此您只能停止執行中的 Web 應用程式，或是啟動目前尚未執行的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="df3aa-227">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![停止現有的 Web 應用程式][13]

## <a name="next-steps"></a><span data-ttu-id="df3aa-229">後續步驟</span><span class="sxs-lookup"><span data-stu-id="df3aa-229">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="df3aa-230">如需建立 Azure Web Apps 的詳細資訊，請參閱 [Web 應用程式概觀]。</span><span class="sxs-lookup"><span data-stu-id="df3aa-230">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[適用於 Eclipse 的 Azure 工具組]: azure-toolkit-for-eclipse.md
[Web 應用程式概觀]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
