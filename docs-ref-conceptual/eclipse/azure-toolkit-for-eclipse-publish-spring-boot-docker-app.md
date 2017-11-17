---
title: "使用適用於 Eclipse 的 Azure 工具組，將 Spring Boot 應用程式發佈為 Docker 容器"
description: "了解如何使用適用於 Eclipse 的 Azure 工具組，將 Web 應用程式發佈至 Microsoft Azure 作為 Docker 容器。"
services: 
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
ms.openlocfilehash: 6894c319a74c3dba6972f2c78b597cd87b49cc66
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="829b1-103">使用適用於 Eclipse 的 Azure 工具組，將 Spring Boot 應用程式發佈為 Docker 容器</span><span class="sxs-lookup"><span data-stu-id="829b1-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="829b1-104">[Spring Framework] 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="829b1-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="829b1-105">建立在該平台之基礎上的其中一個更熱門的專案是 [Spring Boot]，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="829b1-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="829b1-106">[Docker] 是開放原始碼解決方案，可協助開發人員自動化部署、調整及管理容器中執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="829b1-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="829b1-107">本教學課程會逐步引導您使用適用於 Eclipse 的 Azure 工具組，將 Spring Boot 應用程式作為 Docker 容器部署到 Microsoft Azure。</span><span class="sxs-lookup"><span data-stu-id="829b1-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a><span data-ttu-id="829b1-108">複製預設 Spring Boot Docker 存放庫</span><span class="sxs-lookup"><span data-stu-id="829b1-108">Clone the default Spring Boot Docker repository</span></span>

### <a name="import-the-public-repository"></a><span data-ttu-id="829b1-109">匯入公用存放庫</span><span class="sxs-lookup"><span data-stu-id="829b1-109">Import the public repository</span></span>

<span data-ttu-id="829b1-110">下列步驟會逐步引導您使用 IntelliJ 將 Spring Boot Docker 存放庫複製到本機電腦。</span><span class="sxs-lookup"><span data-stu-id="829b1-110">The following steps walk you through cloning the Spring Boot Docker repository to your local computer by using IntelliJ.</span></span> <span data-ttu-id="829b1-111">如果您想要使用命令列，請參閱[將 Spring Boot 應用程式部署到 Azure Container Service 中的 Linux][Deploy Spring Boot on Linux in ACS]。</span><span class="sxs-lookup"><span data-stu-id="829b1-111">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="829b1-112">開啟 Eclipse。</span><span class="sxs-lookup"><span data-stu-id="829b1-112">Open Eclipse.</span></span>

1. <span data-ttu-id="829b1-113">按一下 [檔案] > [匯入]。</span><span class="sxs-lookup"><span data-stu-id="829b1-113">Click **File** > **Import**.</span></span>

   ![檔案匯入功能表][CL01]

1. <span data-ttu-id="829b1-115">開啟 [匯入] 對話方塊時：</span><span class="sxs-lookup"><span data-stu-id="829b1-115">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="829b1-116">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-116">a.</span></span> <span data-ttu-id="829b1-117">展開 [Git]。</span><span class="sxs-lookup"><span data-stu-id="829b1-117">Expand **Git**.</span></span>

   <span data-ttu-id="829b1-118">b.</span><span class="sxs-lookup"><span data-stu-id="829b1-118">b.</span></span> <span data-ttu-id="829b1-119">選取 [Projects from Git] \(Git 中的專案)。</span><span class="sxs-lookup"><span data-stu-id="829b1-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="829b1-120">c.</span><span class="sxs-lookup"><span data-stu-id="829b1-120">c.</span></span> <span data-ttu-id="829b1-121">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="829b1-121">Click **Next**.</span></span>

   ![匯入對話方塊][CL02]

1. <span data-ttu-id="829b1-123">在 [Select Repository Source] \(選取存放庫來源) 頁面上：</span><span class="sxs-lookup"><span data-stu-id="829b1-123">On the **Select Repository Source** page:</span></span>

   <span data-ttu-id="829b1-124">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-124">a.</span></span> <span data-ttu-id="829b1-125">選取 [Clone URI] \(複製 URI)。</span><span class="sxs-lookup"><span data-stu-id="829b1-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="829b1-126">b.</span><span class="sxs-lookup"><span data-stu-id="829b1-126">b.</span></span> <span data-ttu-id="829b1-127">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="829b1-127">Click **Next**.</span></span>

   ![[選取存放庫來源] 頁面][CL03]

