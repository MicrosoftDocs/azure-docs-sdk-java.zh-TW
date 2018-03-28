---
title: 使用適用於 IntelliJ 的 Azure 工具組來發佈 Docker 容器
description: 了解如何使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈至 Microsoft Azure 作為 Docker 容器。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 64cefc1ace5d0377dea25fdbdc83d8dada31ddf7
ms.sourcegitcommit: ed130145f9e5c2d803791d96bb118023175e644a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="8531f-103">使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈作為 Docker 容器</span><span class="sxs-lookup"><span data-stu-id="8531f-103">Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="8531f-104">Docker 容器是常見的 Web 應用程式部署方法。</span><span class="sxs-lookup"><span data-stu-id="8531f-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="8531f-105">藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="8531f-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="8531f-106">適用於 IntelliJ 的 Azure 工具組可為 Java 開發人員簡化此部署程序，其憑借的方法是為 Microsoft Azure 的部署新增「發佈作為 Docker 容器」功能。</span><span class="sxs-lookup"><span data-stu-id="8531f-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="8531f-107">本文會引導您完成所需步驟，以便將應用程式發佈至 Azure 來作為 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="8531f-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="8531f-108">Docker 的詳細資訊位於 [Docker 網站]。</span><span class="sxs-lookup"><span data-stu-id="8531f-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="8531f-109">使用 Docker 容器將您的 Web 應用程式發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="8531f-109">Publish your web app to Azure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="8531f-110">若要發佈 Web 應用程式，您必須建立已可供部署的構件。</span><span class="sxs-lookup"><span data-stu-id="8531f-110">To publish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="8531f-111">若要深入了解，請參閱[建立構件的其他資訊](#artifacts)一節。</span><span class="sxs-lookup"><span data-stu-id="8531f-111">To learn more, see the [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="8531f-112">至少完成部署精靈一次之後，其中的大部分設定便會在您再次執行精靈時作為預設值。</span><span class="sxs-lookup"><span data-stu-id="8531f-112">After you have completed the deployment wizard at least once, most of your settings are used as defaults when you run the wizard again.</span></span>
>

1. <span data-ttu-id="8531f-113">在 IntelliJ 中開啟 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="8531f-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="8531f-114">若要啟動**發佈作為 Docker 容器**精靈，請執行下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="8531f-114">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="8531f-115">在 [專案] 工具視窗中，以滑鼠右鍵按一下您的專案，然後依序按一下 [Azure] 和 [發佈作為 Docker 容器]：</span><span class="sxs-lookup"><span data-stu-id="8531f-115">In the **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![[發佈作為 Docker 容器] 命令][PUB01]

   * <span data-ttu-id="8531f-117">在 IntelliJ 工具列中按一下 [發佈群組] 按鈕，然後按一下 [發佈作為 Docker 容器]：</span><span class="sxs-lookup"><span data-stu-id="8531f-117">On the IntelliJ toolbar, click the **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="8531f-118">![[發佈作為 Docker 容器] 命令][PUB02]</span><span class="sxs-lookup"><span data-stu-id="8531f-118">![The Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="8531f-119">[在 Azure 上部署 Docker 容器] 精靈隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="8531f-119">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB03]

