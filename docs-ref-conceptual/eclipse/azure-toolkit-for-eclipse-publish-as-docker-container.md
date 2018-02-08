---
title: "使用適用於 Eclipse 的 Azure 工具組來發佈 Docker 容器"
description: "了解如何使用適用於 Eclipse 的 Azure 工具組，將 Web 應用程式發佈至 Microsoft Azure 作為 Docker 容器。"
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
ms.openlocfilehash: aec3682cc14204ca2b395d4b851db7bb68cb8728
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="18334-103">使用 Azure Toolkit for Eclipse 將 Web 應用程式發佈作為 Docker 容器</span><span class="sxs-lookup"><span data-stu-id="18334-103">Publish a web app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="18334-104">Docker 容器是常用的 Web 應用程式部署方法。</span><span class="sxs-lookup"><span data-stu-id="18334-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="18334-105">藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="18334-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="18334-106">Azure Toolkit for Eclipse 可為 Java 開發人員簡化此部署程序，其憑借的方法是為 Microsoft Azure 的部署新增「發佈作為 Docker 容器」功能。</span><span class="sxs-lookup"><span data-stu-id="18334-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="18334-107">本文會引導您完成所需步驟，以便將應用程式發佈至 Azure 來作為 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="18334-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
> <span data-ttu-id="18334-108">Docker 的詳細資訊位於 [Docker 網站]。</span><span class="sxs-lookup"><span data-stu-id="18334-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="18334-109">使用 Docker 容器將您的 Web 應用程式發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="18334-109">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="18334-110">在 Eclipse 中開啟 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="18334-110">Open your web app project in Eclipse.</span></span>

1. <span data-ttu-id="18334-111">若要啟動**發佈作為 Docker 容器**精靈，請執行下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="18334-111">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="18334-112">在 [導覽器] 檢視中，以滑鼠右鍵按一下您的專案，然後依序按一下 [Azure] 和 [發佈作為 Docker 容器]：</span><span class="sxs-lookup"><span data-stu-id="18334-112">In the **Navigator** view, right-click your project, click **Azure**, and then click **Publish as Docker Container**.</span></span>

      ![導覽器檢視 [發佈作為 Docker 容器] 命令][PUB01]

   * <span data-ttu-id="18334-114">在 Eclipse 工具列中按一下 [發佈] 按鈕，然後按一下 [發佈作為 Docker 容器]。</span><span class="sxs-lookup"><span data-stu-id="18334-114">On the Eclipse toolbar, click the **Publish** button, and then click **Publish as Docker Container**.</span></span>

      ![Eclipse 工具列 [發佈作為 Docker 容器] 命令][PUB02]
      
   <span data-ttu-id="18334-116">[在 Azure 上部署 Docker 容器] 精靈隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="18334-116">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB03]

1. <span data-ttu-id="18334-118">在 [輸入映像名稱、選取構件路徑並確認要使用的 Docker 主機] 視窗中，執行下列動作︰</span><span class="sxs-lookup"><span data-stu-id="18334-118">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span>

   <span data-ttu-id="18334-119">a.</span><span class="sxs-lookup"><span data-stu-id="18334-119">a.</span></span> <span data-ttu-id="18334-120">在 [Docker 映像名稱] 方塊中，輸入 Docker 主機的唯一名稱 </span><span class="sxs-lookup"><span data-stu-id="18334-120">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="18334-121">(精靈會自動建立一個名稱，但您可以修改)。</span><span class="sxs-lookup"><span data-stu-id="18334-121">(The wizard automatically creates a name, but you can modify it.)</span></span>

   <span data-ttu-id="18334-122">b.</span><span class="sxs-lookup"><span data-stu-id="18334-122">b.</span></span> <span data-ttu-id="18334-123">[主機] 區域會顯示您已建立的任何 Docker 主機。</span><span class="sxs-lookup"><span data-stu-id="18334-123">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="18334-124">執行下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="18334-124">Do either of the following:</span></span>

   * <span data-ttu-id="18334-125">如果您有現存的 Docker 主機，您可以對該主機部署 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="18334-125">If you have an existing Docker host, you can deploy your web app to it.</span></span>
   * <span data-ttu-id="18334-126">若要建立新的 Docker 主機，請按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="18334-126">To create a new Docker host, click **Add**.</span></span>  
      
      <span data-ttu-id="18334-127">[建立 Docker 主機] 對話方塊會隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="18334-127">The **Create Docker Host** dialog box opens.</span></span>

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB04a]