1. <span data-ttu-id="829b1-129">在 [Source Git Repository] \(來源 Git 存放庫) 頁面上：</span><span class="sxs-lookup"><span data-stu-id="829b1-129">On the **Source Git Repository** page:</span></span>

   <span data-ttu-id="829b1-130">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-130">a.</span></span> <span data-ttu-id="829b1-131">針對 [URI]，輸入 `https://github.com/spring-guides/gs-spring-boot-docker.git`。</span><span class="sxs-lookup"><span data-stu-id="829b1-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="829b1-132">此步驟應該會自動填入 [主機] 和 [存放庫路徑] 欄位的正確值。</span><span class="sxs-lookup"><span data-stu-id="829b1-132">This step should automatically populate the **Host** and **Repository path** fields with the correct values.</span></span>
   
   <span data-ttu-id="829b1-133">b.</span><span class="sxs-lookup"><span data-stu-id="829b1-133">b.</span></span> <span data-ttu-id="829b1-134">Spring Boot 是公用存放庫，因此您不應該輸入 Git 使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="829b1-134">The Spring Boot repository is public, so you should not have to enter your Git username and password.</span></span>
   
   <span data-ttu-id="829b1-135">c.</span><span class="sxs-lookup"><span data-stu-id="829b1-135">c.</span></span> <span data-ttu-id="829b1-136">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="829b1-136">Click **Next**.</span></span>

   ![[來源 Git 存放庫] 頁面][CL04]

1. <span data-ttu-id="829b1-138">在 [Branch Selection] \(分支選擇)  頁面上，按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="829b1-138">On the **Branch Selection** page, click **Next**.</span></span>

   ![[分支選擇] 頁面][CL05]

1. <span data-ttu-id="829b1-140">在 [Local Destination] \(本機目的地) 頁面上：</span><span class="sxs-lookup"><span data-stu-id="829b1-140">On the **Local Destination** page:</span></span>

   <span data-ttu-id="829b1-141">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-141">a.</span></span> <span data-ttu-id="829b1-142">指定您想要放置本機存放庫的本機資料夾。</span><span class="sxs-lookup"><span data-stu-id="829b1-142">Specify the local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="829b1-143">b.這是另一個 C# 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="829b1-143">b.</span></span> <span data-ttu-id="829b1-144">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="829b1-144">Click **Next**.</span></span>

   ![[本機目的地] 頁面][CL06]

1. <span data-ttu-id="829b1-146">在 [Select a wizard to use for importing projects] \(選取要用於匯入專案的精靈) 頁面上：</span><span class="sxs-lookup"><span data-stu-id="829b1-146">On the **Select a wizard to use for importing projects** page:</span></span>

   <span data-ttu-id="829b1-147">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-147">a.</span></span> <span data-ttu-id="829b1-148">選取 [Import as a general project] \(匯入為一般專案)。</span><span class="sxs-lookup"><span data-stu-id="829b1-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="829b1-149">b.這是另一個 C# 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="829b1-149">b.</span></span> <span data-ttu-id="829b1-150">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="829b1-150">Click **Next**.</span></span>

   ![[選取要用於匯入專案的精靈] 頁面][CL07]

1. <span data-ttu-id="829b1-152">在 [Import Projects] \(匯入專案) 頁面上：</span><span class="sxs-lookup"><span data-stu-id="829b1-152">On the **Import Projects** page:</span></span>

   <span data-ttu-id="829b1-153">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-153">a.</span></span> <span data-ttu-id="829b1-154">指定專案名稱。</span><span class="sxs-lookup"><span data-stu-id="829b1-154">Specify your project name.</span></span>
   
   <span data-ttu-id="829b1-155">b.</span><span class="sxs-lookup"><span data-stu-id="829b1-155">b.</span></span> <span data-ttu-id="829b1-156">按一下 [完成] 。</span><span class="sxs-lookup"><span data-stu-id="829b1-156">Click **Finish**.</span></span>

   ![[匯入專案] 頁面][CL08]