3. <span data-ttu-id="8531f-121">在 [輸入映像名稱、選取構件路徑並確認要使用的 Docker 主機] 視窗中，執行下列動作︰</span><span class="sxs-lookup"><span data-stu-id="8531f-121">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span> 

   <span data-ttu-id="8531f-122">a.</span><span class="sxs-lookup"><span data-stu-id="8531f-122">a.</span></span> <span data-ttu-id="8531f-123">在 [Docker 映像名稱] 方塊中，輸入 Docker 主機的唯一名稱 </span><span class="sxs-lookup"><span data-stu-id="8531f-123">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="8531f-124">(精靈會自動建立一個名稱，但您可以修改)。</span><span class="sxs-lookup"><span data-stu-id="8531f-124">(The wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="8531f-125">b.</span><span class="sxs-lookup"><span data-stu-id="8531f-125">b.</span></span> <span data-ttu-id="8531f-126">[主機] 區域會顯示您已建立的任何 Docker 主機。</span><span class="sxs-lookup"><span data-stu-id="8531f-126">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="8531f-127">執行下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="8531f-127">Do either of the following:</span></span> 
      * <span data-ttu-id="8531f-128">如果您有現存的 Docker 主機，您可以對該主機部署 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8531f-128">If you have an existing Docker host, you can deploy your web app to it.</span></span>
      * <span data-ttu-id="8531f-129">若要建立 Docker 主機，則請按一下綠色加號 (**+**)。</span><span class="sxs-lookup"><span data-stu-id="8531f-129">To create a Docker host, click the green plus sign (**+**).</span></span>  
       <span data-ttu-id="8531f-130">[建立 Docker 主機] 對話方塊會隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="8531f-130">The **Create Docker Host** dialog box opens.</span></span> 

      ![[在 Azure 上部署 Docker 容器] 精靈][PUB04a]

4. <span data-ttu-id="8531f-132">在 [設定新的虛擬機器] 視窗中，提供下列有關 Docker 主機的資訊 </span><span class="sxs-lookup"><span data-stu-id="8531f-132">In the **Configure the new virtual machine** window, provide the following information about your Docker host.</span></span> <span data-ttu-id="8531f-133">(精靈會自動為您產生大部分的資訊，但您可以對這些資訊進行修改)。</span><span class="sxs-lookup"><span data-stu-id="8531f-133">(The wizard automatically generates most of the information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="8531f-134">a.</span><span class="sxs-lookup"><span data-stu-id="8531f-134">a.</span></span> <span data-ttu-id="8531f-135">在 [名稱] 方塊中，輸入 Docker 主機的唯一名稱 </span><span class="sxs-lookup"><span data-stu-id="8531f-135">In the **Name** box, enter a unique name for the Docker host.</span></span> <span data-ttu-id="8531f-136">(此名稱與您稍早指定的 Docker 映像名稱不同)。</span><span class="sxs-lookup"><span data-stu-id="8531f-136">(It is not the same as the Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="8531f-137">b.</span><span class="sxs-lookup"><span data-stu-id="8531f-137">b.</span></span> <span data-ttu-id="8531f-138">在 [訂用帳戶] 方塊中，輸入您為主機使用的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="8531f-138">In the **Subscription** box, enter the Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="8531f-139">c.</span><span class="sxs-lookup"><span data-stu-id="8531f-139">c.</span></span> <span data-ttu-id="8531f-140">在 [區域] 方塊中，輸入主機所在的地理區域。</span><span class="sxs-lookup"><span data-stu-id="8531f-140">In the **Region** box, enter the geographical region where your host is located.</span></span>
      
   <span data-ttu-id="8531f-141">d.</span><span class="sxs-lookup"><span data-stu-id="8531f-141">d.</span></span> <span data-ttu-id="8531f-142">在 [作業系統和大小] 索引標籤上，執行下列動作︰</span><span class="sxs-lookup"><span data-stu-id="8531f-142">On the **OS and Size** tab, do the following:</span></span>      
      * <span data-ttu-id="8531f-143">**主機作業系統**︰輸入主機所在虛擬機器的作業系統。</span><span class="sxs-lookup"><span data-stu-id="8531f-143">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="8531f-144">**大小**：輸入主機的虛擬機器大小。</span><span class="sxs-lookup"><span data-stu-id="8531f-144">**Size**: Enter the virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="8531f-145">e.</span><span class="sxs-lookup"><span data-stu-id="8531f-145">e.</span></span> <span data-ttu-id="8531f-146">在 [資源群組] 索引標籤上，選取下列其中一項︰</span><span class="sxs-lookup"><span data-stu-id="8531f-146">On the **Resource Group** tab, select either of the following:</span></span>      
      * <span data-ttu-id="8531f-147">**新的資源群組**︰為主機建立資源群組。</span><span class="sxs-lookup"><span data-stu-id="8531f-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="8531f-148">**現有的資源群組**︰指定 Azure 帳戶中的現有資源群組。</span><span class="sxs-lookup"><span data-stu-id="8531f-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="8531f-149">f.</span><span class="sxs-lookup"><span data-stu-id="8531f-149">f.</span></span> <span data-ttu-id="8531f-150">在 [網路] 索引標籤上，選取下列其中一項︰</span><span class="sxs-lookup"><span data-stu-id="8531f-150">On the **Network** tab, select either of the following:</span></span>      
      * <span data-ttu-id="8531f-151">**新的虛擬網路**︰為主機建立虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="8531f-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="8531f-152">**現有的虛擬網路**︰指定 Azure 帳戶中的現有虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="8531f-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="8531f-153">g.</span><span class="sxs-lookup"><span data-stu-id="8531f-153">g.</span></span> <span data-ttu-id="8531f-154">在 [儲存體] 索引標籤上，選取下列其中一項︰</span><span class="sxs-lookup"><span data-stu-id="8531f-154">On the **Storage** tab, select either of the following:</span></span>      
      * <span data-ttu-id="8531f-155">**新的儲存體帳戶**︰為主機建立儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="8531f-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="8531f-156">**現有的儲存體帳戶**︰指定 Azure 帳戶中的現有儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="8531f-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="8531f-157">按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="8531f-157">Click **Next**.</span></span>  
     <span data-ttu-id="8531f-158">[設定登入認證和連接埠設定] 視窗隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="8531f-158">The **Configure log in credentials and port settings** window opens.</span></span>

      ![[設定登入認證和連接埠設定] 視窗][PUB05]

6. <span data-ttu-id="8531f-160">選取下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="8531f-160">Select one of the following options:</span></span>

      * <span data-ttu-id="8531f-161">**從 Azure Key Vault 匯入認證**︰指定先前儲存的一組認證，這些認證儲存在 Azure 訂用帳戶中。</span><span class="sxs-lookup"><span data-stu-id="8531f-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="8531f-162">使用特定帳戶或服務主體所建立的 Azure Key Vault，無法供另一個共用該訂用帳戶的帳戶或服務主體自動存取。</span><span class="sxs-lookup"><span data-stu-id="8531f-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="8531f-163">若要讓另一個帳戶或服務主體使用 Key Vault，您必須使用 Azure 入口網站來新增該帳戶或服務主體。</span><span class="sxs-lookup"><span data-stu-id="8531f-163">To allow another account or service principal to use the key vault, you must use the Azure portal to add the account or service principal.</span></span>

      * <span data-ttu-id="8531f-164">**新的登入認證**︰建立一組新的登入認證。</span><span class="sxs-lookup"><span data-stu-id="8531f-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="8531f-165">如果您選取此選項，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="8531f-165">If you select this option, do the following:</span></span>

    <span data-ttu-id="8531f-166">a.</span><span class="sxs-lookup"><span data-stu-id="8531f-166">a.</span></span> <span data-ttu-id="8531f-167">在 [VM 認證] 索引標籤上，提供下列 Docker 主機虛擬機器登入認證的資訊：</span><span class="sxs-lookup"><span data-stu-id="8531f-167">On the **VM Credentials** tab, provide the following information for the virtual-machine login credentials of your Docker host:</span></span>

    * <span data-ttu-id="8531f-168">[使用者名稱]︰輸入虛擬機器登入認證的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="8531f-168">**Username**: Enter the username for your virtual-machine login credentials.</span></span>

    * <span data-ttu-id="8531f-169">[密碼] 和 [確認]︰輸入虛擬機器登入認證的密碼。</span><span class="sxs-lookup"><span data-stu-id="8531f-169">**Password** and **Confirm**: Enter the password for your virtual-machine login credentials.</span></span>

    * <span data-ttu-id="8531f-170">**SSH**︰輸入 Docker 主機的安全殼層 (SSH) 設定。</span><span class="sxs-lookup"><span data-stu-id="8531f-170">**SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="8531f-171">您可以選取下列其中一個選項︰</span><span class="sxs-lookup"><span data-stu-id="8531f-171">You can select one of the following options:</span></span>

        * <span data-ttu-id="8531f-172">[無]︰將虛擬機器指定為不允許 SSH 連線。</span><span class="sxs-lookup"><span data-stu-id="8531f-172">**None**: Specifies that your virtual machine does not allow SSH connections.</span></span>

        * <span data-ttu-id="8531f-173">**自動產生**︰自動建立透過 SSH 連線的必要設定。</span><span class="sxs-lookup"><span data-stu-id="8531f-173">**Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>

        * <span data-ttu-id="8531f-174">[從目錄匯入]︰可讓您指定包含一組先前儲存之 SSH 設定的目錄。</span><span class="sxs-lookup"><span data-stu-id="8531f-174">**Import from directory**: Allows you to specify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="8531f-175">此目錄必須包含下列兩個檔案︰</span><span class="sxs-lookup"><span data-stu-id="8531f-175">The directory must contain the following two files:</span></span>

            * <span data-ttu-id="8531f-176">id_rsa︰包含使用者的 RSA 識別。</span><span class="sxs-lookup"><span data-stu-id="8531f-176">*id_rsa*: Contains the RSA identification for a user.</span></span>

            * <span data-ttu-id="8531f-177">id_rsa.pub︰包含用於驗證的 RSA 公開金鑰。</span><span class="sxs-lookup"><span data-stu-id="8531f-177">*id_rsa.pub*: Contains the RSA public key that is used for authentication.</span></span>

    <span data-ttu-id="8531f-178">b.</span><span class="sxs-lookup"><span data-stu-id="8531f-178">b.</span></span> <span data-ttu-id="8531f-179">在 [Docker 精靈存取] 索引標籤上，提供下列資訊︰</span><span class="sxs-lookup"><span data-stu-id="8531f-179">On the **Docker Daemon Access** tab, provide the following information:</span></span>

    ![建立 Docker 主機][PUB06]
    
    * <span data-ttu-id="8531f-181">**Docker 精靈連接埠**︰輸入 Docker 主機的唯一 TCP 通訊埠。</span><span class="sxs-lookup"><span data-stu-id="8531f-181">**Docker Daemon port**: Enter the unique TCP port for your Docker host.</span></span>
    
    * <span data-ttu-id="8531f-182">**TLS 安全性**︰輸入 Docker 主機的傳輸層安全性設定。</span><span class="sxs-lookup"><span data-stu-id="8531f-182">**TLS Security**: Enter the Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="8531f-183">您可選擇下列選項：</span><span class="sxs-lookup"><span data-stu-id="8531f-183">You can choose from the following options:</span></span>
    
        * <span data-ttu-id="8531f-184">[無]︰將虛擬機器指定為不允許 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="8531f-184">**None**: Specifies that your virtual machine does not allow TLS connections.</span></span>
        
        * <span data-ttu-id="8531f-185">**自動產生**︰自動建立透過 TLS 連線的必要設定。</span><span class="sxs-lookup"><span data-stu-id="8531f-185">**Auto-generate**: Automatically creates the requisite settings for connecting via TLS.</span></span>
        
        * <span data-ttu-id="8531f-186">**從目錄匯入**︰指定內含一組先前儲存之 TLS 設定的目錄。</span><span class="sxs-lookup"><span data-stu-id="8531f-186">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="8531f-187">該目錄必須包含下列六個檔案︰</span><span class="sxs-lookup"><span data-stu-id="8531f-187">The directory must contain the following six files:</span></span>
        
            * <span data-ttu-id="8531f-188">ca.pem 和 ca key.pem︰包含 TLS 憑證授權單位的憑證和公開金鑰。</span><span class="sxs-lookup"><span data-stu-id="8531f-188">*ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.</span></span>
            
            * <span data-ttu-id="8531f-189">cert.pem 和 key.pem︰包含 TLS 驗證使用的用戶端憑證和公開金鑰。</span><span class="sxs-lookup"><span data-stu-id="8531f-189">*cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.</span></span>
            
            * <span data-ttu-id="8531f-190">server.pem 和 server-key.pem︰包含用於 TLS 驗證使用的用戶端憑證和公開金鑰。</span><span class="sxs-lookup"><span data-stu-id="8531f-190">*server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.</span></span>

7. <span data-ttu-id="8531f-191">在輸入必要資訊之後，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="8531f-191">After you have entered the required information, click **Finish**.</span></span>  
    <span data-ttu-id="8531f-192">[在 Azure 上部署 Docker 容器] 精靈隨即重新開啟。</span><span class="sxs-lookup"><span data-stu-id="8531f-192">The **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB07]

8. <span data-ttu-id="8531f-194">按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="8531f-194">Click **Next**.</span></span>  
    <span data-ttu-id="8531f-195">[設定要建立的 Docker 容器] 視窗隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="8531f-195">The **Configure the Docker container to be created** window opens.</span></span>

   ![[設定要建立的 Docker 容器] 視窗][PUB08]

9. <span data-ttu-id="8531f-197">在 [設定要建立的 Docker 容器] 視窗中，提供下列資訊︰</span><span class="sxs-lookup"><span data-stu-id="8531f-197">In the **Configure the Docker container to be created** window, provide the following information:</span></span> 

   <span data-ttu-id="8531f-198">a.</span><span class="sxs-lookup"><span data-stu-id="8531f-198">a.</span></span> <span data-ttu-id="8531f-199">在 [Docker 容器名稱] 方塊中，輸入 Docker 容器的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="8531f-199">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="8531f-200">b.</span><span class="sxs-lookup"><span data-stu-id="8531f-200">b.</span></span> <span data-ttu-id="8531f-201">選擇下列其中一個 Docker 映像：</span><span class="sxs-lookup"><span data-stu-id="8531f-201">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="8531f-202">**預先定義的 Docker 映像**︰指定 Azure 中的既存 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="8531f-202">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="8531f-203">此方塊中的 Docker 映像清單包含數個映像，而我們已將 Azure 工具組設定為要修補這些映像，因此系統會自動部署您的構件。</span><span class="sxs-lookup"><span data-stu-id="8531f-203">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="8531f-204">**自訂 Dockerfile**︰指定本機電腦中先前儲存的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="8531f-204">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="8531f-205">這個更進階的功能適用於想要部署專屬 Dockerfile 的開發人員。</span><span class="sxs-lookup"><span data-stu-id="8531f-205">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="8531f-206">不過，這取決於使用此選項來確保已正確建置其 Dockerfile 的開發人員。</span><span class="sxs-lookup"><span data-stu-id="8531f-206">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="8531f-207">Azure 工具組並不會驗證自訂 Dockerfile 中的內容，因此如果 Dockerfile 有問題，則部署可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="8531f-207">Because the Azure Toolkit does not validate the content in a custom Dockerfile, the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="8531f-208">此外，由於 Azure 工具組預期自訂的 Dockerfile 會包含 Web 應用程式構件，因此會嘗試開啟 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="8531f-208">In addition, because the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, it attempts to open an HTTP connection.</span></span> <span data-ttu-id="8531f-209">如果開發人員發佈不同類型的構件，他們可能會在部署後收到無關緊要的錯誤。</span><span class="sxs-lookup"><span data-stu-id="8531f-209">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="8531f-210">c.</span><span class="sxs-lookup"><span data-stu-id="8531f-210">c.</span></span> <span data-ttu-id="8531f-211">在 [通訊埠設定] 方塊中，輸入 Docker 容器的唯一 TCP 通訊埠繫結。</span><span class="sxs-lookup"><span data-stu-id="8531f-211">In the **Port settings** box, enter the unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="8531f-212">完成上述步驟之後，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="8531f-212">After you have completed the preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="8531f-213">Azure 工具組會開始將您的 Web 應用程式部署至 Azure 中的 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="8531f-213">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> <span data-ttu-id="8531f-214">除非您已將 IntelliJ 設定為在背景中部署，否則畫面上會出現 [部署至 Azure] 進度列。</span><span class="sxs-lookup"><span data-stu-id="8531f-214">Unless you have configured IntelliJ to be deployed in the background, a **Deploying to Azure** progress bar appears.</span></span> 

![部署進度列][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="8531f-216">建立構件的其他資訊</span><span class="sxs-lookup"><span data-stu-id="8531f-216">Additional information about creating artifacts</span></span>

<span data-ttu-id="8531f-217">若要建立已可供部署的構件，請執行下列步驟︰</span><span class="sxs-lookup"><span data-stu-id="8531f-217">To create a deployment-ready artifact, do the following:</span></span>

1. <span data-ttu-id="8531f-218">在 IntelliJ 中開啟 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="8531f-218">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="8531f-219">依序按一下 [檔案] 及 [專案結構]。</span><span class="sxs-lookup"><span data-stu-id="8531f-219">Click **File**, and then click **Project Structure**.</span></span>

   ![[專案結構] 命令][ART01]

3. <span data-ttu-id="8531f-221">若要新增構件，請按一下綠色加號 (**+**)，然後按一下 [Web 應用程式: 封存]。</span><span class="sxs-lookup"><span data-stu-id="8531f-221">To add an artifact, click the green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![[Web 應用程式: 封存] 命令][ART02]

4. <span data-ttu-id="8531f-223">在 [名稱] 方塊中，輸入構件的名稱 (請勿加上「.war」副檔名)，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="8531f-223">In the **Name** box, enter a name for your artifact (do not include the *.war* extension), and then click **OK**.</span></span>

   ![構件的 [名稱] 方塊][ART03]

<span data-ttu-id="8531f-225">如需在 IntelliJ 中建立構件的詳細資訊，請參閱 JetBrains 網站上的[設定構件]。</span><span class="sxs-lookup"><span data-stu-id="8531f-225">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on the JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8531f-226">後續步驟</span><span class="sxs-lookup"><span data-stu-id="8531f-226">Next steps</span></span>

<span data-ttu-id="8531f-227">如需 Docker 的其他資源，請參閱 [Docker 網站]。</span><span class="sxs-lookup"><span data-stu-id="8531f-227">For additional resources for Docker, see the official [Docker website].</span></span>

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