1. <span data-ttu-id="18334-129">在 [設定新的虛擬機器] 視窗中，對您的 Docker 主機指定下列資訊。</span><span class="sxs-lookup"><span data-stu-id="18334-129">In the **Configure the new virtual machine** window, specify the following options for your Docker host.</span></span> <span data-ttu-id="18334-130">(精靈會自動為您產生大部分的選項，但您可以對這些資訊進行修改)。</span><span class="sxs-lookup"><span data-stu-id="18334-130">(The wizard automatically generates most of the options for you, but you can modify any of them.)</span></span>

   <span data-ttu-id="18334-131">a.</span><span class="sxs-lookup"><span data-stu-id="18334-131">a.</span></span> <span data-ttu-id="18334-132">**名稱**︰輸入 Docker 主機的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="18334-132">**Name**: Enter a unique name for the Docker host.</span></span> <span data-ttu-id="18334-133">(此名稱與您稍早指定的 Docker 映像名稱不同)。</span><span class="sxs-lookup"><span data-stu-id="18334-133">(It is not the same as the Docker image name that you specified earlier.)</span></span>

   <span data-ttu-id="18334-134">b.</span><span class="sxs-lookup"><span data-stu-id="18334-134">b.</span></span> <span data-ttu-id="18334-135">**訂用帳戶**：輸入您用於主機的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="18334-135">**Subscription**: Enter the Azure subscription that you use for your host.</span></span>

   <span data-ttu-id="18334-136">c.</span><span class="sxs-lookup"><span data-stu-id="18334-136">c.</span></span> <span data-ttu-id="18334-137">**區域**：輸入主機所在的地理區域。</span><span class="sxs-lookup"><span data-stu-id="18334-137">**Region**: Enter the geographical region where your host is located.</span></span>

   <span data-ttu-id="18334-138">d.</span><span class="sxs-lookup"><span data-stu-id="18334-138">d.</span></span> <span data-ttu-id="18334-139">在 [主機作業系統和大小] 索引標籤上︰</span><span class="sxs-lookup"><span data-stu-id="18334-139">On the **Host OS and Size** tab:</span></span> 
   * <span data-ttu-id="18334-140">**主機作業系統**︰輸入主機所在虛擬機器的作業系統。</span><span class="sxs-lookup"><span data-stu-id="18334-140">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span>
   * <span data-ttu-id="18334-141">**大小**：輸入主機的虛擬機器大小。</span><span class="sxs-lookup"><span data-stu-id="18334-141">**Size**: Enter the virtual-machine size for your host.</span></span>

   <span data-ttu-id="18334-142">e.</span><span class="sxs-lookup"><span data-stu-id="18334-142">e.</span></span> <span data-ttu-id="18334-143">在 [資源群組] 索引標籤上：</span><span class="sxs-lookup"><span data-stu-id="18334-143">On the **Resource Group** tab:</span></span> 
   * <span data-ttu-id="18334-144">**新的資源群組**︰為主機建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="18334-144">**New resource group**: Create a new resource group for your host.</span></span>
   * <span data-ttu-id="18334-145">**現有的資源群組**︰輸入 Azure 帳戶中的現有資源群組。</span><span class="sxs-lookup"><span data-stu-id="18334-145">**Existing resource group**: Enter an existing resource group from your Azure account.</span></span>

   <span data-ttu-id="18334-146">f.</span><span class="sxs-lookup"><span data-stu-id="18334-146">f.</span></span> <span data-ttu-id="18334-147">在 [網路] 索引標籤上︰</span><span class="sxs-lookup"><span data-stu-id="18334-147">On the **Network** tab:</span></span> 
   * <span data-ttu-id="18334-148">**新的虛擬網路**︰為主機建立新的虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="18334-148">**New virtual network**: Create a new virtual network for your host.</span></span>
   * <span data-ttu-id="18334-149">**現有的虛擬網路**︰輸入 Azure 帳戶中的現有虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="18334-149">**Existing virtual network**: Enter an existing virtual network from your Azure account.</span></span>

   <span data-ttu-id="18334-150">g.</span><span class="sxs-lookup"><span data-stu-id="18334-150">g.</span></span> <span data-ttu-id="18334-151">在 [儲存體] 索引標籤上：</span><span class="sxs-lookup"><span data-stu-id="18334-151">On the **Storage** tab:</span></span> 
   * <span data-ttu-id="18334-152">**新的儲存體帳戶**︰為主機建立新的儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="18334-152">**New storage account**: Create a new storage account for your host.</span></span>
   * <span data-ttu-id="18334-153">**現有的儲存體帳戶**︰輸入 Azure 帳戶中的現有儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="18334-153">**Existing storage account**: Enter an existing storage account from your Azure account.</span></span>

