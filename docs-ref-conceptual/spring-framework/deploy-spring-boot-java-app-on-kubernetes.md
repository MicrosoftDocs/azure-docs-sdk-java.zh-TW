---
title: 在 Azure Kubernetes Service 中的 Kubernetes 上部署 Spring Boot 應用程式
description: 本教學課程會逐步引導您將 Spring Boot 應用程式部署為 Microsoft Azure 上之 Kubernetes 叢集的步驟。
services: container-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 42bb030a916cc5aaf1e20242518a0a400b8baa88
ms.sourcegitcommit: f33befab25a66a252b4c91c7aeb1b77cb32821bb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2019
ms.locfileid: "59745156"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a><span data-ttu-id="5d356-103">在 Azure Kubernetes Service 中的 Kubernetes 叢集上部署 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="5d356-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Kubernetes Service</span></span>

<span data-ttu-id="5d356-104">**[Kubernetes]** 和 **[Docker]** 是開放原始碼解決方案，可協助開發人員自動化部署、調整及管理在容器中執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d356-104">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications running in containers.</span></span>

<span data-ttu-id="5d356-105">本教學課程會逐步引導您結合這兩項受歡迎的開放原始碼技術，以開發 Spring Boot 應用程式並把它部署到 Microsoft Azure。</span><span class="sxs-lookup"><span data-stu-id="5d356-105">This tutorial walks you through combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="5d356-106">更具體來說，您使用 *[Spring Boot]* 開發應用程式、用 *[Kubernetes]* 部署容器，以及用 [Azure Kubernetes Service (AKS)] 裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d356-106">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Kubernetes Service (AKS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5d356-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="5d356-107">Prerequisites</span></span>

