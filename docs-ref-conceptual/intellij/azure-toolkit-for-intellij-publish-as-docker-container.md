---
title: "使用適用於 IntelliJ 的 Azure 工具組來發佈 Docker 容器"
description: "了解如何使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈至 Microsoft Azure 作為 Docker 容器。"
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
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: 3db8bf06892ca6c53cf93ee4ce151549044806d1
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="f7686-103">使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈作為 Docker 容器</span><span class="sxs-lookup"><span data-stu-id="f7686-103">Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="f7686-104">Docker 容器是常見的 Web 應用程式部署方法。</span><span class="sxs-lookup"><span data-stu-id="f7686-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="f7686-105">藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="f7686-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="f7686-106">適用於 IntelliJ 的 Azure 工具組可為 Java 開發人員簡化此部署程序，其憑借的方法是為 Microsoft Azure 的部署新增「發佈作為 Docker 容器」功能。</span><span class="sxs-lookup"><span data-stu-id="f7686-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="f7686-107">本文會引導您完成所需步驟，以便將應用程式發佈至 Azure 來作為 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="f7686-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="f7686-108">Docker 的詳細資訊位於 [Docker 網站]。</span><span class="sxs-lookup"><span data-stu-id="f7686-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="f7686-109">使用 Docker 容器將您的 Web 應用程式發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="f7686-109">Publish your web app to Azure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="f7686-110">若要發佈 Web 應用程式，您必須建立已可供部署的構件。</span><span class="sxs-lookup"><span data-stu-id="f7686-110">To publish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="f7686-111">若要深入了解，請參閱[建立構件的其他資訊](#artifacts)一節。</span><span class="sxs-lookup"><span data-stu-id="f7686-111">To learn more, see the [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="f7686-112">至少完成部署精靈一次之後，其中的大部分設定便會在您再次執行精靈時作為預設值。</span><span class="sxs-lookup"><span data-stu-id="f7686-112">After you have completed the deployment wizard at least once, most of your settings are used as defaults when you run the wizard again.</span></span>
>

1. <span data-ttu-id="f7686-113">在 IntelliJ 中開啟 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="f7686-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="f7686-114">若要啟動**發佈作為 Docker 容器**精靈，請執行下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="f7686-114">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="f7686-115">在 [專案] 工具視窗中，以滑鼠右鍵按一下您的專案，然後依序按一下 [Azure] 和 [發佈作為 Docker 容器]：</span><span class="sxs-lookup"><span data-stu-id="f7686-115">In the **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![[發佈作為 Docker 容器] 命令][PUB01]

   * <span data-ttu-id="f7686-117">在 IntelliJ 工具列中按一下 [發佈群組] 按鈕，然後按一下 [發佈作為 Docker 容器]：</span><span class="sxs-lookup"><span data-stu-id="f7686-117">On the IntelliJ toolbar, click the **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="f7686-118">![[發佈作為 Docker 容器] 命令][PUB02]</span><span class="sxs-lookup"><span data-stu-id="f7686-118">![The Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="f7686-119">[在 Azure 上部署 Docker 容器] 精靈隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="f7686-119">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB03]