1. <span data-ttu-id="18334-154">按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="18334-154">Click **Next**.</span></span>

1. <span data-ttu-id="18334-155">在 [設定登入認證和連接埠設定] 視窗中，選取下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="18334-155">In the **Configure log in credentials and port settings** window, select one of the following options:</span></span>

   * <span data-ttu-id="18334-156">**從 Azure Key Vault 匯入認證**︰指定先前儲存在 Azure 訂用帳戶中的一組認證。</span><span class="sxs-lookup"><span data-stu-id="18334-156">**Import credentials from Azure Key Vault**: Specifies a previously saved set of credentials that are stored in your Azure subscription.</span></span> 

   >[!NOTE]
   ><span data-ttu-id="18334-157">使用特定帳戶或服務主體所建立的 Azure Key Vault，無法供另一個共用該訂用帳戶的帳戶或服務主體自動存取。</span><span class="sxs-lookup"><span data-stu-id="18334-157">An Azure Key Vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="18334-158">若要讓另一個帳戶或服務主體使用 Key Vault，您必須使用 Azure 入口網站來新增該帳戶或服務主體。</span><span class="sxs-lookup"><span data-stu-id="18334-158">To allow another account or service principal to use the Key Vault, you must use the Azure portal to add the account or service principal.</span></span>
   >

   * <span data-ttu-id="18334-159">**新的登入認證**︰建立一組新的登入認證。</span><span class="sxs-lookup"><span data-stu-id="18334-159">**New log in credentials**: Creates a new set of login credentials.</span></span> <span data-ttu-id="18334-160">如果您選取此選項，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="18334-160">If you select this option, do the following:</span></span> 
    
      * <span data-ttu-id="18334-161">針對 Docker 主機的虛擬機器登入認證，請在 [VM 認證] 索引標籤上選擇下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="18334-161">On the **VM Credentials** tab, choose one of the following options for the virtual-machine login credentials of your Docker host:</span></span> 

         * <span data-ttu-id="18334-162">**使用者名稱**︰輸入虛擬機器登入認證的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="18334-162">**Username**: Enter the username for your virtual machine login credentials.</span></span> 
         * <span data-ttu-id="18334-163">**密碼**和**確認**︰輸入虛擬機器登入認證的密碼。</span><span class="sxs-lookup"><span data-stu-id="18334-163">**Password** and **Confirm**: Enter the password for your virtual machine login credentials.</span></span> 
         * <span data-ttu-id="18334-164">**SSH**︰輸入 Docker 主機的安全殼層 (SSH) 設定。</span><span class="sxs-lookup"><span data-stu-id="18334-164">**SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="18334-165">您可選擇下列選項：</span><span class="sxs-lookup"><span data-stu-id="18334-165">You can choose from the following options:</span></span> 
            * <span data-ttu-id="18334-166">**無**︰指定虛擬機器將不允許 SSH 連線。</span><span class="sxs-lookup"><span data-stu-id="18334-166">**None**: Specifies that your virtual machine will not allow SSH connections.</span></span> 
            * <span data-ttu-id="18334-167">**自動產生**︰自動建立透過 SSH 連線的必要設定。</span><span class="sxs-lookup"><span data-stu-id="18334-167">**Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span> 
            * <span data-ttu-id="18334-168">**從目錄匯入**︰指定內含一組先前儲存之 SSH 設定的目錄。</span><span class="sxs-lookup"><span data-stu-id="18334-168">**Import from directory**: Specifies a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="18334-169">此目錄必須包含下列兩個檔案︰</span><span class="sxs-lookup"><span data-stu-id="18334-169">The directory must contain the following two files:</span></span> 
               * <span data-ttu-id="18334-170">id_rsa︰包含使用者的 RSA 識別。</span><span class="sxs-lookup"><span data-stu-id="18334-170">*id_rsa*: Contains the RSA identification for a user.</span></span> 
               * <span data-ttu-id="18334-171">id_rsa.pub︰包含用於驗證的 RSA 公開金鑰。</span><span class="sxs-lookup"><span data-stu-id="18334-171">*id_rsa.pub*: Contains the RSA public key that is used for authentication.</span></span> 
        
         ![建立 Docker 主機][PUB05]

      * <span data-ttu-id="18334-173">在 [Docker 精靈認證] 索引標籤上，指定下列選項︰</span><span class="sxs-lookup"><span data-stu-id="18334-173">On the **Docker Daemon Credentials** tab, specify the following options:</span></span> 

         * <span data-ttu-id="18334-174">**Docker 精靈連接埠**︰輸入 Docker 主機的唯一 TCP 通訊埠。</span><span class="sxs-lookup"><span data-stu-id="18334-174">**Docker Daemon port**: Enter the unique TCP port for your Docker host.</span></span> 
         * <span data-ttu-id="18334-175">**TLS 安全性**︰輸入 Docker 主機的傳輸層安全性設定。</span><span class="sxs-lookup"><span data-stu-id="18334-175">**TLS Security**: Enter the Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="18334-176">您可選擇下列選項：</span><span class="sxs-lookup"><span data-stu-id="18334-176">You can choose from the following options:</span></span> 
            * <span data-ttu-id="18334-177">**無**︰指定虛擬機器將不允許 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="18334-177">**None**: Specifies that your virtual machine will not allow TLS connections.</span></span> 
            * <span data-ttu-id="18334-178">**自動產生**︰自動建立透過 TLS 連線的必要設定。</span><span class="sxs-lookup"><span data-stu-id="18334-178">**Auto-generate**: Automatically creates the requisite settings for connecting via TLS.</span></span> 
            * <span data-ttu-id="18334-179">**從目錄匯入**︰指定內含一組先前儲存之 TLS 設定的目錄。</span><span class="sxs-lookup"><span data-stu-id="18334-179">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="18334-180">更具體來說，目錄必須包含下列六個檔案︰</span><span class="sxs-lookup"><span data-stu-id="18334-180">More specifically, the directory must contain the following six files:</span></span> 
               * <span data-ttu-id="18334-181">ca.pem 和 ca key.pem︰包含 TLS 憑證授權單位的憑證和公開金鑰。</span><span class="sxs-lookup"><span data-stu-id="18334-181">*ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.</span></span> 
               * <span data-ttu-id="18334-182">cert.pem 和 key.pem︰包含用於 TLS 驗證的用戶端憑證和公開金鑰。</span><span class="sxs-lookup"><span data-stu-id="18334-182">*cert.pem* and *key.pem*: Contain the client certificate and public key that is used for TLS authentication.</span></span> 
               * <span data-ttu-id="18334-183">server.pem 和 server-key.pem︰包含主機的伺服器憑證和公開金鑰。</span><span class="sxs-lookup"><span data-stu-id="18334-183">*server.pem* and *server-key.pem*: Contain the server certificate and public key for the host.</span></span> 

         ![建立 Docker 主機][PUB06]