1. <span data-ttu-id="829b1-158">成功複製存放庫時，您會看到 Eclipse 中列出所有檔案。</span><span class="sxs-lookup"><span data-stu-id="829b1-158">When the repository is cloned successfully, you see all the files listed in Eclipse.</span></span>

   ![Local repository (本機存放庫)][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="829b1-160">從本機存放庫建立 Maven 專案</span><span class="sxs-lookup"><span data-stu-id="829b1-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="829b1-161">Spring Boot Docker 存放庫包含將用於此教學課程的已完成 Maven 專案。</span><span class="sxs-lookup"><span data-stu-id="829b1-161">The Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="829b1-162">按一下 [檔案] > [匯入]。</span><span class="sxs-lookup"><span data-stu-id="829b1-162">Click **File** > **Import**.</span></span>

   ![[檔案] 功能表上的 [匯入] 命令][CL01]

1. <span data-ttu-id="829b1-164">開啟 [匯入] 對話方塊時：</span><span class="sxs-lookup"><span data-stu-id="829b1-164">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="829b1-165">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-165">a.</span></span> <span data-ttu-id="829b1-166">展開 [Maven]。</span><span class="sxs-lookup"><span data-stu-id="829b1-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="829b1-167">b.</span><span class="sxs-lookup"><span data-stu-id="829b1-167">b.</span></span> <span data-ttu-id="829b1-168">選取 [Existing Maven Projects] \(現有 Maven 專案)。</span><span class="sxs-lookup"><span data-stu-id="829b1-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="829b1-169">c.</span><span class="sxs-lookup"><span data-stu-id="829b1-169">c.</span></span> <span data-ttu-id="829b1-170">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="829b1-170">Click **Next**.</span></span>

   ![匯入對話方塊][MV01]

1. <span data-ttu-id="829b1-172">在 [Maven Projects] \(Maven 專案) 頁面上：</span><span class="sxs-lookup"><span data-stu-id="829b1-172">On the **Maven Projects** page:</span></span>

   <span data-ttu-id="829b1-173">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-173">a.</span></span> <span data-ttu-id="829b1-174">針對 [根目錄]，指定您本機存放庫中的 **complete** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="829b1-174">For **Root Directory**, specify the **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="829b1-175">b.</span><span class="sxs-lookup"><span data-stu-id="829b1-175">b.</span></span> <span data-ttu-id="829b1-176">展開 [進階] 區段，然後在 [Name template] \(名稱範本) 中輸入自訂名稱。</span><span class="sxs-lookup"><span data-stu-id="829b1-176">Expand the **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="829b1-177">c.</span><span class="sxs-lookup"><span data-stu-id="829b1-177">c.</span></span> <span data-ttu-id="829b1-178">針對專案中的 **pom.xml** 檔案選取方塊。</span><span class="sxs-lookup"><span data-stu-id="829b1-178">Select the box for the **pom.xml** file in the project.</span></span>
   
   <span data-ttu-id="829b1-179">d.</span><span class="sxs-lookup"><span data-stu-id="829b1-179">d.</span></span> <span data-ttu-id="829b1-180">按一下 [完成] 。</span><span class="sxs-lookup"><span data-stu-id="829b1-180">Click **Finish**.</span></span>

   ![[Maven 專案] 頁面][MV02]

1. <span data-ttu-id="829b1-182">成功開啟 Maven 專案時，您會看到 Eclipse 中列出第二個專案。</span><span class="sxs-lookup"><span data-stu-id="829b1-182">When the Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![Local Maven project (本機 Maven 專案)][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="829b1-184">使用 Maven 建置 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="829b1-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="829b1-185">在 Eclipse 專案總管中，選取 Maven 專案。</span><span class="sxs-lookup"><span data-stu-id="829b1-185">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="829b1-186">按一下 [執行] > [執行身分] > [Maven 組建]。</span><span class="sxs-lookup"><span data-stu-id="829b1-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![以 Maven 組建方式執行的命令][BU01]

1. <span data-ttu-id="829b1-188">成功建置應用程式時，主控台視窗會顯示狀態。</span><span class="sxs-lookup"><span data-stu-id="829b1-188">When your application is successfully built, the console window shows the status.</span></span>

   ![Successful Maven build (成功的 Maven 組建)][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="829b1-190">使用 Docker 容器將您的 Web 應用程式發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="829b1-190">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="829b1-191">在 Eclipse 專案總管中，選取 Maven 專案。</span><span class="sxs-lookup"><span data-stu-id="829b1-191">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="829b1-192">按一下 Azure 發佈 功能表，然後按一下發佈為 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="829b1-192">Click the Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![[發佈為 Docker 容器] 命令][PU01]

1. <span data-ttu-id="829b1-194">當 [在 Azure 上部署 Docker 容器] 對話方塊顯示時：</span><span class="sxs-lookup"><span data-stu-id="829b1-194">When the **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="829b1-195">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-195">a.</span></span> <span data-ttu-id="829b1-196">輸入自訂 Docker 映像名稱。</span><span class="sxs-lookup"><span data-stu-id="829b1-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="829b1-197">b.</span><span class="sxs-lookup"><span data-stu-id="829b1-197">b.</span></span> <span data-ttu-id="829b1-198">在 [Artifact to deploy] \(要部署的構件) 中，指定您剛剛建置之 **gs-spring-boot-docker-0.1.0.jar** 檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="829b1-198">For **Artifact to deploy**, specify the path to the **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Specify Docker options (指定 Docker 選項)][PU02]

   <span data-ttu-id="829b1-200">顯示任何現有的 Docker 主機。</span><span class="sxs-lookup"><span data-stu-id="829b1-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="829b1-201">如果您選擇部署到現有的主機，您可以跳至步驟 5。</span><span class="sxs-lookup"><span data-stu-id="829b1-201">If you choose to deploy to an existing host, you can skip to step 5.</span></span> <span data-ttu-id="829b1-202">否則，請使用下列步驟來建立主機：</span><span class="sxs-lookup"><span data-stu-id="829b1-202">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="829b1-203">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-203">a.</span></span> <span data-ttu-id="829b1-204">按一下 [新增] 。</span><span class="sxs-lookup"><span data-stu-id="829b1-204">Click **Add**.</span></span>

      ![新增 Docker 主機][PU03]

   <span data-ttu-id="829b1-206">b.</span><span class="sxs-lookup"><span data-stu-id="829b1-206">b.</span></span> <span data-ttu-id="829b1-207">當 [建立 Docker 主機] 對話方塊顯示時，您可以選擇接受預設值，或是為新的 Docker 主機指定任何自訂設定。</span><span class="sxs-lookup"><span data-stu-id="829b1-207">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="829b1-208">(如需各種設定的詳細說明，請參閱[使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈為 Docker 容器][Publish Container with Azure Toolkit])。當您已指定要使用的設定時，按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="829b1-208">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![指定 Docker 主機選項][PU04]

   <span data-ttu-id="829b1-210">c.</span><span class="sxs-lookup"><span data-stu-id="829b1-210">c.</span></span> <span data-ttu-id="829b1-211">您可以選擇使用 Azure Key Vault 中的現有登入認證，或者可以選擇輸入新的 Docker 登入認證。</span><span class="sxs-lookup"><span data-stu-id="829b1-211">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="829b1-212">當您已指定選項時，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="829b1-212">Click **Finish** when you have specified your options.</span></span>

      ![指定 Docker 主機認證][PU05]

1. <span data-ttu-id="829b1-214">選取您的 Docker 主機，然後按一下下一步。</span><span class="sxs-lookup"><span data-stu-id="829b1-214">Select your Docker host, and then click **Next**.</span></span>

   ![選取要使用的 Docker 主機][PU06]

1. <span data-ttu-id="829b1-216">在 [在 Azure 上部署 Docker 容器] 對話方塊的最後一頁，指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="829b1-216">On the last page of the **Deploying Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="829b1-217">a.</span><span class="sxs-lookup"><span data-stu-id="829b1-217">a.</span></span> <span data-ttu-id="829b1-218">您可以選擇對將會主控 Docker 容器的容器指定自訂名稱，或者您可以接受預設值。</span><span class="sxs-lookup"><span data-stu-id="829b1-218">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="829b1-219">b.</span><span class="sxs-lookup"><span data-stu-id="829b1-219">b.</span></span> <span data-ttu-id="829b1-220">使用下列語法輸入 Docker 主機的 TCP 連接埠：[外部連接埠]:[內部連接埠]。</span><span class="sxs-lookup"><span data-stu-id="829b1-220">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="829b1-221">例如，**80:8080** 會指定外部連接埠 80 和預設內部 Spring Boot 連接埠 8080。</span><span class="sxs-lookup"><span data-stu-id="829b1-221">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="829b1-222">如果您已自訂內部連接埠 (例如藉由編輯 application.yml 檔案)，您必須指定連接埠號碼才能在 Azure 中正確路由。</span><span class="sxs-lookup"><span data-stu-id="829b1-222">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="829b1-223">c.</span><span class="sxs-lookup"><span data-stu-id="829b1-223">c.</span></span> <span data-ttu-id="829b1-224">設定這些選項之後，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="829b1-224">After you configure these options, click **Finish**.</span></span>

   ![在 Azure 上部署 Docker 容器][PU07]

1. <span data-ttu-id="829b1-226">當 Azure 工具組完成發佈時，Azure 活動記錄會顯示狀態為「已發佈」。</span><span class="sxs-lookup"><span data-stu-id="829b1-226">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![已成功部署 Docker 主機][PU08]

## <a name="next-steps"></a><span data-ttu-id="829b1-228">後續步驟</span><span class="sxs-lookup"><span data-stu-id="829b1-228">Next steps</span></span>

<span data-ttu-id="829b1-229">如需適用於 Docker 的其他資源，請參閱 Docker 的官方網站。</span><span class="sxs-lookup"><span data-stu-id="829b1-229">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]: /azure/container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: azure-toolkit-for-eclipse-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
