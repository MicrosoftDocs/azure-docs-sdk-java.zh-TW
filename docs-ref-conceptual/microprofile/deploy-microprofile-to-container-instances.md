---
title: 使用 Docker 與 Azure 將 MicroProfile 應用程式部署至雲端
description: 了解如何使用 Docker 和 Azure 容器執行個體將 MicroProfile 應用程式部署至雲端。
services: container-instances;container-registry
documentationcenter: java
author: brunoborges
manager: routlaw
editor: brunoborges
ms.assetid: ''
ms.author: brborges
ms.date: 11/21/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 6ba12bb183969103676fa988199603df6cf36bba
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533607"
---
# <a name="deploy-a-microprofile-app-to-the-cloud-by-using-docker-and-azure"></a><span data-ttu-id="9cafd-103">使用 Docker 與 Azure 將 MicroProfile 應用程式部署至雲端</span><span class="sxs-lookup"><span data-stu-id="9cafd-103">Deploy a MicroProfile app to the cloud by using Docker and Azure</span></span>

<span data-ttu-id="9cafd-104">本文會示範如何將 [MicroProfile.io] 應用程式封裝至 Docker 容器，並在 Azure 容器執行個體上加以執行。</span><span class="sxs-lookup"><span data-stu-id="9cafd-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
> <span data-ttu-id="9cafd-105">只要 Docker 容器映像是可自動執行的 (也就是包含執行階段的映像)，此程序就能與 MicroProfile.io 的任何實作搭配使用。</span><span class="sxs-lookup"><span data-stu-id="9cafd-105">This procedure works with any implementation of MicroProfile.io, as long as the Docker container image is self-executable (that is, the image includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9cafd-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="9cafd-106">Prerequisites</span></span>

<span data-ttu-id="9cafd-107">若要完成本教學課程，您需要下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="9cafd-107">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="9cafd-108">Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="9cafd-108">An Azure subscription.</span></span> <span data-ttu-id="9cafd-109">如果您還沒有 Azure 訂用帳戶，您可以註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="9cafd-109">If you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="9cafd-110">已安裝 [Azure CLI]。</span><span class="sxs-lookup"><span data-stu-id="9cafd-110">The [Azure CLI], installed.</span></span>
* <span data-ttu-id="9cafd-111">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="9cafd-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="9cafd-112">若要進一步了解您在進行 Azure 開發時有哪些可用的 JDK，請參閱 [Java 對於 Azure 和 Azure Stack 的長期支援](https://aka.ms/azure-jdks)。</span><span class="sxs-lookup"><span data-stu-id="9cafd-112">For more information about the JDKs that are available to use when you develop on Azure, see [Java long-term support for Azure and Azure Stack](https://aka.ms/azure-jdks).</span></span>
* <span data-ttu-id="9cafd-113">[Apache Maven] 建置工具 (第 3 版或更新版本)。</span><span class="sxs-lookup"><span data-stu-id="9cafd-113">The [Apache Maven] build tool (version 3 or later).</span></span>
* <span data-ttu-id="9cafd-114">[Git] 用戶端。</span><span class="sxs-lookup"><span data-stu-id="9cafd-114">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="9cafd-115">MicroProfile Hello Azure 範例</span><span class="sxs-lookup"><span data-stu-id="9cafd-115">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="9cafd-116">在本文中，您將使用 [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) 範例。</span><span class="sxs-lookup"><span data-stu-id="9cafd-116">In this article, you use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample.</span></span> <span data-ttu-id="9cafd-117">使用下列命令在本機加以複製、建置和執行：</span><span class="sxs-lookup"><span data-stu-id="9cafd-117">Clone, build, and run it locally by using the following commands:</span></span>

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

<span data-ttu-id="9cafd-118">您可以藉由呼叫 `curl` 或使用[瀏覽器](http://localhost:8080/api/hello)來測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="9cafd-118">You can test the application by calling `curl` or by using a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="9cafd-119">將應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="9cafd-119">Deploy the app to Azure</span></span>

<span data-ttu-id="9cafd-120">現在，使用 [Azure 容器執行個體]和 [Azure Container Registry] 服務，將此應用程式移至 Azure。</span><span class="sxs-lookup"><span data-stu-id="9cafd-120">Now bring this application to Azure by using the [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="9cafd-121">建置 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="9cafd-121">Build a Docker image</span></span>

<span data-ttu-id="9cafd-122">範例專案提供了您可以使用的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="9cafd-122">The sample project provides a Dockerfile that you can use.</span></span> <span data-ttu-id="9cafd-123">但您不需要安裝 Docker，因為您將使用 Azure Container Registry Build 功能在雲端中建置映像。</span><span class="sxs-lookup"><span data-stu-id="9cafd-123">You don't need to have Docker installed, though, because you'll use the Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="9cafd-124">若要建置映像並準備在 Azure 上加以執行，請執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="9cafd-124">To build the image and prepare to run it on Azure, do the following:</span></span>

1. <span data-ttu-id="9cafd-125">安裝並登入 Azure CLI。</span><span class="sxs-lookup"><span data-stu-id="9cafd-125">Install and sign in to the Azure CLI.</span></span>
1. <span data-ttu-id="9cafd-126">建立 Azure 資源群組。</span><span class="sxs-lookup"><span data-stu-id="9cafd-126">Create an Azure resource group.</span></span>
1. <span data-ttu-id="9cafd-127">建立 Azure Container Registry 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9cafd-127">Create an Azure container registry instance.</span></span>
1. <span data-ttu-id="9cafd-128">建置 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="9cafd-128">Build a Docker image.</span></span>
1. <span data-ttu-id="9cafd-129">將 Docker 映像發佈至先前建立的容器登錄執行個體。</span><span class="sxs-lookup"><span data-stu-id="9cafd-129">Publish the Docker image to the previously created container registry instance.</span></span>
1. <span data-ttu-id="9cafd-130">(選擇性) 在單一命令中建置映像並將其發佈至容器登錄執行個體。</span><span class="sxs-lookup"><span data-stu-id="9cafd-130">(Optional) Build and publish the image to the container registry instance in one command.</span></span>


#### <a name="set-up-the-azure-cli"></a><span data-ttu-id="9cafd-131">設定 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9cafd-131">Set up the Azure CLI</span></span>

<span data-ttu-id="9cafd-132">請確定您有 Azure 訂用帳戶、您已安裝 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)，且您已向帳戶進行驗證：</span><span class="sxs-lookup"><span data-stu-id="9cafd-132">Make sure that you have an Azure subscription, that you've installed [the Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you're authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="9cafd-133">建立資源群組</span><span class="sxs-lookup"><span data-stu-id="9cafd-133">Create a resource group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-a-container-registry-instance"></a><span data-ttu-id="9cafd-134">建立容器登錄執行個體</span><span class="sxs-lookup"><span data-stu-id="9cafd-134">Create a container registry instance</span></span>

<span data-ttu-id="9cafd-135">此命令應會使用基本名稱和隨機數字來建立全域唯一的容器登錄執行個體。</span><span class="sxs-lookup"><span data-stu-id="9cafd-135">This command should create a globally unique container registry instance with a basic name and a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="9cafd-136">建置 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="9cafd-136">Build the Docker image</span></span>

<span data-ttu-id="9cafd-137">雖然您可以使用 Docker 本身在本機輕鬆建立 Docker 映像，但您可以考慮在雲端中建立映像，原因如下：</span><span class="sxs-lookup"><span data-stu-id="9cafd-137">Although you can easily build the Docker image locally by using Docker itself, you might consider building it in the cloud for few reasons:</span></span>

* <span data-ttu-id="9cafd-138">您不需要在本機安裝 Docker。</span><span class="sxs-lookup"><span data-stu-id="9cafd-138">You don't have to install Docker locally.</span></span>
* <span data-ttu-id="9cafd-139">速度會快得多，因為建置作業會在其他地方進行 (內容上傳時間除外)。</span><span class="sxs-lookup"><span data-stu-id="9cafd-139">It's much faster, because the build happens elsewhere (except for context upload time).</span></span>
* <span data-ttu-id="9cafd-140">雲端中的程序可更快速地存取網際網路，因此下載速度更快。</span><span class="sxs-lookup"><span data-stu-id="9cafd-140">The process in the cloud has faster access to the internet and, therefore, faster downloads.</span></span>
* <span data-ttu-id="9cafd-141">映像會直接進入容器登錄執行個體。</span><span class="sxs-lookup"><span data-stu-id="9cafd-141">The image goes directly into the container registry instance.</span></span>

<span data-ttu-id="9cafd-142">基於這些原因，您將使用 [Azure Container Registry Build] 功能來建置映像：</span><span class="sxs-lookup"><span data-stu-id="9cafd-142">For these reasons, you build the image by using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-the-docker-image-from-the-azure-container-registry-instance-to-container-instances"></a><span data-ttu-id="9cafd-143">將 Docker 映像從 Azure Container Registry 執行個體部署至容器執行個體</span><span class="sxs-lookup"><span data-stu-id="9cafd-143">Deploy the Docker image from the Azure container registry instance to Container Instances</span></span>

<span data-ttu-id="9cafd-144">現在，您的容器登錄執行個體上已有可用的映像，接著請推送和具現化容器執行個體上的容器執行個體。</span><span class="sxs-lookup"><span data-stu-id="9cafd-144">Now that the image is available on your container registry instance, push and instantiate a container instance on Container Instances.</span></span> <span data-ttu-id="9cafd-145">但請先確定您可以向容器登錄執行個體進行驗證：</span><span class="sxs-lookup"><span data-stu-id="9cafd-145">But first, make sure that you can authenticate into the container registry instance:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="9cafd-146">測試已部署的 MicroProfile 應用程式</span><span class="sxs-lookup"><span data-stu-id="9cafd-146">Test your deployed MicroProfile application</span></span>

<span data-ttu-id="9cafd-147">您的應用程式現在應該已啟動並且在執行中。</span><span class="sxs-lookup"><span data-stu-id="9cafd-147">Your application should now be up and running.</span></span> <span data-ttu-id="9cafd-148">若要從命令列介面進行測試，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="9cafd-148">To test it from the command-line interface, use the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="9cafd-149">恭喜！</span><span class="sxs-lookup"><span data-stu-id="9cafd-149">Congratulations!</span></span> <span data-ttu-id="9cafd-150">您已成功將 MicroProfile 應用程式建置為 Docker 容器，並將其部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="9cafd-150">You've successfully built a MicroProfile application as a Docker container and deployed it to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9cafd-151">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9cafd-151">Next steps</span></span>

<span data-ttu-id="9cafd-152">如需本文所討論之各種技術的詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="9cafd-152">For more information about the various technologies discussed in this article, see:</span></span>

* [<span data-ttu-id="9cafd-153">從 Azure CLI 登入 Azure</span><span class="sxs-lookup"><span data-stu-id="9cafd-153">Sign in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure Container Registry Build]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure CLI]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Apache Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Azure 容器執行個體]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Instances]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Registry]:  https://docs.microsoft.com/azure/container-registry
