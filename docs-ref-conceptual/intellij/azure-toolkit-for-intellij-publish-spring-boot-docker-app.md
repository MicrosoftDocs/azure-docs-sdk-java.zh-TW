---
title: "使用適用於 IntelliJ 的 Azure 工具組，將 Spring Boot 應用程式發佈為 Docker 容器"
description: "了解如何使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈至 Microsoft Azure 作為 Docker 容器。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 4228352efa4354bfe4969c1a5ecd3f3b40483f85
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="d088e-103">使用適用於 IntelliJ 的 Azure 工具組，將 Spring Boot 應用程式發佈為 Docker 容器</span><span class="sxs-lookup"><span data-stu-id="d088e-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="d088e-104">[Spring Framework] 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="d088e-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="d088e-105">建立在該平台之基礎上的其中一個更熱門的專案是 [Spring Boot]，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="d088e-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="d088e-106">[Docker] 是開放原始碼解決方案，可協助開發人員自動化部署、調整及管理容器中執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d088e-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="d088e-107">本教學課程會逐步引導您使用適用於 IntelliJ 的 Azure 工具組，將 Spring Boot 應用程式作為 Docker 容器部署到 Microsoft Azure。</span><span class="sxs-lookup"><span data-stu-id="d088e-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a><span data-ttu-id="d088e-108">複製預設 Spring Boot Docker 存放庫</span><span class="sxs-lookup"><span data-stu-id="d088e-108">Clone the default Spring Boot Docker repo</span></span>

<span data-ttu-id="d088e-109">下列步驟會逐步引導您使用 IntelliJ 複製 Spring Boot Docker 存放庫。</span><span class="sxs-lookup"><span data-stu-id="d088e-109">The following steps walk you through cloning the Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="d088e-110">如果您想要使用命令列，請參閱[將 Spring Boot 應用程式部署到 Azure Container Service 中的 Linux][Deploy Spring Boot on Linux in AKS]。</span><span class="sxs-lookup"><span data-stu-id="d088e-110">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in AKS].</span></span>

1. <span data-ttu-id="d088e-111">開啟 IntelliJ。</span><span class="sxs-lookup"><span data-stu-id="d088e-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="d088e-112">在 [歡迎使用] 畫面上，選取 [從版本控制簽出] 清單中的 [GitHub] 選項。</span><span class="sxs-lookup"><span data-stu-id="d088e-112">On the welcome screen, select the **GitHub** option in the **Check out from Version Control** list.</span></span>

   ![版本控制的 GitHub 選項][CL01]

1. <span data-ttu-id="d088e-114">如果系統提示您登入，請輸入您的認證。</span><span class="sxs-lookup"><span data-stu-id="d088e-114">Enter your credentials if you are prompted to log in.</span></span>

   * <span data-ttu-id="d088e-115">如果您使用使用者名稱/密碼來登入 GitHub：</span><span class="sxs-lookup"><span data-stu-id="d088e-115">If you are using a username/password to log in to GitHub:</span></span> 

      ![用於輸入 GitHub 使用者名稱和密碼的對話方塊][CL02a]

   * <span data-ttu-id="d088e-117">如果您使用權杖來登入 GitHub：</span><span class="sxs-lookup"><span data-stu-id="d088e-117">If you are using a token to log in to GitHub:</span></span> 

      ![用於輸入 GitHub 權杖的對話方塊][CL02b]

1. <span data-ttu-id="d088e-119">針對存放庫 URL 輸入 **https://github.com/spring-guides/gs-spring-boot-docker.git**，指定您的本機路徑和資料夾資訊，然後按一下 [複製]。</span><span class="sxs-lookup"><span data-stu-id="d088e-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for the repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![[複製存放庫] 對話方塊][CL03]

1. <span data-ttu-id="d088e-121">當系統提示您建立 IntelliJ 專案時，選取 [否]。</span><span class="sxs-lookup"><span data-stu-id="d088e-121">When you're prompted to create an IntelliJ project, select **No**.</span></span>

   ![拒絕建立 IntelliJ 專案][CL04]

1. <span data-ttu-id="d088e-123">在 [歡迎使用] 頁面上，按一下 [匯入專案]。</span><span class="sxs-lookup"><span data-stu-id="d088e-123">On the welcome page, click **Import Project**.</span></span>

   ![選擇 [匯入專案]][CL05]

1. <span data-ttu-id="d088e-125">找出您複製 Spring Boot 存放庫所在的路徑，選取根目錄底下的 **complete** 資料夾，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d088e-125">Locate the path where you cloned the Spring Boot repo, select the **complete** folder under the root, and then click **OK**.</span></span>

   ![選取要匯入的資料夾][CL06]