* <span data-ttu-id="5d356-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="5d356-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="5d356-109">[Azure 命令列介面 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="5d356-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="5d356-110">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="5d356-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="5d356-111">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="5d356-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="5d356-112">Apache 的 [Maven] 建置工具 (第 3 版)。</span><span class="sxs-lookup"><span data-stu-id="5d356-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="5d356-113">[Git] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="5d356-113">A [Git] client.</span></span>
* <span data-ttu-id="5d356-114">[Docker] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="5d356-114">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5d356-115">由於本教學課程的虛擬化需求，您無法遵循本文中關於虛擬機器的步驟；您必須在啟用虛擬化功能的情況下使用實體電腦。</span><span class="sxs-lookup"><span data-stu-id="5d356-115">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="5d356-116">建立 Spring Boot on Docker Getting Started Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="5d356-116">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="5d356-117">下列步驟會引導您建置 Spring Boot Web 應用程式，並在本機加以測試。</span><span class="sxs-lookup"><span data-stu-id="5d356-117">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="5d356-118">開啟命令提示字元並建立本機目錄來保存您的應用程式，然後變更至該目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="5d356-118">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="5d356-119">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="5d356-119">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="5d356-120">將 [Spring Boot on Docker Getting Started] 範例專案複製到目錄。</span><span class="sxs-lookup"><span data-stu-id="5d356-120">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="5d356-121">將目錄變更至已完成的專案。</span><span class="sxs-lookup"><span data-stu-id="5d356-121">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="5d356-122">使用 Maven 來建置及執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d356-122">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="5d356-123">瀏覽至 `http://localhost:8080` 來測試 Web 應用程式，或使用下列 `curl` 命令：</span><span class="sxs-lookup"><span data-stu-id="5d356-123">Test the web app by browsing to `http://localhost:8080`, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="5d356-124">您應該會看到下列訊息：**Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="5d356-124">You should see the following message displayed: **Hello Docker World**</span></span>

   ![在本機瀏覽範例應用程式][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="5d356-126">使用 Azure CLI 建立 Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="5d356-126">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="5d356-127">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="5d356-127">Open a command prompt.</span></span>

1. <span data-ttu-id="5d356-128">登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="5d356-128">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="5d356-129">選擇您的 Azure 訂用帳戶：</span><span class="sxs-lookup"><span data-stu-id="5d356-129">Choose your Azure Subscription:</span></span>
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. <span data-ttu-id="5d356-130">為此教學課程中使用的 Azure 資源建立資源群組。</span><span class="sxs-lookup"><span data-stu-id="5d356-130">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="5d356-131">在資源群組中建立私用的 Azure Container Registry。</span><span class="sxs-lookup"><span data-stu-id="5d356-131">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="5d356-132">本教學課程會在稍後的步驟中，將範例應用程式推送為此登錄的 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="5d356-132">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="5d356-133">以登錄的唯一名稱取代 `wingtiptoysregistry`。</span><span class="sxs-lookup"><span data-stu-id="5d356-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --resource-group wingtiptoys-kubernetes --location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a><span data-ttu-id="5d356-134">透過 Jib 將應用程式推送至容器登錄</span><span class="sxs-lookup"><span data-stu-id="5d356-134">Push your app to the container registry via Jib</span></span>

1. <span data-ttu-id="5d356-135">從 Azure CLI登入您的 Azure Container Registry。</span><span class="sxs-lookup"><span data-stu-id="5d356-135">Log in to your Azure Container Registry from the Azure CLI.</span></span>
   ```azurecli
   # set the default name for Azure Container Registry, otherwise you will need to specify the name in "az acr login"
   az configure --defaults acr=wingtiptoysregistry
   az acr login
   ```

1. <span data-ttu-id="5d356-136">巡覽至 Spring Boot 應用程式已完成的專案目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5d356-136">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="5d356-137">用 Azure Container Registry 的登錄名稱和最新版本的 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) 更新 *pom.xml* 檔案中的 `<properties>` 集合。</span><span class="sxs-lookup"><span data-stu-id="5d356-137">Update the `<properties>` collection in the *pom.xml* file with the registry name for your Azure Container Registry and the latest version of [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin).</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>1.0.2</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="5d356-138">更新 *pom.xml* 檔案中的 `<plugins>` 集合，讓 `<plugin>` 中包含 `jib-maven-plugin`。</span><span class="sxs-lookup"><span data-stu-id="5d356-138">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the `jib-maven-plugin`.</span></span>

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>              
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>                
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="5d356-139">瀏覽至 Spring Boot 應用程式已完成的專案目錄，然後執行下列命令來建置映像，並將映像推送到登錄：</span><span class="sxs-lookup"><span data-stu-id="5d356-139">Navigate to the completed project directory for your Spring Boot application and run the following command to build the image and push the image to the registry:</span></span>

   ```
   mvn compile jib:build
   ```

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a><span data-ttu-id="5d356-140">使用 Azure CLI 在 AKS 上建立 Kubernetes 叢集</span><span class="sxs-lookup"><span data-stu-id="5d356-140">Create a Kubernetes Cluster on AKS using the Azure CLI</span></span>

1. <span data-ttu-id="5d356-141">在 Azure Kubernetes Service 中建立 Kubernetes 叢集。</span><span class="sxs-lookup"><span data-stu-id="5d356-141">Create a Kubernetes cluster in Azure Kubernetes Service.</span></span> <span data-ttu-id="5d356-142">下列命令會在 *wingtiptoys-kubernetes* 資源群組中建立*kubernetes* 叢集，以 *wingtiptoys-akscluster* 為叢集名稱，*wingtiptoys-kubernetes* 為 DNS 前置詞：</span><span class="sxs-lookup"><span data-stu-id="5d356-142">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-akscluster* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   <span data-ttu-id="5d356-143">此命令可能需要一些時間才能完成。</span><span class="sxs-lookup"><span data-stu-id="5d356-143">This command may take a while to complete.</span></span>

1. <span data-ttu-id="5d356-144">當您使用 Azure Container Registry (ACR) 搭配 Azure Kubernetes Service (AKS) 時，需要將 Azure Container Registry 的提取存取權授與 Azure Kubernetes Service。</span><span class="sxs-lookup"><span data-stu-id="5d356-144">When you're using Azure Container Registry (ACR) with Azure Kubernetes Service (AKS), you need to grant Azure Kubernetes Service pull access to Azure Container Registry.</span></span> <span data-ttu-id="5d356-145">Azure 會在您建立 Azure Kubernetes Service 時，建立預設的服務主體。</span><span class="sxs-lookup"><span data-stu-id="5d356-145">Azure creates a default service principal when you are creating Azure Kubernetes Service.</span></span> <span data-ttu-id="5d356-146">在 Bash 或 Powershell 中執行下列指令碼，將 AKS 存取權授與 ACR；[從 Azure Kubernetes Service 對 Azure Container Registry 進行驗證]中另有詳細資訊供您參考。</span><span class="sxs-lookup"><span data-stu-id="5d356-146">Please run the following scripts in bash or Powershell to grant AKS access to ACR, see more details at [Authenticate with Azure Container Registry from Azure Kubernetes Service].</span></span>

```bash
   # Get the id of the service principal configured for AKS
   CLIENT_ID=$(az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv)
   
   # Get the ACR registry resource id
   ACR_ID=$(az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv)
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

  <span data-ttu-id="5d356-147">-- 或 --</span><span class="sxs-lookup"><span data-stu-id="5d356-147">-- or --</span></span>

```PowerShell
   # Get the id of the service principal configured for AKS
   $CLIENT_ID = az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv
   
   # Get the ACR registry resource id
   $ACR_ID = az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

1. <span data-ttu-id="5d356-148">使用 Azure CLI 安裝 `kubectl`。</span><span class="sxs-lookup"><span data-stu-id="5d356-148">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="5d356-149">Linux 使用者在此命令前可能要加上 `sudo`，因為它會將 Kubernetes CLI 部署到 `/usr/local/bin`。</span><span class="sxs-lookup"><span data-stu-id="5d356-149">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az aks install-cli
   ```

1. <span data-ttu-id="5d356-150">下載叢集設定資訊，以便從 Kubernetes Web 介面和 `kubectl` 管理您的叢集。</span><span class="sxs-lookup"><span data-stu-id="5d356-150">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="5d356-151">部署 Kubernetes 叢集的映像</span><span class="sxs-lookup"><span data-stu-id="5d356-151">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="5d356-152">本教學課程使用 `kubectl` 部署應用程式，然後讓您透過 Kubernetes web 介面瀏覽部署。</span><span class="sxs-lookup"><span data-stu-id="5d356-152">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-kubectl"></a><span data-ttu-id="5d356-153">使用 kubectl 部署</span><span class="sxs-lookup"><span data-stu-id="5d356-153">Deploy with kubectl</span></span>

1. <span data-ttu-id="5d356-154">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="5d356-154">Open a command prompt.</span></span>

1. <span data-ttu-id="5d356-155">使用 `kubectl run` 命令在 Kubernetes 叢集中執行容器。</span><span class="sxs-lookup"><span data-stu-id="5d356-155">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="5d356-156">為您在 Kubernetes 中的應用程式提供服務名稱和完整的映像名稱。</span><span class="sxs-lookup"><span data-stu-id="5d356-156">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="5d356-157">例如︰</span><span class="sxs-lookup"><span data-stu-id="5d356-157">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="5d356-158">在這個命令中：</span><span class="sxs-lookup"><span data-stu-id="5d356-158">In this command:</span></span>

   * <span data-ttu-id="5d356-159">容器名稱 `gs-spring-boot-docker` 直接指定在 `run` 命令後面</span><span class="sxs-lookup"><span data-stu-id="5d356-159">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="5d356-160">`--image` 參數指定合併的登入伺服器和映像名稱為 `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span><span class="sxs-lookup"><span data-stu-id="5d356-160">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="5d356-161">使用 `kubectl expose` 命令向外部公開您的 Kubernetes 叢集。</span><span class="sxs-lookup"><span data-stu-id="5d356-161">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="5d356-162">指定您的服務名稱、用來存取應用程式的公開 TCP 通訊埠，以及應用程式接聽的內部目標連接埠。</span><span class="sxs-lookup"><span data-stu-id="5d356-162">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="5d356-163">例如︰</span><span class="sxs-lookup"><span data-stu-id="5d356-163">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="5d356-164">在這個命令中：</span><span class="sxs-lookup"><span data-stu-id="5d356-164">In this command:</span></span>

   * <span data-ttu-id="5d356-165">容器名稱 `gs-spring-boot-docker` 直接指定在 `expose deployment` 命令後面</span><span class="sxs-lookup"><span data-stu-id="5d356-165">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="5d356-166">`--type` 參數指定叢集使用負載平衡器</span><span class="sxs-lookup"><span data-stu-id="5d356-166">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="5d356-167">`--port` 參數指定公開的 TCP 通訊埠 80。</span><span class="sxs-lookup"><span data-stu-id="5d356-167">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="5d356-168">您會在此連接埠存取應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d356-168">You access the app on this port.</span></span>

   * <span data-ttu-id="5d356-169">`--target-port` 參數指定內部 TCP 通訊埠 8080。</span><span class="sxs-lookup"><span data-stu-id="5d356-169">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="5d356-170">負載平衡器會在此連接埠上將要求轉送到您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d356-170">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="5d356-171">一旦應用程式部署至叢集，請查詢外部 IP 位址，並在網頁瀏覽器中開啟它：</span><span class="sxs-lookup"><span data-stu-id="5d356-171">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=default
   ```

   ![在 Azure 上瀏覽範例應用程式][SB02]



### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="5d356-173">使用 Kubernetes Web 介面部署</span><span class="sxs-lookup"><span data-stu-id="5d356-173">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="5d356-174">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="5d356-174">Open a command prompt.</span></span>

1. <span data-ttu-id="5d356-175">在預設瀏覽器中開啟 Kubernetes 叢集的設定網站：</span><span class="sxs-lookup"><span data-stu-id="5d356-175">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. <span data-ttu-id="5d356-176">當 Kubernetes 設定網站在您的瀏覽器中開啟時，請按一下連結以**部署容器化應用程式**：</span><span class="sxs-lookup"><span data-stu-id="5d356-176">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes 設定網站][KB01]

1. <span data-ttu-id="5d356-178">當 [資源建立] 頁面出現時，請指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="5d356-178">When the **Resource Creation** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="5d356-179">a.</span><span class="sxs-lookup"><span data-stu-id="5d356-179">a.</span></span> <span data-ttu-id="5d356-180">選取 [建立應用程式]。</span><span class="sxs-lookup"><span data-stu-id="5d356-180">Select **CREATE AN APP**.</span></span>

   <span data-ttu-id="5d356-181">b.</span><span class="sxs-lookup"><span data-stu-id="5d356-181">b.</span></span> <span data-ttu-id="5d356-182">在 [應用程式名稱] 中輸入您的 Spring Boot 應用程式名稱，例如："*gs-spring-boot-docker*"。</span><span class="sxs-lookup"><span data-stu-id="5d356-182">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="5d356-183">c.</span><span class="sxs-lookup"><span data-stu-id="5d356-183">c.</span></span> <span data-ttu-id="5d356-184">在 [容器映像] 中輸入先前的登入伺服器和容器映像，例如："*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"。</span><span class="sxs-lookup"><span data-stu-id="5d356-184">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="5d356-185">d.</span><span class="sxs-lookup"><span data-stu-id="5d356-185">d.</span></span> <span data-ttu-id="5d356-186">針對 [服務] 選擇 [外部]。</span><span class="sxs-lookup"><span data-stu-id="5d356-186">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="5d356-187">e.</span><span class="sxs-lookup"><span data-stu-id="5d356-187">e.</span></span> <span data-ttu-id="5d356-188">在 [連接埠] 和 [目標連接埠] 文字方塊中指定您的外部和內部連接埠。</span><span class="sxs-lookup"><span data-stu-id="5d356-188">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes 設定網站][KB02]


1. <span data-ttu-id="5d356-190">按一下 [部署] 來部署容器。</span><span class="sxs-lookup"><span data-stu-id="5d356-190">Click **Deploy** to deploy the container.</span></span>

   ![Kubernetes 部署][KB05]

1. <span data-ttu-id="5d356-192">應用程式一經部署，您就會看到 [服務] 底下列出您的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d356-192">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes 服務][KB06]

1. <span data-ttu-id="5d356-194">如果按一下**外部端點**連結，您會看到您的 Spring Boot 應用程式在 Azure 上執行。</span><span class="sxs-lookup"><span data-stu-id="5d356-194">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes 服務][KB07]

   ![在 Azure 上瀏覽範例應用程式][SB02]

## <a name="next-steps"></a><span data-ttu-id="5d356-197">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5d356-197">Next steps</span></span>

<span data-ttu-id="5d356-198">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="5d356-198">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5d356-199">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="5d356-199">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="5d356-200">其他資源</span><span class="sxs-lookup"><span data-stu-id="5d356-200">Additional Resources</span></span>

<span data-ttu-id="5d356-201">如需進一步了解在 Azure 上使用 Spring Boot 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="5d356-201">For more information about using Spring Boot on Azure, see the following article:</span></span>

* [<span data-ttu-id="5d356-202">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5d356-202">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

<span data-ttu-id="5d356-203">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="5d356-203">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="5d356-204">如需使用 Visual Studio Code 將 Java 應用程式部署至 Kubernetes 的詳細資訊，請參閱 [Visual Studio Code Java 教學課程]。</span><span class="sxs-lookup"><span data-stu-id="5d356-204">For more information about deploying a Java application to Kubernetes with Visual Studio Code, see [Visual Studio Code Java Tutorials].</span></span>

<span data-ttu-id="5d356-205">如需 Spring Boot on Docker 範例專案的詳細資訊，請參閱 [Spring Boot on Docker Getting Started]。</span><span class="sxs-lookup"><span data-stu-id="5d356-205">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="5d356-206">下列連結提供建立 Spring Boot 應用程式的其他資訊：</span><span class="sxs-lookup"><span data-stu-id="5d356-206">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="5d356-207">如需建立簡易 Spring Boot 應用程式的詳細資訊，請參閱 Spring Initializr，網址為 https://start.spring.io/。</span><span class="sxs-lookup"><span data-stu-id="5d356-207">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="5d356-208">下列連結提供以 Azure 使用 Kubernetes 的其他資訊：</span><span class="sxs-lookup"><span data-stu-id="5d356-208">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="5d356-209">在 Azure Kubernetes Service 中開始使用 Kubernetes 叢集</span><span class="sxs-lookup"><span data-stu-id="5d356-209">Get started with a Kubernetes cluster in Azure Kubernetes Service</span></span>](/azure/aks/intro-kubernetes)

<span data-ttu-id="5d356-210">如需使用 Kubernetes 命令列介面的詳細資訊，請參閱 **kubectl** 使用者指南，網址為 <https://kubernetes.io/docs/user-guide/kubectl/> 。</span><span class="sxs-lookup"><span data-stu-id="5d356-210">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="5d356-211">Kubernetes 網站有幾篇文章討論在私用登錄中使用映像：</span><span class="sxs-lookup"><span data-stu-id="5d356-211">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="5d356-212">[設定 Pod 的服務帳戶]</span><span class="sxs-lookup"><span data-stu-id="5d356-212">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="5d356-213">[命名空間]</span><span class="sxs-lookup"><span data-stu-id="5d356-213">[Namespaces]</span></span>
* <span data-ttu-id="5d356-214">[從私用登錄提取映像]</span><span class="sxs-lookup"><span data-stu-id="5d356-214">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="5d356-215">如需如何搭配 Azure 使用自訂 Docker 映像的其他範例，請參閱[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]。</span><span class="sxs-lookup"><span data-stu-id="5d356-215">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<span data-ttu-id="5d356-216">如需直接在 Azure Kubernetes Service (AKS) 中搭配 Azure Dev Spaces 反覆執行和偵錯容器的詳細資訊，請參閱[開始透過 Java 使用 Azure Dev Spaces]</span><span class="sxs-lookup"><span data-stu-id="5d356-216">For more information about iteratively running and debugging containers directly in Azure Kubernetes Service (AKS) with Azure Dev Spaces, see [Get started on Azure Dev Spaces with Java]</span></span>

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Kubernetes Service (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[使用 Azure DevOps 和 Java]: /azure/devops/java/
[Working with Azure DevOps and Java]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[設定 Pod 的服務帳戶]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Configuring Service Accounts for Pods]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[命名空間]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[從私用登錄提取映像]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
[Pulling an Image from a Private Registry]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- Newly added -->
[從 Azure Kubernetes Service 對 Azure Container Registry 進行驗證]: /azure/container-registry/container-registry-auth-aks/
[Authenticate with Azure Container Registry from Azure Kubernetes Service]: /azure/container-registry/container-registry-auth-aks/
[Visual Studio Code Java 教學課程]: https://code.visualstudio.com/docs/java/java-kubernetes/
[Visual Studio Code Java Tutorials]: https://code.visualstudio.com/docs/java/java-kubernetes/
[開始透過 Java 使用 Azure Dev Spaces]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
[Get started on Azure Dev Spaces with Java]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB07.png
