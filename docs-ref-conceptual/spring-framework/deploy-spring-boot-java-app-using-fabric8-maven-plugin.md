---
title: "使用 Fabric8 Maven 外掛程式部署 Spring Boot 應用程式"
description: "本教學課程將會逐步引導您使用適用於 Apache Maven 的 Fabric8 外掛程式在 Microsoft Azure 上部署 Spring Boot 應用程式。"
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: yuwzho;robmcm
ms.openlocfilehash: 6e33c43d3fb4b63cff1f1c7c04cbf9523aa97770
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-spring-boot-app-using-the-fabric8-maven-plugin"></a><span data-ttu-id="6977a-103">使用 Fabric8 Maven 外掛程式部署 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="6977a-103">Deploy a Spring Boot app using the Fabric8 Maven Plugin</span></span>

<span data-ttu-id="6977a-104">**[Fabric8]** 是一個以 **[Kubernetes]** 為基礎建置的開放原始碼解決方案，它可協助開發人員在 Linux 容器中建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="6977a-104">**[Fabric8]** is an open-source solution that is built on **[Kubernetes]**, which helps developers create applications in Linux containers.</span></span>

<span data-ttu-id="6977a-105">本教學課程會逐步引導您使用適用於 Maven 的 Fabric8 外掛程式來開發應用程式以部署至 [Azure Container Service (AKS)] 中的 Linux 主機。</span><span class="sxs-lookup"><span data-stu-id="6977a-105">This tutorial walks you through using the Fabric8 plugin for Maven to develop to deploy an application to a Linux host in the [Azure Container Service (AKS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6977a-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="6977a-106">Prerequisites</span></span>

<span data-ttu-id="6977a-107">若要完成本教學課程中的步驟，您必須具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="6977a-107">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="6977a-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="6977a-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="6977a-109">[Azure 命令列介面 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="6977a-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="6977a-110">最新的 [Java 開發工具組 (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="6977a-110">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="6977a-111">Apache 的 [Maven] 建置工具 (第 3 版)。</span><span class="sxs-lookup"><span data-stu-id="6977a-111">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="6977a-112">[Git] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="6977a-112">A [Git] client.</span></span>
* <span data-ttu-id="6977a-113">[Docker] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="6977a-113">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="6977a-114">由於本教學課程的虛擬化需求，您無法遵循本文中關於虛擬機器的步驟；您必須在啟用虛擬化功能的情況下使用實體電腦。</span><span class="sxs-lookup"><span data-stu-id="6977a-114">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="6977a-115">建立 Spring Boot on Docker Getting Started Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="6977a-115">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="6977a-116">下列步驟會引導您建置 Spring Boot Web 應用程式，並在本機加以測試。</span><span class="sxs-lookup"><span data-stu-id="6977a-116">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="6977a-117">開啟命令提示字元並建立本機目錄來保存您的應用程式，然後變更至該目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-117">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```shell
   md /home/GenaSoto/SpringBoot
   cd /home/GenaSoto/SpringBoot
   ```
   <span data-ttu-id="6977a-118">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="6977a-118">-- or --</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```

1. <span data-ttu-id="6977a-119">將 [Spring Boot on Docker Getting Started] 範例專案複製到目錄。</span><span class="sxs-lookup"><span data-stu-id="6977a-119">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="6977a-120">將目錄變更至已完成的專案；例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-120">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```
   <span data-ttu-id="6977a-121">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="6977a-121">-- or --</span></span>
   ```shell
   cd gs-spring-boot-docker\complete
   ```

1. <span data-ttu-id="6977a-122">使用 Maven 來建置及執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="6977a-122">Use Maven to build and run the sample app.</span></span>
   ```shell
   mvn clean package spring-boot:run
   ```

1. <span data-ttu-id="6977a-123">瀏覽至 http://localhost:8080 來測試 Web 應用程式，或使用下列 `curl` 命令：</span><span class="sxs-lookup"><span data-stu-id="6977a-123">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="6977a-124">您應該會看到「Hello Docker World」的訊息。</span><span class="sxs-lookup"><span data-stu-id="6977a-124">You should see a **Hello Docker World** message displayed.</span></span>

   ![於本機瀏覽範例應用程式][SB01]


## <a name="install-the-kubernetes-command-line-interface-and-create-an-azure-resource-group-using-the-azure-cli"></a><span data-ttu-id="6977a-126">安裝 Kubernetes 命令列介面，並使用 Azure CLI 建立 Azure 資源群組</span><span class="sxs-lookup"><span data-stu-id="6977a-126">Install the Kubernetes command-line interface and create an Azure resource group using the Azure CLI</span></span>

1. <span data-ttu-id="6977a-127">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="6977a-127">Open a command prompt.</span></span>

1. <span data-ttu-id="6977a-128">輸入下列命令來登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="6977a-128">Type the following command to log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="6977a-129">依照指示完成登入程序</span><span class="sxs-lookup"><span data-stu-id="6977a-129">Follow the instructions to complete the login process</span></span>
   
   <span data-ttu-id="6977a-130">Azure CLI 將會顯示您的帳戶清單，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-130">The Azure CLI will display a list of your accounts; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "00000000-0000-0000-0000-000000000000",
       "isDefault": false,
       "name": "Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "00000000-0000-0000-0000-000000000000",
       "user": {
         "name": "Gena.Soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. <span data-ttu-id="6977a-131">如果您尚未安裝 Kubernetes 命令列介面 (`kubectl`)，您可以使用 Azure CLI 來安裝，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-131">If you do not already have the Kubernetes command-line interface (`kubectl`) installed, you can install using the Azure CLI; for example:</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="6977a-132">Linux 使用者在此命令前可能要加上 `sudo`，因為它會將 Kubernetes CLI 部署到 `/usr/local/bin`。</span><span class="sxs-lookup"><span data-stu-id="6977a-132">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   >
   > <span data-ttu-id="6977a-133">如果您已經安裝了 `kubectl`，但您的 `kubectl` 版本太舊，當您嘗試完成本文章稍後所列出的步驟時，可能會看到類似下列範例的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="6977a-133">If you already have `kubectl`) installed and your version of `kubectl` is too old, you may see an error message similar to the following example when you attempt to complete the steps listed later in this article:</span></span>
   >
   > ```
   > error: group map[autoscaling:0x0000000000 batch:0x0000000000 certificates.k8s.io
   > :0x0000000000 extensions:0x0000000000 storage.k8s.io:0x0000000000 apps:0x0000000
   > 000 authentication.k8s.io:0x0000000000 policy:0x0000000000 rbac.authorization.k8
   > s.io:0x0000000000 federation:0x0000000000 authorization.k8s.io:0x0000000000 comp
   > onentconfig:0x0000000000] is already registered
   > ```
   >
   > <span data-ttu-id="6977a-134">如果發生這種情況，您將需要重新安裝 `kubectl` 以更新您的版本。</span><span class="sxs-lookup"><span data-stu-id="6977a-134">If this happens, you will need to reinstall `kubectl` to update your version.</span></span>
   >

1. <span data-ttu-id="6977a-135">為您將在本教學課程中使用的 Azure 資源建立資源群組，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-135">Create a resource group for the Azure resources that you will use in this tutorial; for example:</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=westeurope
   ```
   <span data-ttu-id="6977a-136">其中：</span><span class="sxs-lookup"><span data-stu-id="6977a-136">Where:</span></span>  
      * <span data-ttu-id="6977a-137">*wingtiptoys-kubernetes* 是您資源群組的唯一名稱</span><span class="sxs-lookup"><span data-stu-id="6977a-137">*wingtiptoys-kubernetes* is a unique name for your resource group</span></span>  
      * <span data-ttu-id="6977a-138">*westeurope* 是您應用程式的適當地理位置</span><span class="sxs-lookup"><span data-stu-id="6977a-138">*westeurope* is an appropriate geographic location for your application</span></span>  

   <span data-ttu-id="6977a-139">Azure CLI 將會顯示建立資源群組的結果，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-139">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes",
     "location": "westeurope",
     "managedBy": null,
     "name": "wingtiptoys-kubernetes",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```


## <a name="create-a-kubernetes-cluster-using-the-azure-cli"></a><span data-ttu-id="6977a-140">使用 Azure CLI 建立 Kubernetes 叢集</span><span class="sxs-lookup"><span data-stu-id="6977a-140">Create a Kubernetes cluster using the Azure CLI</span></span>

1. <span data-ttu-id="6977a-141">在您的新資源群組中建立 Kubernetes 叢集，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-141">Create a Kubernetes cluster in your new resource group; for example:</span></span>  
   ```azurecli 
   az acs create --orchestrator-type kubernetes --resource-group wingtiptoys-kubernetes --name wingtiptoys-cluster --generate-ssh-keys --dns-prefix=wingtiptoys
   ```
   <span data-ttu-id="6977a-142">其中：</span><span class="sxs-lookup"><span data-stu-id="6977a-142">Where:</span></span>  
      * <span data-ttu-id="6977a-143">*wingtiptoys-kubernetes* 是本文先前所提及之您的資源群組名稱</span><span class="sxs-lookup"><span data-stu-id="6977a-143">*wingtiptoys-kubernetes* is the name of your resource group from earlier in this article</span></span>  
      * <span data-ttu-id="6977a-144">*wingtiptoys-cluster* 是您 Kubernetes 叢集的唯一名稱</span><span class="sxs-lookup"><span data-stu-id="6977a-144">*wingtiptoys-cluster* is a unique name for your Kubernetes cluster</span></span>
      * <span data-ttu-id="6977a-145">*wingtiptoys* 是您應用程式 DNS 名稱的唯一名稱</span><span class="sxs-lookup"><span data-stu-id="6977a-145">*wingtiptoys* is a unique name DNS name for your application</span></span>

   <span data-ttu-id="6977a-146">Azure CLI 將會顯示建立叢集的結果，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-146">The Azure CLI will display the results of your cluster creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.Resources/deployments/azurecli0000000000.00000000000",
     "name": "azurecli0000000000.00000000000",
     "properties": {
       "correlationId": "00000000-0000-0000-0000-000000000000",
       "debugSetting": null,
       "dependencies": [],
       "mode": "Incremental",
       "outputs": {
         "masterFQDN": {
           "type": "String",
           "value": "wingtiptoysmgmt.westeurope.cloudapp.azure.com"
         },
         "sshMaster0": {
           "type": "String",
           "value": "ssh azureuser@wingtiptoysmgmt.westeurope.cloudapp.azure.com -A -p 22"
         }
       },
       "parameters": {
         "clientSecret": {
           "type": "SecureString"
         }
       },
       "parametersLink": null,
       "providers": [
         {
           "id": null,
           "namespace": "Microsoft.ContainerService",
           "registrationState": null,
           "resourceTypes": [
             {
               "aliases": null,
               "apiVersions": null,
               "locations": [
                 "westeurope"
               ],
               "properties": null,
               "resourceType": "containerServices"
             }
           ]
         }
       ],
       "provisioningState": "Succeeded",
       "template": null,
       "templateLink": null,
       "timestamp": "2017-09-15T01:00:00.000000+00:00"
     },
     "resourceGroup": "wingtiptoys-kubernetes"
   }
   ```

1. <span data-ttu-id="6977a-147">下載您針對新 Kubernetes 叢集的認證，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-147">Download your credentials for your new Kubernetes cluster; for example:</span></span>  
   ```azurecli 
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes --name wingtiptoys-cluster
   ```

1. <span data-ttu-id="6977a-148">使用下列命令驗證您的連線：</span><span class="sxs-lookup"><span data-stu-id="6977a-148">Verify your connection with the following command:</span></span>
   ```shell 
   kubectl get nodes
   ```

   <span data-ttu-id="6977a-149">您應該會看到一份節點和狀態的清單，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="6977a-149">You should see a list of nodes and statuses like the following example:</span></span>

   ```shell
   NAME                    STATUS                     AGE       VERSION
   k8s-agent-00000000-0    Ready                      5h        v1.6.6
   k8s-agent-00000000-1    Ready                      5h        v1.6.6
   k8s-agent-00000000-2    Ready                      5h        v1.6.6
   k8s-master-00000000-0   Ready,SchedulingDisabled   5h        v1.6.6
   ```


## <a name="create-a-private-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="6977a-150">使用 Azure CLI 建立私人 Azure 容器登錄</span><span class="sxs-lookup"><span data-stu-id="6977a-150">Create a private Azure container registry using the Azure CLI</span></span>

1. <span data-ttu-id="6977a-151">在您的資源群組中建立私人 Azure 容器登錄來裝載您的 Docker 映像，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-151">Create a private Azure container registry in your resource group to host your Docker image; for example:</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes --location westeurope --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="6977a-152">其中：</span><span class="sxs-lookup"><span data-stu-id="6977a-152">Where:</span></span>  
      * <span data-ttu-id="6977a-153">*wingtiptoys-kubernetes* 是本文先前所提及之您的資源群組名稱</span><span class="sxs-lookup"><span data-stu-id="6977a-153">*wingtiptoys-kubernetes* is the name of your resource group from earlier in this article</span></span>  
      * <span data-ttu-id="6977a-154">*wingtiptoysregistry* 是您私人登錄的唯一名稱</span><span class="sxs-lookup"><span data-stu-id="6977a-154">*wingtiptoysregistry* is a unique name for your private registry</span></span>
      * <span data-ttu-id="6977a-155">*westeurope* 是您應用程式的適當地理位置</span><span class="sxs-lookup"><span data-stu-id="6977a-155">*westeurope* is an appropriate geographic location for your application</span></span>  

   <span data-ttu-id="6977a-156">Azure CLI 將會顯示建立登錄的結果，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-156">The Azure CLI will display the results of your registry creation; for example:</span></span>  

   ```json
   {
     "adminUserEnabled": true,
     "creationDate": "2017-09-15T01:00:00.000000+00:00",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.ContainerRegistry/registries/wingtiptoysregistry",
     "location": "westeurope",
     "loginServer": "wingtiptoysregistry.azurecr.io",
     "name": "wingtiptoysregistry",
     "provisioningState": "Succeeded",
     "resourceGroup": "wingtiptoys-kubernetes",
     "sku": {
       "name": "Basic",
       "tier": "Basic"
     },
     "storageAccount": {
       "name": "wingtiptoysregistr000000"
     },
     "tags": {},
     "type": "Microsoft.ContainerRegistry/registries"
   }
   ```

1. <span data-ttu-id="6977a-157">從 Azure CLI 擷取容器登錄的密碼。</span><span class="sxs-lookup"><span data-stu-id="6977a-157">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   <span data-ttu-id="6977a-158">Azure CLI 將會顯示登錄的密碼，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-158">The Azure CLI will display the password for your registry; for example:</span></span>  

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="6977a-159">巡覽至您 Maven 安裝的設定目錄 (預設為 ~/.m2/ 或 C:\Users\使用者名稱\.m2)，並使用文字編輯器開啟 *settings.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6977a-159">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="6977a-160">將您的 Azure Container Registry URL、使用者名稱和密碼新增至 *settings.xml* 檔案的新 `<server>` 集合中。</span><span class="sxs-lookup"><span data-stu-id="6977a-160">Add your Azure Container Registry URL, username and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="6977a-161">`id` 和 `username` 是登錄的名稱。</span><span class="sxs-lookup"><span data-stu-id="6977a-161">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="6977a-162">使用上一個命令的 `password` 值 (不含引號)。</span><span class="sxs-lookup"><span data-stu-id="6977a-162">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry.azurecr.io</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="6977a-163">瀏覽至 Spring Boot 應用程式的已完成專案目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6977a-163">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="6977a-164">使用 Azure Container Registry 的登入伺服器值來更新 *pom.xml* 檔案中的 `<properties>` 集合。</span><span class="sxs-lookup"><span data-stu-id="6977a-164">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="6977a-165">更新 *pom.xml* 檔案中的 `<plugins>` 集合，以便 `<plugin>` 包含 Azure Container Registry 的登入伺服器位址和登錄名稱。</span><span class="sxs-lookup"><span data-stu-id="6977a-165">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
     <groupId>com.spotify</groupId>
     <artifactId>dockerfile-maven-plugin</artifactId>
     <version>1.3.4</version>
     <configuration>
       <repository>${docker.image.prefix}/${project.artifactId}</repository>
       <serverId>${docker.image.prefix}</serverId>
       <registryUrl>https://${docker.image.prefix}</registryUrl>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="6977a-166">瀏覽至 Spring Boot 應用程式的已完成專案目錄，然後執行下列 Maven 命令來建置 Docker 容器，並將映像推送到您的登錄：</span><span class="sxs-lookup"><span data-stu-id="6977a-166">Navigate to the completed project directory for your Spring Boot application, and run the following Maven command to build the Docker container and push the image to your registry:</span></span>

   ```shell
   mvn package dockerfile:build -DpushImage
   ```

   <span data-ttu-id="6977a-167">Maven 將會顯示建置的結果，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-167">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 38.113 s
   [INFO] Finished at: 2017-09-15T02:00:00-07:00
   [INFO] Final Memory: 47M/338M
   [INFO] ----------------------------------------------------
   ```


## <a name="configure-your-spring-boot-app-to-use-the-fabric8-maven-plugin"></a><span data-ttu-id="6977a-168">設定 Spring Boot 應用程式以使用 Fabric8 Maven 外掛程式</span><span class="sxs-lookup"><span data-stu-id="6977a-168">Configure your Spring Boot app to use the Fabric8 Maven plugin</span></span>

1. <span data-ttu-id="6977a-169">瀏覽至 Spring Boot 應用程式的已完成專案目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6977a-169">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="6977a-170">更新 *pom.xml* 檔案中的 `<plugins>` 集合以新增 Fabric8 Maven 外掛程式：</span><span class="sxs-lookup"><span data-stu-id="6977a-170">Update the `<plugins>` collection in the *pom.xml* file to add the Fabric8 Maven plugin:</span></span>

   ```xml
   <plugin>
     <groupId>io.fabric8</groupId>
     <artifactId>fabric8-maven-plugin</artifactId>
     <version>3.5.30</version>
     <configuration>
       <ignoreServices>false</ignoreServices>
       <registry>${docker.image.prefix}</registry>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="6977a-171">瀏覽至 Spring Boot 應用程式的主要來源目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*" 或 "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete/src/main*")，並建立名稱為 "*fabric8*" 的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="6977a-171">Navigate to the main source directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete/src/main*"), and create a new folder named "*fabric8*".</span></span>

1. <span data-ttu-id="6977a-172">在新的 *fabric8* 資料夾中建立三個 YAML 片段檔案：</span><span class="sxs-lookup"><span data-stu-id="6977a-172">Create three YAML fragment files in the new *fabric8* folder:</span></span>

   <span data-ttu-id="6977a-173">a.</span><span class="sxs-lookup"><span data-stu-id="6977a-173">a.</span></span> <span data-ttu-id="6977a-174">建立一個含有下列內容且名為 **deployment.yml** 的檔案：</span><span class="sxs-lookup"><span data-stu-id="6977a-174">Create a file named **deployment.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
        name: ${project.artifactId}
        labels:
          run: gs-spring-boot-docker
      spec:
        replicas: 1
        selector:
          matchLabels:
            run: gs-spring-boot-docker
        strategy:
          rollingUpdate:
            maxSurge: 1
            maxUnavailable: 1
          type: RollingUpdate
        template:
          metadata:
            labels:
              run: gs-spring-boot-docker
          spec:
            containers:
            - image: ${docker.image.prefix}/${project.artifactId}:latest
              name: gs-spring-boot-docker
              imagePullPolicy: Always
              ports:
              - containerPort: 8080
            imagePullSecrets:
              - name: mysecrets
      ```

   <span data-ttu-id="6977a-175">b.</span><span class="sxs-lookup"><span data-stu-id="6977a-175">b.</span></span> <span data-ttu-id="6977a-176">建立一個含有下列內容且名為 **secrets.yml** 的檔案：</span><span class="sxs-lookup"><span data-stu-id="6977a-176">Create a file named **secrets.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: mysecrets
        namespace: default
        annotations:
          maven.fabric8.io/dockerServerId:  ${docker.image.prefix}
      type: kubernetes.io/dockercfg
      ```

   <span data-ttu-id="6977a-177">c.</span><span class="sxs-lookup"><span data-stu-id="6977a-177">c.</span></span> <span data-ttu-id="6977a-178">建立一個含有下列內容且名為 **service.yml** 的檔案：</span><span class="sxs-lookup"><span data-stu-id="6977a-178">Create a file named **service.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: ${project.artifactId}
        labels:
          expose: "true"
      spec:
        ports:
        - port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

1. <span data-ttu-id="6977a-179">執行下列 Maven 命令以建置 Kubernetes 資源清單檔案：</span><span class="sxs-lookup"><span data-stu-id="6977a-179">Run the following Maven command to build the Kubernetes resource list file:</span></span>

   ```shell
   mvn fabric8:resource
   ```

   <span data-ttu-id="6977a-180">此命令會將 *src/main/fabric8* 資料夾下的所有 Kubernetes 資源 YAML 檔案合併為包含 Kubernetes 資源清單的 YAML 檔案，此檔案可以直接套用到 Kubernetes 叢集，或是匯出至 Helm 圖表。</span><span class="sxs-lookup"><span data-stu-id="6977a-180">This command merges all Kubernetes resource yaml files under the *src/main/fabric8* folder to a YAML file that contains a Kubernetes resource list, which can be applied to Kubernetes cluster directly or export to a helm chart.</span></span>

   <span data-ttu-id="6977a-181">Maven 將會顯示建置的結果，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-181">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 16.744 s
   [INFO] Finished at: 2017-09-15T02:35:00-07:00
   [INFO] Final Memory: 30M/290M
   [INFO] ----------------------------------------------------
   ```

1. <span data-ttu-id="6977a-182">執行下列 Maven 命令以將資源清單檔案套用至您的 Kubernetes 叢集：</span><span class="sxs-lookup"><span data-stu-id="6977a-182">Run the following Maven command to apply the resource list file to your Kubernetes cluster:</span></span>

   ```shell
   mvn fabric8:apply
   ```

   <span data-ttu-id="6977a-183">Maven 將會顯示建置的結果，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-183">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 14.814 s
   [INFO] Finished at: 2017-09-15T02:41:00-07:00
   [INFO] Final Memory: 35M/288M
   [INFO] ----------------------------------------------------
   ```

1. <span data-ttu-id="6977a-184">一旦應用程式部署至叢集，請使用 `kubectl` 應用程式查詢外部 IP 位址，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-184">Once the app is deployed to the cluster, query the external IP address using the `kubectl` application; for example:</span></span>
   ```shell
   kubectl get svc -w
   ```

   <span data-ttu-id="6977a-185">`kubectl` 將會顯示您的內部和外部 IP 位址，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-185">`kubectl` will display your internal and external IP addresses; for example:</span></span>
   
   ```shell
   NAME                    CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
   kubernetes              10.0.0.1     <none>        443/TCP        19h
   gs-spring-boot-docker   10.0.242.8   13.65.196.3   80:31215/TCP   3m
   ```
   
   <span data-ttu-id="6977a-186">您可以使用外部 IP 位址來在網頁瀏覽器中開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="6977a-186">You can use the external IP address to open your application in a web browser.</span></span>

   ![以外部方式瀏覽範例應用程式][SB02]

## <a name="delete-your-kubernetes-cluster"></a><span data-ttu-id="6977a-188">刪除 Kubernetes 叢集</span><span class="sxs-lookup"><span data-stu-id="6977a-188">Delete your Kubernetes cluster</span></span>

<span data-ttu-id="6977a-189">當您不再需要 Kubernetes 叢集時，可以使用 `az group delete` 命令來移除資源群組，這將會移除與其相關聯的所有資源，例如：</span><span class="sxs-lookup"><span data-stu-id="6977a-189">When your Kubernetes cluster is no longer needed, you can use the `az group delete` command to remove the resource group, which will remove all of its related resources; for example:</span></span>

   ```azurecli
   az group delete --name wingtiptoys-kubernetes --yes --no-wait
   ```

## <a name="next-steps"></a><span data-ttu-id="6977a-190">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6977a-190">Next steps</span></span>

<span data-ttu-id="6977a-191">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="6977a-191">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="6977a-192">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6977a-192">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="6977a-193">將 Spring Boot 應用程式部署到 Azure Container Service 中的 Linux</span><span class="sxs-lookup"><span data-stu-id="6977a-193">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)
* [<span data-ttu-id="6977a-194">將 Spring Boot 應用程式部署到 Azure Container Service 中的 Kubernetes 叢集</span><span class="sxs-lookup"><span data-stu-id="6977a-194">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="6977a-195">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="6977a-195">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="6977a-196">如需 Spring Boot on Docker 範例專案的進一步詳細資訊，請參閱 [Spring Boot on Docker Getting Started]。</span><span class="sxs-lookup"><span data-stu-id="6977a-196">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="6977a-197">如需開始使用您自己的 Spring Boot 應用程式的說明，請參閱 **Spring Initializr** (網址為 <https://start.spring.io/> \(英文\))。</span><span class="sxs-lookup"><span data-stu-id="6977a-197">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at <https://start.spring.io/>.</span></span>

<span data-ttu-id="6977a-198">如需開始建立簡單 Spring Boot 應用程式的相關詳細資訊，請參閱 Spring Initializr (網址為 <https://start.spring.io/> \(英文\))。</span><span class="sxs-lookup"><span data-stu-id="6977a-198">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at <https://start.spring.io/>.</span></span>

<span data-ttu-id="6977a-199">如需如何搭配 Azure 使用自訂 Docker 映像的其他範例，請參閱[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]。</span><span class="sxs-lookup"><span data-stu-id="6977a-199">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Fabric8]: https://fabric8.io/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java 開發工具組 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Maven]: http://maven.apache.org/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB02.png
