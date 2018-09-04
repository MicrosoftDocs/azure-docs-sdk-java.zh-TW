---
title: 使用 Docker 與 Azure 將 MicroProfile 應用程式部署到雲端
description: 了解如何使用 Docker 和 Azure 容器執行個體將 MicroProfile 應用程式部署至雲端。
services: container-instances;container-retistry
documentationcenter: java
author: brborges
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: brborges
ms.date: 07/30/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: c6254d11ee1596a23076931c9a2a2370b5f52409
ms.sourcegitcommit: 3d0896f821907278547c283c54b53fbd7f4f30f0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2018
ms.locfileid: "43153857"
---
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a><span data-ttu-id="eb369-103">使用 Docker 與 Azure 將 MicroProfile 應用程式部署到雲端</span><span class="sxs-lookup"><span data-stu-id="eb369-103">Deploy a MicroProfile application to the cloud with Docker and Azure</span></span>

<span data-ttu-id="eb369-104">本文會示範如何將 [MicroProfile.io] 應用程式封裝至 Docker 容器，並在 Azure 容器執行個體上加以執行。</span><span class="sxs-lookup"><span data-stu-id="eb369-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
>
> <span data-ttu-id="eb369-105">只要 Docker 容器映像是可自動執行的 (也就是包含執行階段)，此程序就能與 MicroProfile.io 的任何實作搭配使用。</span><span class="sxs-lookup"><span data-stu-id="eb369-105">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb369-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="eb369-106">Prerequisites</span></span>