1. <span data-ttu-id="d088e-127">當系統提示您時，選取 [從現有來源建立專案]。</span><span class="sxs-lookup"><span data-stu-id="d088e-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![從現有來源建立專案的選項][CL07]

1. <span data-ttu-id="d088e-129">指定您的專案名稱或接受預設值，確認 **complete** 資料夾的正確路徑，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="d088e-129">Specify your project name or accept the default, verify the correct path to the **complete** folder, and then click **Next**.</span></span>

   ![指定專案名稱][CL08]

1. <span data-ttu-id="d088e-131">自訂任何要匯入的目錄，然後按 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="d088e-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![選擇目錄][CL09]

1. <span data-ttu-id="d088e-133">檢閱要匯入的程式庫，然後按 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="d088e-133">Review the libraries to import, and then click **Next**.</span></span>

   ![檢閱專案程式庫][CL10]

1. <span data-ttu-id="d088e-135">檢閱模組結構，然後按 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="d088e-135">Review the module structure, and then click **Next**.</span></span>

   ![檢閱模組結構][CL11]

1. <span data-ttu-id="d088e-137">指定您的 JDK，然後按 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="d088e-137">Specify your JDK, and then click **Next**.</span></span>

   ![指定 JDK][CL12]

1. <span data-ttu-id="d088e-139">按一下 [完成] 。</span><span class="sxs-lookup"><span data-stu-id="d088e-139">Click **Finish**.</span></span>

   ![[完成] 按鈕][CL13]

<span data-ttu-id="d088e-141">IntelliJ 會匯入 Spring Boot 應用程式作為專案，並且在匯入完成時顯示結構。</span><span class="sxs-lookup"><span data-stu-id="d088e-141">IntelliJ imports the Spring Boot app as a project and displays the structure when the import has finished.</span></span>