1. <span data-ttu-id="18334-185">輸入以上所有資訊之後，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="18334-185">After you have entered all of the preceding information, click **Finish**.</span></span>

1. <span data-ttu-id="18334-186">在 [在 Azure 上部署 Docker 容器] 精靈中，按 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="18334-186">In the **Deploy Docker Container on Azure** wizard, click **Next**.</span></span>

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB07]

1. <span data-ttu-id="18334-188">在 [設定要建立的 Docker 容器] 視窗中，執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="18334-188">In the **Configure the Docker container to be created** window, do the following:</span></span>

   <span data-ttu-id="18334-189">a.</span><span class="sxs-lookup"><span data-stu-id="18334-189">a.</span></span> <span data-ttu-id="18334-190">在 [Docker 容器名稱] 方塊中，輸入 Docker 容器的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="18334-190">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="18334-191">b.</span><span class="sxs-lookup"><span data-stu-id="18334-191">b.</span></span> <span data-ttu-id="18334-192">選擇下列其中一個 Docker 映像：</span><span class="sxs-lookup"><span data-stu-id="18334-192">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="18334-193">**預先定義的 Docker 映像**︰指定 Azure 中的既存 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="18334-193">**Predefined Docker image**: Specifies a pre-existing Docker image from Azure.</span></span> 

      >[!NOTE]
      ><span data-ttu-id="18334-194">此方塊中的 Docker 映像清單包含數個映像，而我們已將 Azure 工具組設定為要修補這些映像，因此系統會自動部署您的構件。</span><span class="sxs-lookup"><span data-stu-id="18334-194">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span>
      >

      * <span data-ttu-id="18334-195">**自訂 Dockerfile**︰指定本機電腦中先前儲存的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="18334-195">**Custom Dockerfile**: Specifies a previously saved Dockerfile from your local computer.</span></span>

      >[!NOTE]
      ><span data-ttu-id="18334-196">這個更進階的功能適用於想要部署專屬 Dockerfile 的開發人員。</span><span class="sxs-lookup"><span data-stu-id="18334-196">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="18334-197">不過，這取決於使用此選項來確保已正確建置其 Dockerfile 的開發人員。</span><span class="sxs-lookup"><span data-stu-id="18334-197">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="18334-198">Azure 工具組並不會驗證自訂 Dockerfile 中的內容，因此如果 Dockerfile 有問題，則部署可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="18334-198">The Azure Toolkit does not validate the content in a custom Dockerfile, so the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="18334-199">此外，Azure 工具組預期自訂的 Dockerfile 會包含 Web 應用程式構件，而且會嘗試開啟 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="18334-199">In addition, the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, and it will attempt to open an HTTP connection.</span></span> <span data-ttu-id="18334-200">如果開發人員發佈不同類型的構件，他們可能會在部署後收到無關緊要的錯誤。</span><span class="sxs-lookup"><span data-stu-id="18334-200">If developers publish a different type of artifact, they may receive innocuous errors after deployment.</span></span>
      >

   <span data-ttu-id="18334-201">c.</span><span class="sxs-lookup"><span data-stu-id="18334-201">c.</span></span> <span data-ttu-id="18334-202">**連接埠設定**：輸入 Docker 容器的唯一 TCP 通訊埠繫結。</span><span class="sxs-lookup"><span data-stu-id="18334-202">**Port settings**: Enter the unique TCP port binding for your Docker container.</span></span>

      ![[設定要建立的 Docker 容器] 視窗][PUB08]

1. <span data-ttu-id="18334-204">完成上述所有步驟之後，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="18334-204">After you have completed all of the preceding steps, click **Finish**.</span></span>

<span data-ttu-id="18334-205">Azure 工具組會開始將您的 Web 應用程式部署至 Azure 中的 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="18334-205">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="18334-206">後續步驟</span><span class="sxs-lookup"><span data-stu-id="18334-206">Next steps</span></span>

<span data-ttu-id="18334-207">如需 Docker 的其他資源，請參閱 [Docker 網站]。</span><span class="sxs-lookup"><span data-stu-id="18334-207">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Docker 網站]: https://www.docker.com/

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png