<span data-ttu-id="eb369-107">若要完成本教學課程中的步驟，您必須具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="eb369-107">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="eb369-108">Azure 訂用帳戶；如果您沒有 Azure 訂用帳戶，可以註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="eb369-108">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="eb369-109">[Azure 命令列介面 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="eb369-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="eb369-110">最新的 [Java 開發套件] \(JDK\)，1.8 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="eb369-110">An up-to-date [Java Development Kit (JDK)], version 1.8 or later.</span></span>
* <span data-ttu-id="eb369-111">Apache 的 [Maven] 建置工具 (第 3 版以上)。</span><span class="sxs-lookup"><span data-stu-id="eb369-111">Apache's [Maven] build tool (version 3+).</span></span>
* <span data-ttu-id="eb369-112">[Git] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="eb369-112">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="eb369-113">MicroProfile Hello Azure 範例</span><span class="sxs-lookup"><span data-stu-id="eb369-113">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="eb369-114">在本文中，我們將使用 [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) 範例：</span><span class="sxs-lookup"><span data-stu-id="eb369-114">For this article, we will use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample:</span></span>

### <a name="clone-build-and-run-locally"></a><span data-ttu-id="eb369-115">在本機複製、建置並執行</span><span class="sxs-lookup"><span data-stu-id="eb369-115">Clone, build, and run locally</span></span>

```bash
$ git clone https://github.com/Azure-Samples/microprofile-hello-azure.git
$ mvn clean package
...
[INFO] BUILD SUCCESS
...
$ mvn payara-micro:start
...
[2018-07-30T13:34:51.553-0700] [] [INFO] [] [PayaraMicro] [tid: _ThreadID=1 _ThreadName=main] [timeMillis: 1532982891553] [levelValue: 800] Payara Micro  5.182 #badassmicrofish (build 303) ready in 10,304 (ms)
...
```

<span data-ttu-id="eb369-116">您可以藉由呼叫 `curl` 或透過[瀏覽器](http://localhost:8080/api/hello)進行瀏覽，來測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="eb369-116">You can test the application by calling `curl` or visiting through a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a><span data-ttu-id="eb369-117">部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="eb369-117">Deploy to Azure</span></span>

<span data-ttu-id="eb369-118">現在讓我們使用 [Azure 容器執行個體] 和 [Azure Container Registry] 服務來將此應用程式帶入雲端。</span><span class="sxs-lookup"><span data-stu-id="eb369-118">Now let's bring this application to the cloud using [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="eb369-119">建置 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="eb369-119">Build a Docker image</span></span>

<span data-ttu-id="eb369-120">範例專案已提供您可以使用的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="eb369-120">The sample project already provides a Dockerfile you can use.</span></span> <span data-ttu-id="eb369-121">不過，您不需要安裝 Docker，因為我們將使用 Azure Container Registry Build 功能在雲端中建置映像。</span><span class="sxs-lookup"><span data-stu-id="eb369-121">You don't need Docker installed though, as we will use Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="eb369-122">若要建置映像，並讓它能在 Azure 上執行，您必須請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="eb369-122">To build the image and be ready to run on Azure, you will have to follow these steps:</span></span>

1. <span data-ttu-id="eb369-123">使用 Azure CLI 安裝並登入</span><span class="sxs-lookup"><span data-stu-id="eb369-123">Install and log in with Azure CLI</span></span>
1. <span data-ttu-id="eb369-124">建立 Azure 資源群組</span><span class="sxs-lookup"><span data-stu-id="eb369-124">Create an Azure Resource Group</span></span>
1. <span data-ttu-id="eb369-125">建立 Azure Container Registry (ACR)</span><span class="sxs-lookup"><span data-stu-id="eb369-125">Create an Azure Container Registry (ACR)</span></span>
1. <span data-ttu-id="eb369-126">建置 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="eb369-126">Build the Docker image</span></span>
1. <span data-ttu-id="eb369-127">將 Docker 映像發佈到之前建立的 ACR</span><span class="sxs-lookup"><span data-stu-id="eb369-127">Publish the Docker image to the ACR created before</span></span>
1. <span data-ttu-id="eb369-128">(選擇性) 以一個命令建置並發佈至 ACR</span><span class="sxs-lookup"><span data-stu-id="eb369-128">(Optionally) Build and publish to ACR in one command</span></span>


#### <a name="set-up-azure-cli"></a><span data-ttu-id="eb369-129">設定 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="eb369-129">Set up Azure CLI</span></span>

<span data-ttu-id="eb369-130">請確定您有 Azure 訂用帳戶、[已安裝 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)，並且已向您的帳戶進行驗證：</span><span class="sxs-lookup"><span data-stu-id="eb369-130">Make sure you have an Azure subscription, [Azure CLI installed](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you are authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="eb369-131">建立資源群組</span><span class="sxs-lookup"><span data-stu-id="eb369-131">Create a Resource Group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="eb369-132">建立 Azure Container Registry 執行個體</span><span class="sxs-lookup"><span data-stu-id="eb369-132">Create an Azure Container Registry instance</span></span>

<span data-ttu-id="eb369-133">此命令會使用基本名稱和隨機數字來建立應該是全域唯一的容器登錄。</span><span class="sxs-lookup"><span data-stu-id="eb369-133">This command will create a globally unique (hopefully) container registry using a basic name with a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="eb369-134">建置 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="eb369-134">Build the Docker image</span></span>

<span data-ttu-id="eb369-135">雖然您可以使用 Docker 本身在本機輕鬆建立 Docker 映像，但您可以考慮在雲端中建立映像，原因如下：</span><span class="sxs-lookup"><span data-stu-id="eb369-135">While you can easily build the Docker image locally using Docker itself, you may want to consider building it in the Cloud for few reasons:</span></span>

1. <span data-ttu-id="eb369-136">不需要在本機安裝 Docker</span><span class="sxs-lookup"><span data-stu-id="eb369-136">No need to install Docker locally</span></span>
1. <span data-ttu-id="eb369-137">速度更快，因為建置作業會在其他地方進行 (不包含內容上傳時間)</span><span class="sxs-lookup"><span data-stu-id="eb369-137">Much faster since build will happen elsewhere (except for context upload time)</span></span>
1. <span data-ttu-id="eb369-138">雲端中的程序可存取更快速的網際網路，因此下載速度更快</span><span class="sxs-lookup"><span data-stu-id="eb369-138">Process in the Cloud has access to faster Internet, therefore faster downloads</span></span>
1. <span data-ttu-id="eb369-139">映像會直接進入 Container Registry</span><span class="sxs-lookup"><span data-stu-id="eb369-139">Image goes directly into the Container Registry</span></span>

<span data-ttu-id="eb369-140">基於這些原因，我們將使用 [Azure Container Registry Build] 功能建置映像：</span><span class="sxs-lookup"><span data-stu-id="eb369-140">Because of these reasons, we will build the image using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a><span data-ttu-id="eb369-141">將 Docker 映像從 Azure Container Registry (ACR) 部署至容器執行個體 (ACI)</span><span class="sxs-lookup"><span data-stu-id="eb369-141">Deploy Docker Image from Azure Container Registry (ACR) into Container Instances (ACI)</span></span>

<span data-ttu-id="eb369-142">現在，您的 ACR 上已有映像可用，接著讓我們來推送和具現化 ACI 上的容器個體。</span><span class="sxs-lookup"><span data-stu-id="eb369-142">Now that the image is available on your ACR, let's push and instanciate a container instance on ACI.</span></span> <span data-ttu-id="eb369-143">首先，我們要確定我們可以進入 ACR 以進行驗證：</span><span class="sxs-lookup"><span data-stu-id="eb369-143">But first, we need to make sure we can authenticate into the ACR:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="eb369-144">測試已部署的 MicroProfile 應用程式</span><span class="sxs-lookup"><span data-stu-id="eb369-144">Test Your Deployed MicroProfile Application</span></span>

<span data-ttu-id="eb369-145">您的應用程式現在應該已啟動並且在執行中。</span><span class="sxs-lookup"><span data-stu-id="eb369-145">Your application should now be up and running.</span></span> <span data-ttu-id="eb369-146">若要從命令列中進行測試，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="eb369-146">To test it from the command-line, try the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="eb369-147">恭喜！</span><span class="sxs-lookup"><span data-stu-id="eb369-147">Congratulations!</span></span> <span data-ttu-id="eb369-148">您已成功建置 MicroProfile 應用程式，並將其部署為 Microsoft Azure 上的 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="eb369-148">You have successfuly built and deployed a MicroProfile application as a Docker container onto Microsoft Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb369-149">後續步驟</span><span class="sxs-lookup"><span data-stu-id="eb369-149">Next steps</span></span>

<span data-ttu-id="eb369-150">如需本文所討論之各種技術的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="eb369-150">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* [<span data-ttu-id="eb369-151">從 Azure CLI 登入 Azure</span><span class="sxs-lookup"><span data-stu-id="eb369-151">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure Container Registry Build]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