![IntelliJ 中的 Spring Boot 應用程式][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="d088e-143">建置 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="d088e-143">Build your Spring Boot app</span></span>

### <a name="build-the-app-by-using-the-maven-pom"></a><span data-ttu-id="d088e-144">使用 Maven POM 建置應用程式</span><span class="sxs-lookup"><span data-stu-id="d088e-144">Build the app by using the Maven POM</span></span>

1. <span data-ttu-id="d088e-145">開啟 Maven 工具視窗 (如果尚未開啟)。</span><span class="sxs-lookup"><span data-stu-id="d088e-145">Open the Maven tool window if it is not already opened.</span></span> <span data-ttu-id="d088e-146">按一下 [檢視] > [工具視窗] > [Maven 專案]。</span><span class="sxs-lookup"><span data-stu-id="d088e-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![[工具視窗] 和 [Maven 專案] 命令][BU01]

1. <span data-ttu-id="d088e-148">在 Maven 工具視窗中，以滑鼠右鍵按一下 [套件]，然後選取 [執行 Maven 建置]。</span><span class="sxs-lookup"><span data-stu-id="d088e-148">In the Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="d088e-149">(如果 Maven 專案未自動顯示，請按一下 Maven 工具列上的**重新匯入**圖示)。</span><span class="sxs-lookup"><span data-stu-id="d088e-149">(If your Maven project does not show up automatically, click the **Reimport** icon on the Maven toolbar.)</span></span>

   ![[執行 Maven 建置] 命令][BU02]

1. <span data-ttu-id="d088e-151">Spring Boot 應用程式成功建立時，IntelliJ 應該會顯示「建置成功」訊息。</span><span class="sxs-lookup"><span data-stu-id="d088e-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![「建置成功」訊息][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="d088e-153">建立已可供部署的構件</span><span class="sxs-lookup"><span data-stu-id="d088e-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="d088e-154">若要發佈 Spring Boot 應用程式，您必須建立已可供部署的構件。</span><span class="sxs-lookup"><span data-stu-id="d088e-154">To publish your Spring Boot app, you need to create a deployment-ready artifact.</span></span> <span data-ttu-id="d088e-155">請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="d088e-155">Use the following steps:</span></span>

1. <span data-ttu-id="d088e-156">在 IntelliJ 中開啟 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="d088e-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="d088e-157">依序按一下 [檔案] 及 [專案結構]。</span><span class="sxs-lookup"><span data-stu-id="d088e-157">Click **File**, and then click **Project Structure**.</span></span>

   ![[專案結構] 命令][ART01]

1. <span data-ttu-id="d088e-159">按一下綠色的加號 (**+**) 來新增構件，按一下 [JAR]，然後按一下 [空白]。</span><span class="sxs-lookup"><span data-stu-id="d088e-159">Click the green plus (**+**) symbol to add an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![新增構件][ART02]

1. <span data-ttu-id="d088e-161">為構件命名同時確定未新增 ".jar" 副檔名，然後指定 Maven 輸出的目標資料夾。</span><span class="sxs-lookup"><span data-stu-id="d088e-161">Name your artifact while making sure not to add the ".jar" extension, and then specify the target folder for the Maven output.</span></span>

   ![指定構件屬性][ART03]

1. <span data-ttu-id="d088e-163">建立構件的資訊清單 (選用)：</span><span class="sxs-lookup"><span data-stu-id="d088e-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="d088e-164">a.</span><span class="sxs-lookup"><span data-stu-id="d088e-164">a.</span></span> <span data-ttu-id="d088e-165">按一下 [建立資訊清單]。</span><span class="sxs-lookup"><span data-stu-id="d088e-165">Click **Create Manifest**.</span></span>

      ![按一下 [建立資訊清單] 按鈕][ART04a]

   <span data-ttu-id="d088e-167">b.</span><span class="sxs-lookup"><span data-stu-id="d088e-167">b.</span></span> <span data-ttu-id="d088e-168">選擇構件的預設路徑，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d088e-168">Choose the default path for the artifact, and then click **OK**.</span></span>

      ![指定構件路徑][ART04b]

   <span data-ttu-id="d088e-170">c.</span><span class="sxs-lookup"><span data-stu-id="d088e-170">c.</span></span> <span data-ttu-id="d088e-171">按一下省略符號 (**...**) 以找出主要類別。</span><span class="sxs-lookup"><span data-stu-id="d088e-171">Click the ellipsis (**...**) to locate the main class.</span></span>

      ![找出主要類別][ART04c]

   <span data-ttu-id="d088e-173">d.</span><span class="sxs-lookup"><span data-stu-id="d088e-173">d.</span></span> <span data-ttu-id="d088e-174">選擇您的主要類別，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d088e-174">Choose your main class, and then click **OK**.</span></span>

      ![指定主要類別][ART04d]

1. <span data-ttu-id="d088e-176">按一下 [SERVICEPRINCIPAL] 。</span><span class="sxs-lookup"><span data-stu-id="d088e-176">Click **OK**.</span></span>

   ![關閉 [專案結構] 對話方塊][ART05]

> [!NOTE]
> <span data-ttu-id="d088e-178">如需在 IntelliJ 中建立成品的詳細資訊，請參閱 JetBrains 網站上的[設定成品]。</span><span class="sxs-lookup"><span data-stu-id="d088e-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on the JetBrains website.</span></span>
>

### <a name="build-the-artifact-for-deployment"></a><span data-ttu-id="d088e-179">建置要部署的構件</span><span class="sxs-lookup"><span data-stu-id="d088e-179">Build the artifact for deployment</span></span>

1. <span data-ttu-id="d088e-180">按一下 [建置]，然後按一下 [構件]。</span><span class="sxs-lookup"><span data-stu-id="d088e-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![[建置構件] 命令][BU04]

1. <span data-ttu-id="d088e-182">當 [建置構件] 內容功能表顯示時，按一下 [建置]。</span><span class="sxs-lookup"><span data-stu-id="d088e-182">When the **Build Artifact** context menu appears, click **Build**.</span></span>

   ![[建置構件] 操作功能表][BU05]

<span data-ttu-id="d088e-184">IntelliJ 應該會在專案工具視窗中為 Spring Boot 應用程式顯示已完成的構件。</span><span class="sxs-lookup"><span data-stu-id="d088e-184">IntelliJ should display the completed artifact for your Spring Boot app in the project tool window.</span></span>

   ![建立的構件][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="d088e-186">使用 Docker 容器將您的 Web 應用程式發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="d088e-186">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="d088e-187">如果您尚未登入 Azure 帳戶，請遵循[適用於 IntelliJ 的 Azure 工具組登入指示][Azure Sign In for IntelliJ]中的步驟。</span><span class="sxs-lookup"><span data-stu-id="d088e-187">If you have not signed in to your Azure account, follow the steps in [Sign-in instructions for the Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="d088e-188">在 [專案總管] 工具視窗中，以滑鼠右鍵按一下專案，然後選取 [Azure] > [發佈為 Docker 容器]。</span><span class="sxs-lookup"><span data-stu-id="d088e-188">In the Project Explorer tool window, right-click the project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![[發佈為 Docker 容器] 命令][PU01]

1. <span data-ttu-id="d088e-190">當 [在 Azure 上部署 Docker 容器] 對話方塊顯示時，將會顯示任何現有的 Docker 主機。</span><span class="sxs-lookup"><span data-stu-id="d088e-190">When the **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="d088e-191">如果您選擇部署到現有的主機，您可以跳至步驟 4。</span><span class="sxs-lookup"><span data-stu-id="d088e-191">If you choose to deploy to an existing host, you can skip to step 4.</span></span> <span data-ttu-id="d088e-192">否則，請使用下列步驟來建立主機：</span><span class="sxs-lookup"><span data-stu-id="d088e-192">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="d088e-193">a.</span><span class="sxs-lookup"><span data-stu-id="d088e-193">a.</span></span> <span data-ttu-id="d088e-194">按一下綠色的加號 (**+**)。</span><span class="sxs-lookup"><span data-stu-id="d088e-194">Click the green plus (**+**) symbol.</span></span>

      ![新增 Docker 主機][PU02]

   <span data-ttu-id="d088e-196">b.</span><span class="sxs-lookup"><span data-stu-id="d088e-196">b.</span></span> <span data-ttu-id="d088e-197">當 [建立 Docker 主機] 對話方塊顯示時，您可以選擇接受預設值，或是為新的 Docker 主機指定任何自訂設定。</span><span class="sxs-lookup"><span data-stu-id="d088e-197">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="d088e-198">(如需各種設定的詳細說明，請參閱[使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈為 Docker 容器][Publish Container with Azure Toolkit])。當您已指定要使用的設定時，按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="d088e-198">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![指定 Docker 主機選項][PU03a]

   <span data-ttu-id="d088e-200">c.</span><span class="sxs-lookup"><span data-stu-id="d088e-200">c.</span></span> <span data-ttu-id="d088e-201">您可以選擇使用 Azure Key Vault 中的現有登入認證，或者可以選擇輸入新的 Docker 登入認證。</span><span class="sxs-lookup"><span data-stu-id="d088e-201">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="d088e-202">當您已指定選項時，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="d088e-202">Click **Finish** when you have specified your options.</span></span>

      ![指定 Docker 主機認證][PU03b]

1. <span data-ttu-id="d088e-204">選取您的 Docker 主機，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="d088e-204">Select your Docker host, and then click **Next**.</span></span>

   ![選取要使用的 Docker 主機][PU04]

1. <span data-ttu-id="d088e-206">在 [在 Azure 上部署 Docker 容器] 對話方塊的最後一頁，指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="d088e-206">On the last page of the **Deploy Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="d088e-207">a.</span><span class="sxs-lookup"><span data-stu-id="d088e-207">a.</span></span> <span data-ttu-id="d088e-208">您可以選擇對將會主控 Docker 容器的容器指定自訂名稱，或者您可以接受預設值。</span><span class="sxs-lookup"><span data-stu-id="d088e-208">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="d088e-209">b.</span><span class="sxs-lookup"><span data-stu-id="d088e-209">b.</span></span> <span data-ttu-id="d088e-210">使用下列語法輸入 Docker 主機的 TCP 連接埠：[外部連接埠]:[內部連接埠]。</span><span class="sxs-lookup"><span data-stu-id="d088e-210">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="d088e-211">例如，**80:8080** 會指定外部連接埠 80 和預設內部 Spring Boot 連接埠 8080。</span><span class="sxs-lookup"><span data-stu-id="d088e-211">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="d088e-212">如果您已自訂內部連接埠 (例如藉由編輯 application.yml 檔案)，您必須指定連接埠號碼才能在 Azure 中正確路由。</span><span class="sxs-lookup"><span data-stu-id="d088e-212">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="d088e-213">c.</span><span class="sxs-lookup"><span data-stu-id="d088e-213">c.</span></span> <span data-ttu-id="d088e-214">設定這些選項之後，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="d088e-214">After you have configured these options, click **Finish**.</span></span>

   ![在 Azure 上部署 Docker 容器][PU05]

1. <span data-ttu-id="d088e-216">當 Azure 工具組完成發佈時，Azure 活動記錄會顯示狀態為「已發佈」。</span><span class="sxs-lookup"><span data-stu-id="d088e-216">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![已成功部署 Docker 主機][PU06]

## <a name="next-steps"></a><span data-ttu-id="d088e-218">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d088e-218">Next steps</span></span>

<span data-ttu-id="d088e-219">若要了解使用 IntelliJ 來建立 Spring Boot 應用程式的其他方法，請參閱 JetBrains 網站上的 [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) (建立 Spring Boot 專案)。</span><span class="sxs-lookup"><span data-stu-id="d088e-219">To learn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on the JetBrains website.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[設定成品]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in AKS]: /azure/container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