3. <span data-ttu-id="f7686-121">在 [輸入映像名稱、選取構件路徑並確認要使用的 Docker 主機] 視窗中，執行下列動作︰</span><span class="sxs-lookup"><span data-stu-id="f7686-121">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span> 

   <span data-ttu-id="f7686-122">a.</span><span class="sxs-lookup"><span data-stu-id="f7686-122">a.</span></span> <span data-ttu-id="f7686-123">在 [Docker 映像名稱] 方塊中，輸入 Docker 主機的唯一名稱 </span><span class="sxs-lookup"><span data-stu-id="f7686-123">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="f7686-124">(精靈會自動建立一個名稱，但您可以修改)。</span><span class="sxs-lookup"><span data-stu-id="f7686-124">(The wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="f7686-125">b.這是另一個 C# 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7686-125">b.</span></span> <span data-ttu-id="f7686-126">[主機] 區域會顯示您已建立的任何 Docker 主機。</span><span class="sxs-lookup"><span data-stu-id="f7686-126">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="f7686-127">執行下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="f7686-127">Do either of the following:</span></span> 
      * <span data-ttu-id="f7686-128">如果您有現存的 Docker 主機，您可以對該主機部署 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7686-128">If you have an existing Docker host, you can deploy your web app to it.</span></span>
      * <span data-ttu-id="f7686-129">若要建立 Docker 主機，則請按一下綠色加號 (**+**)。</span><span class="sxs-lookup"><span data-stu-id="f7686-129">To create a Docker host, click the green plus sign (**+**).</span></span>  
       <span data-ttu-id="f7686-130">[建立 Docker 主機] 對話方塊會隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="f7686-130">The **Create Docker Host** dialog box opens.</span></span> 

      ![[在 Azure 上部署 Docker 容器] 精靈][PUB04a]

4. <span data-ttu-id="f7686-132">在 [設定新的虛擬機器] 視窗中，提供下列有關 Docker 主機的資訊 </span><span class="sxs-lookup"><span data-stu-id="f7686-132">In the **Configure the new virtual machine** window, provide the following information about your Docker host.</span></span> <span data-ttu-id="f7686-133">(精靈會自動為您產生大部分的資訊，但您可以對這些資訊進行修改)。</span><span class="sxs-lookup"><span data-stu-id="f7686-133">(The wizard automatically generates most of the information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="f7686-134">a.</span><span class="sxs-lookup"><span data-stu-id="f7686-134">a.</span></span> <span data-ttu-id="f7686-135">在 [名稱] 方塊中，輸入 Docker 主機的唯一名稱 </span><span class="sxs-lookup"><span data-stu-id="f7686-135">In the **Name** box, enter a unique name for the Docker host.</span></span> <span data-ttu-id="f7686-136">(此名稱與您稍早指定的 Docker 映像名稱不同)。</span><span class="sxs-lookup"><span data-stu-id="f7686-136">(It is not the same as the Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="f7686-137">b.這是另一個 C# 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7686-137">b.</span></span> <span data-ttu-id="f7686-138">在 [訂用帳戶] 方塊中，輸入您為主機使用的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="f7686-138">In the **Subscription** box, enter the Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="f7686-139">c.</span><span class="sxs-lookup"><span data-stu-id="f7686-139">c.</span></span> <span data-ttu-id="f7686-140">在 [區域] 方塊中，輸入主機所在的地理區域。</span><span class="sxs-lookup"><span data-stu-id="f7686-140">In the **Region** box, enter the geographical region where your host is located.</span></span>
      
   <span data-ttu-id="f7686-141">d.</span><span class="sxs-lookup"><span data-stu-id="f7686-141">d.</span></span> <span data-ttu-id="f7686-142">在 [作業系統和大小] 索引標籤上，執行下列動作︰</span><span class="sxs-lookup"><span data-stu-id="f7686-142">On the **OS and Size** tab, do the following:</span></span>      
      * <span data-ttu-id="f7686-143">**主機作業系統**︰輸入主機所在虛擬機器的作業系統。</span><span class="sxs-lookup"><span data-stu-id="f7686-143">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="f7686-144">**大小**：輸入主機的虛擬機器大小。</span><span class="sxs-lookup"><span data-stu-id="f7686-144">**Size**: Enter the virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="f7686-145">e.</span><span class="sxs-lookup"><span data-stu-id="f7686-145">e.</span></span> <span data-ttu-id="f7686-146">在 [資源群組] 索引標籤上，選取下列其中一項︰</span><span class="sxs-lookup"><span data-stu-id="f7686-146">On the **Resource Group** tab, select either of the following:</span></span>      
      * <span data-ttu-id="f7686-147">**新的資源群組**︰為主機建立資源群組。</span><span class="sxs-lookup"><span data-stu-id="f7686-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="f7686-148">**現有的資源群組**︰指定 Azure 帳戶中的現有資源群組。</span><span class="sxs-lookup"><span data-stu-id="f7686-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="f7686-149">f.</span><span class="sxs-lookup"><span data-stu-id="f7686-149">f.</span></span> <span data-ttu-id="f7686-150">在 [網路] 索引標籤上，選取下列其中一項︰</span><span class="sxs-lookup"><span data-stu-id="f7686-150">On the **Network** tab, select either of the following:</span></span>      
      * <span data-ttu-id="f7686-151">**新的虛擬網路**︰為主機建立虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="f7686-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="f7686-152">**現有的虛擬網路**︰指定 Azure 帳戶中的現有虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="f7686-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="f7686-153">g.</span><span class="sxs-lookup"><span data-stu-id="f7686-153">g.</span></span> <span data-ttu-id="f7686-154">在 [儲存體] 索引標籤上，選取下列其中一項︰</span><span class="sxs-lookup"><span data-stu-id="f7686-154">On the **Storage** tab, select either of the following:</span></span>      
      * <span data-ttu-id="f7686-155">**新的儲存體帳戶**︰為主機建立儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="f7686-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="f7686-156">**現有的儲存體帳戶**︰指定 Azure 帳戶中的現有儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="f7686-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="f7686-157">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="f7686-157">Click **Next**.</span></span>  
     <span data-ttu-id="f7686-158">[設定登入認證和連接埠設定] 視窗隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="f7686-158">The **Configure log in credentials and port settings** window opens.</span></span>

      ![[設定登入認證和連接埠設定] 視窗][PUB05]

6. <span data-ttu-id="f7686-160">選取下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="f7686-160">Select one of the following options:</span></span>

      * <span data-ttu-id="f7686-161">**從 Azure Key Vault 匯入認證**︰指定先前儲存的一組認證，這些認證儲存在 Azure 訂用帳戶中。</span><span class="sxs-lookup"><span data-stu-id="f7686-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="f7686-162">使用特定帳戶或服務主體所建立的 Azure Key Vault，無法供另一個共用該訂用帳戶的帳戶或服務主體自動存取。</span><span class="sxs-lookup"><span data-stu-id="f7686-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="f7686-163">若要讓另一個帳戶或服務主體使用 Key Vault，您必須使用 Azure 入口網站來新增該帳戶或服務主體。</span><span class="sxs-lookup"><span data-stu-id="f7686-163">To allow another account or service principal to use the key vault, you must use the Azure portal to add the account or service principal.</span></span>

      * <span data-ttu-id="f7686-164">**新的登入認證**︰建立一組新的登入認證。</span><span class="sxs-lookup"><span data-stu-id="f7686-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="f7686-165">如果您選取此選項，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="f7686-165">If you select this option, do the following:</span></span>

        <span data-ttu-id="f7686-166">a.</span><span class="sxs-lookup"><span data-stu-id="f7686-166">a.</span></span> <span data-ttu-id="f7686-167">在 **VM 認證** 索引標籤上提供下列資訊，以供 Docker 主機的虛擬機器登入認證使用︰ * **使用者名稱** ︰ 輸入虛擬機器登入認證的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="f7686-167">On the **VM Credentials** tab, provide the following information for the virtual-machine login credentials of your Docker host: * **Username**: Enter the username for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="f7686-168">* **密碼**和**確認**︰輸入虛擬機器登入認證的密碼。</span><span class="sxs-lookup"><span data-stu-id="f7686-168">* **Password** and **Confirm**: Enter the password for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="f7686-169">* **SSH**︰輸入 Docker 主機的安全殼層 (SSH) 設定。</span><span class="sxs-lookup"><span data-stu-id="f7686-169">* **SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="f7686-170">您可以選取下列其中一個選項： * **無** ︰ 將虛擬機器指定為不允許 SSH 連線。</span><span class="sxs-lookup"><span data-stu-id="f7686-170">You can select one of the following options: * **None**: Specifies that your virtual machine does not allow SSH connections.</span></span>
                <span data-ttu-id="f7686-171">* **自動產生**︰自動建立透過 SSH 連線的必要設定。</span><span class="sxs-lookup"><span data-stu-id="f7686-171">* **Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>
                <span data-ttu-id="f7686-172">* **從目錄匯入**︰可讓您指定內含一組先前儲存之 SSH 設定的目錄。</span><span class="sxs-lookup"><span data-stu-id="f7686-172">* **Import from directory**: Allows you to specify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="f7686-173">此目錄必須包含下列兩個檔案︰</span><span class="sxs-lookup"><span data-stu-id="f7686-173">The directory must contain the following two files:</span></span>
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        <span data-ttu-id="f7686-174">b.這是另一個 C# 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7686-174">b.</span></span> <span data-ttu-id="f7686-175">在 [Docker 精靈存取] 索引標籤上，提供下列資訊︰</span><span class="sxs-lookup"><span data-stu-id="f7686-175">On the **Docker Daemon Access** tab, provide the following information:</span></span>

          ![建立 Docker 主機][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. <span data-ttu-id="f7686-177">在輸入必要資訊之後，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="f7686-177">After you have entered the required information, click **Finish**.</span></span>  
    <span data-ttu-id="f7686-178">[在 Azure 上部署 Docker 容器] 精靈隨即重新開啟。</span><span class="sxs-lookup"><span data-stu-id="f7686-178">The **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB07]

8. <span data-ttu-id="f7686-180">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="f7686-180">Click **Next**.</span></span>  
    <span data-ttu-id="f7686-181">[設定要建立的 Docker 容器] 視窗隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="f7686-181">The **Configure the Docker container to be created** window opens.</span></span>

   ![[設定要建立的 Docker 容器] 視窗][PUB08]

9. <span data-ttu-id="f7686-183">在 [設定要建立的 Docker 容器] 視窗中，提供下列資訊︰</span><span class="sxs-lookup"><span data-stu-id="f7686-183">In the **Configure the Docker container to be created** window, provide the following information:</span></span> 

   <span data-ttu-id="f7686-184">a.</span><span class="sxs-lookup"><span data-stu-id="f7686-184">a.</span></span> <span data-ttu-id="f7686-185">在 [Docker 容器名稱] 方塊中，輸入 Docker 容器的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="f7686-185">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="f7686-186">b.這是另一個 C# 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7686-186">b.</span></span> <span data-ttu-id="f7686-187">選擇下列其中一個 Docker 映像：</span><span class="sxs-lookup"><span data-stu-id="f7686-187">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="f7686-188">**預先定義的 Docker 映像**︰指定 Azure 中的既存 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="f7686-188">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="f7686-189">此方塊中的 Docker 映像清單包含數個映像，而我們已將 Azure 工具組設定為要修補這些映像，因此系統會自動部署您的構件。</span><span class="sxs-lookup"><span data-stu-id="f7686-189">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="f7686-190">**自訂 Dockerfile**︰指定本機電腦中先前儲存的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="f7686-190">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="f7686-191">這個更進階的功能適用於想要部署專屬 Dockerfile 的開發人員。</span><span class="sxs-lookup"><span data-stu-id="f7686-191">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="f7686-192">不過，這取決於使用此選項來確保已正確建置其 Dockerfile 的開發人員。</span><span class="sxs-lookup"><span data-stu-id="f7686-192">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="f7686-193">Azure 工具組並不會驗證自訂 Dockerfile 中的內容，因此如果 Dockerfile 有問題，則部署可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="f7686-193">Because the Azure Toolkit does not validate the content in a custom Dockerfile, the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="f7686-194">此外，由於 Azure 工具組預期自訂的 Dockerfile 會包含 Web 應用程式構件，因此會嘗試開啟 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="f7686-194">In addition, because the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, it attempts to open an HTTP connection.</span></span> <span data-ttu-id="f7686-195">如果開發人員發佈不同類型的構件，他們可能會在部署後收到無關緊要的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f7686-195">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="f7686-196">c.</span><span class="sxs-lookup"><span data-stu-id="f7686-196">c.</span></span> <span data-ttu-id="f7686-197">在 [通訊埠設定] 方塊中，輸入 Docker 容器的唯一 TCP 通訊埠繫結。</span><span class="sxs-lookup"><span data-stu-id="f7686-197">In the **Port settings** box, enter the unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="f7686-198">完成上述步驟之後，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="f7686-198">After you have completed the preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="f7686-199">Azure 工具組會開始將您的 Web 應用程式部署至 Azure 中的 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="f7686-199">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> <span data-ttu-id="f7686-200">除非您已將 IntelliJ 設定為在背景中部署，否則畫面上會出現 [部署至 Azure] 進度列。</span><span class="sxs-lookup"><span data-stu-id="f7686-200">Unless you have configured IntelliJ to be deployed in the background, a **Deploying to Azure** progress bar appears.</span></span> 

![部署進度列][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="f7686-202">建立構件的其他資訊</span><span class="sxs-lookup"><span data-stu-id="f7686-202">Additional information about creating artifacts</span></span>

<span data-ttu-id="f7686-203">若要建立已可供部署的構件，請執行下列步驟︰</span><span class="sxs-lookup"><span data-stu-id="f7686-203">To create a deployment-ready artifact, do the following:</span></span>

1. <span data-ttu-id="f7686-204">在 IntelliJ 中開啟 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="f7686-204">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="f7686-205">依序按一下 [檔案] 及 [專案結構]。</span><span class="sxs-lookup"><span data-stu-id="f7686-205">Click **File**, and then click **Project Structure**.</span></span>

   ![[專案結構] 命令][ART01]

3. <span data-ttu-id="f7686-207">若要新增構件，請按一下綠色加號 (**+**)，然後按一下 [Web 應用程式: 封存]。</span><span class="sxs-lookup"><span data-stu-id="f7686-207">To add an artifact, click the green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![[Web 應用程式: 封存] 命令][ART02]

4. <span data-ttu-id="f7686-209">在 [名稱] 方塊中，輸入構件的名稱 (請勿加上「.war」副檔名)，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f7686-209">In the **Name** box, enter a name for your artifact (do not include the *.war* extension), and then click **OK**.</span></span>

   ![構件的 [名稱] 方塊][ART03]

<span data-ttu-id="f7686-211">如需在 IntelliJ 中建立構件的詳細資訊，請參閱 JetBrains 網站上的[設定構件]。</span><span class="sxs-lookup"><span data-stu-id="f7686-211">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on the JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7686-212">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f7686-212">Next steps</span></span>

<span data-ttu-id="f7686-213">如需 Docker 的其他資源，請參閱 [Docker 網站]。</span><span class="sxs-lookup"><span data-stu-id="f7686-213">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Docker 網站]: https://www.docker.com/
[設定構件]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
