---
title: 將 Java 型 MicroProfile 服務部署至用於容器的 Azure Web App
description: 了解如何使用 Docker 和用於容器的 Azure Web App 來部署 MicroProfile 服務
services: container-registry;app-service
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: container-registry;app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 4ecfbf92bc30bc491c991e30111cce8375da7f1a
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533594"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a><span data-ttu-id="0c84e-103">將 Java 型 MicroProfile 服務部署至用於容器的 Azure Web App</span><span class="sxs-lookup"><span data-stu-id="0c84e-103">Deploy a Java-based MicroProfile service to Azure Web App for Containers</span></span>

<span data-ttu-id="0c84e-104">MicroProfile 非常適合用來建置小型 Java 應用程式，以便您快速且輕鬆地將其部署到[用於容器的 Azure Web App](https://azure.microsoft.com/services/app-service/containers/) 等服務。</span><span class="sxs-lookup"><span data-stu-id="0c84e-104">MicroProfile is a great way to build exceedingly small Java applications that you can quickly and easily deploy to services such as [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/).</span></span> <span data-ttu-id="0c84e-105">在本教學課程中，您將建立簡單的 MicroProfile 型微服務，而此微服務接著會容器化為 Docker 容器，並部署到 [Azure Container Registry 執行個體](https://azure.microsoft.com/services/container-registry/)，然後使用用於容器的 Azure Web App 進行裝載。</span><span class="sxs-lookup"><span data-stu-id="0c84e-105">In this tutorial, you create a simple, MicroProfile-based microservice that you containerize into a Docker container, deploy to an [Azure container registry instance](https://azure.microsoft.com/services/container-registry/), and then host by using Azure Web App for Containers.</span></span>

> [!NOTE]
> <span data-ttu-id="0c84e-106">只要 Docker 容器映像是可自動執行的 (也就是包含執行階段的映像)，此程序就能與 MicroProfile.io 的任何實作搭配使用。</span><span class="sxs-lookup"><span data-stu-id="0c84e-106">This procedure works with any implementation of MicroProfile.io, as long the Docker container image is self-executable (that is, the image includes the runtime).</span></span>

<span data-ttu-id="0c84e-107">在此範例中，您會使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile 1.3](https://microprofile.io/) 建立小型 (大約 5,000 個位元組) Java Web 應用程式需求 (WAR) 檔案，然後將其封裝成大約 174 MB 的 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="0c84e-107">In this sample, you use [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile 1.3](https://microprofile.io/) to create a small (approximately 5,000 bytes) Java web application requirement (WAR) file, and then package it into a Docker image of approximately 174 megabytes (MB).</span></span> <span data-ttu-id="0c84e-108">Docker 映像包含 Web 應用程式進行完整容器化部署所需的一切。</span><span class="sxs-lookup"><span data-stu-id="0c84e-108">The Docker image contains everything necessary for a fully containerized deployment of the web app.</span></span>

<span data-ttu-id="0c84e-109">當應用程式的原始程式碼有所變更時，並不需要將整個 174 MB 的 Docker 映像重新部署，因為 Docker 只會上傳有差異的部分 (這明顯小很多)。</span><span class="sxs-lookup"><span data-stu-id="0c84e-109">An entire 174 MB Docker image doesn't need to be redeployed whenever the application source code is changed, because Docker uploads only the differences (which are significantly smaller).</span></span> <span data-ttu-id="0c84e-110">因此，透過 CI/CD 管線執行新版 MicroProfile 應用程式的程序會極有效率且快速，並且可減少阻力和啟用快速開發反覆項目。</span><span class="sxs-lookup"><span data-stu-id="0c84e-110">Consequently, the process of executing a new release of a MicroProfile application via a CI/CD pipeline is extremely efficient and quick, reducing friction and enabling rapid development iteration.</span></span>

<span data-ttu-id="0c84e-111">在本教學課程中，您會先在本機建立及執行程式碼，然後將其部署為 Azure 上的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c84e-111">You'll work through this tutorial by first creating and running the code locally and then deploying it as a web app on Azure.</span></span> <span data-ttu-id="0c84e-112">在這兩個案例中，您可以依賴 Docker 來簡化並標準化您的工作。</span><span class="sxs-lookup"><span data-stu-id="0c84e-112">In both phases, you can depend on Docker to simplify and standardize your efforts.</span></span> <span data-ttu-id="0c84e-113">開始之前，您將建立 Azure Container Registry 執行個體用以儲存 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="0c84e-113">Before you begin, you'll create an Azure container registry instance for storing your Docker containers.</span></span>

## <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="0c84e-114">建立 Azure Container Registry 執行個體</span><span class="sxs-lookup"><span data-stu-id="0c84e-114">Create an Azure container registry instance</span></span>

<span data-ttu-id="0c84e-115">您可以使用 [Azure 入口網站](http://portal.azure.com)來建立 Azure Container Registry 執行個體，但您也有其他選擇，例如 Azure CLI。</span><span class="sxs-lookup"><span data-stu-id="0c84e-115">You can use the [Azure portal](http://portal.azure.com) to create the Azure container registry instance, but there are other choices also, such as the Azure CLI.</span></span> <span data-ttu-id="0c84e-116">若要建立新的 Azure Container Registry 執行個體，請執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="0c84e-116">To create a new Azure container registry instance, do the following:</span></span>

1. <span data-ttu-id="0c84e-117">登入 [Azure 入口網站](http://portal.azure.com)並建立新的 Azure Container Registry 資源。</span><span class="sxs-lookup"><span data-stu-id="0c84e-117">Sign in to the [Azure portal](http://portal.azure.com) and create a new Azure container registry resource.</span></span> <span data-ttu-id="0c84e-118">提供登錄名稱 (此名稱應設定為 *pom.xml* 檔案中的 `docker.registry` 屬性)。</span><span class="sxs-lookup"><span data-stu-id="0c84e-118">Provide a registry name (this name should be set as the `docker.registry` property in the *pom.xml* file).</span></span> <span data-ttu-id="0c84e-119">視需要變更預設值，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="0c84e-119">Change the defaults as you want, and then select **Create**.</span></span>

1. <span data-ttu-id="0c84e-120">容器登錄執行個體大約會在 30 秒內啟用，屆時請選取容器登錄執行個體，然後在左窗格中選取 [存取金鑰]  連結。</span><span class="sxs-lookup"><span data-stu-id="0c84e-120">In about 30 seconds, when the container registry instance is live, select the container registry instance and then, in the left pane, select the **Access keys** link.</span></span> 

    <span data-ttu-id="0c84e-121">請務必啟用*管理員使用者*設定，以便您可以從電腦存取此容器登錄執行個體，並將 Docker 容器推送至其中。</span><span class="sxs-lookup"><span data-stu-id="0c84e-121">Be sure to enable the *admin user* setting, so that you can access this container registry instance from your machines and push Docker containers into it.</span></span> <span data-ttu-id="0c84e-122">如此也可讓您從您將即將設定的「用於容器的 Web App」執行個體進行存取。</span><span class="sxs-lookup"><span data-stu-id="0c84e-122">Doing so also enables access from the Azure Web App for Containers instance that you'll set up soon.</span></span>

1. <span data-ttu-id="0c84e-123">在 [存取金鑰]  窗格中複製**使用者名稱**和**密碼**值，並將其貼到全域 Maven *settings.xml* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="0c84e-123">In the **Access keys** pane, copy the **username** and **password** values and paste them into your global Maven *settings.xml* file.</span></span> <span data-ttu-id="0c84e-124">如需 Maven 設定的詳細資訊，請移至 [Apache Maven 專案](https://maven.apache.org/settings.html)網站。</span><span class="sxs-lookup"><span data-stu-id="0c84e-124">For more information about Maven settings, go to the [Apache Maven Project](https://maven.apache.org/settings.html) website.</span></span> 

   <span data-ttu-id="0c84e-125">以下提供 *${user.home}/.m2/settings.xml* 檔案的範例，供您參考：</span><span class="sxs-lookup"><span data-stu-id="0c84e-125">For your reference, here's an example of the *${user.home}/.m2/settings.xml* file:</span></span>

    ```xml
    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
        <servers>
          <server>
            <id>jogilescr.azurecr.io</id>
            <username>jogilescr</username>
            <password>ojoirshois.this-isn't-real.hrihslirhlishrglih</password>
          </server>
        </servers>
    </settings>
    ```

<span data-ttu-id="0c84e-126">現在，您已建立容器登錄執行個體，接下來可以在本機建置和執行您的 MicroProfile 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c84e-126">Now that you've created your container registry instance, you can move on to building and running your MicroProfile application locally.</span></span>

## <a name="create-your-microprofile-application"></a><span data-ttu-id="0c84e-127">建立 MicroProfile 應用程式</span><span class="sxs-lookup"><span data-stu-id="0c84e-127">Create your MicroProfile application</span></span>

<span data-ttu-id="0c84e-128">此範例以 GitHub 上的可用範例應用程式為基礎。</span><span class="sxs-lookup"><span data-stu-id="0c84e-128">This example code is based on a sample application that's available on GitHub.</span></span> <span data-ttu-id="0c84e-129">若要將程式碼複製到您的電腦上，請輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="0c84e-129">To clone the code onto your machine, enter the following commands:</span></span>

```
git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git

cd microprofile-docker-helloworld
```

<span data-ttu-id="0c84e-130">此目錄中包含 *pom.xml* 檔案，它可讓您以 Maven 建置工具所使用的格式來指定專案。</span><span class="sxs-lookup"><span data-stu-id="0c84e-130">This directory contains a *pom.xml* file that you use to specify the project in the format that's used by the Maven build tool.</span></span> <span data-ttu-id="0c84e-131">您可以根據自己的需求來編輯此檔案。</span><span class="sxs-lookup"><span data-stu-id="0c84e-131">You can edit the file to suit your own needs.</span></span> <span data-ttu-id="0c84e-132">特別是，`docker.registry` 和 `docker.name` 屬性應變更為您在設定 Azure Container Registry 執行個體時所建立的 `docker.registry` 和 `docker.name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="0c84e-132">In particular, the `docker.registry` and `docker.name` properties should be changed to the `docker.registry` and `docker.name` properties that were created when you set up the Azure container registry instance.</span></span>

<span data-ttu-id="0c84e-133">此目錄中另一個要注意的檔案是 Dockerfile，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0c84e-133">Another file of note in this directory is the Dockerfile, which is reproduced below:</span></span>

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

<span data-ttu-id="0c84e-134">Dockerfile 會建立以 Payara Micro Docker 容器為基礎的新 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="0c84e-134">The Dockerfile creates a new Docker container that's based on the Payara Micro Docker Container.</span></span> <span data-ttu-id="0c84e-135">它會複製已建立作為組建程序一部分的 WAR 檔案。</span><span class="sxs-lookup"><span data-stu-id="0c84e-135">It copies in the WAR file that was created as part of your build process.</span></span> <span data-ttu-id="0c84e-136">它也會公開連接埠 8080，如此一來，當服務在 Docker 容器內啟動並執行時，您就可以存取該服務。</span><span class="sxs-lookup"><span data-stu-id="0c84e-136">It also exposes port 8080 so that you can access the service after it's up and running within a Docker container.</span></span>

<span data-ttu-id="0c84e-137">當您開啟 *src* 目錄時，您最終將會發現在此處重現的 `Application` 類別：</span><span class="sxs-lookup"><span data-stu-id="0c84e-137">When you open the *src* directory, you'll eventually discover the `Application` class that's reproduced here:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

<span data-ttu-id="0c84e-138">*@ApplicationPath("/api")* 註釋會指定此微服務的基底端點。</span><span class="sxs-lookup"><span data-stu-id="0c84e-138">The *@ApplicationPath("/api")* annotation specifies the base endpoint for this microservice.</span></span> <span data-ttu-id="0c84e-139">也就是說，對於所有的端點，存取任何特定 REST 端點所需的其餘 URL 部分前面都會加上 */api*。</span><span class="sxs-lookup"><span data-stu-id="0c84e-139">That is, for all endpoints, */api* precedes the rest of the URL that's required to access any specific REST endpoint.</span></span>

<span data-ttu-id="0c84e-140">*api* 套件內會有名為 `API` 的類別，其中包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0c84e-140">Inside the *api* package is a class named `API`, which contains the following code:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld.api;

import javax.enterprise.context.ApplicationScoped;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;

import static javax.ws.rs.core.MediaType.TEXT_HTML;

@ApplicationScoped
@Path("/")
public class API {

    @GET
    @Path("/helloworld")
    @Produces(TEXT_HTML)
    public String info() {
        return "Hello, world!";
    }
}
```

<span data-ttu-id="0c84e-141">藉由使用 *@Path("/helloworld")* 註釋，您可以看到，此 REST 端點在與指定於 `Application` 類別中的 */api* 結合時，將會是 */api/helloworld*。</span><span class="sxs-lookup"><span data-stu-id="0c84e-141">Through the use of the *@Path("/helloworld")* annotation, you can see that this REST endpoint, when it's combined with the */api* that's specified in the `Application` class, will be */api/helloworld*.</span></span> <span data-ttu-id="0c84e-142">使用 HTTP GET 要求來呼叫此端點時，您可以看到此方法會產生文字/html，這事實上就是硬式編碼的 "Hello, world!" 字串。</span><span class="sxs-lookup"><span data-stu-id="0c84e-142">When you call this endpoint by using an HTTP GET request, you can see that the method produces text/html and, in fact, it's simply a hard-coded string, "Hello, world!"</span></span>

<span data-ttu-id="0c84e-143">至此，本文已說明了您要使用 MicroProfile 建立微服務所需的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="0c84e-143">So far, this article has covered all the code that's required for you to create a microservice by using MicroProfile.</span></span> <span data-ttu-id="0c84e-144">現在，您可以使用 Maven 來建置微服務，並將其容器化為 Docker 容器，然後在本機執行；其步驟如下：</span><span class="sxs-lookup"><span data-stu-id="0c84e-144">You can now use Maven to build it, containerize it into a Docker container, and run it locally by doing the following:</span></span>

1. <span data-ttu-id="0c84e-145">執行 `mvn clean package` 並等候它完成。</span><span class="sxs-lookup"><span data-stu-id="0c84e-145">Run `mvn clean package`, and wait until it is complete.</span></span>

1. <span data-ttu-id="0c84e-146">執行 `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`。</span><span class="sxs-lookup"><span data-stu-id="0c84e-146">Run `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`.</span></span> <span data-ttu-id="0c84e-147">例如 *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*，其中， *\<docker.registry>* 是 *jogilescr.azurecr.io*，而 *\<docker.name>* 是 *samples/docker-helloworld*。</span><span class="sxs-lookup"><span data-stu-id="0c84e-147">For example, *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*, where *\<docker.registry>* is *jogilescr.azurecr.io* and *\<docker.name>* is *samples/docker-helloworld*.</span></span>

1. <span data-ttu-id="0c84e-148">嘗試在網頁瀏覽器中存取 [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) 和 [http://localhost:8080/health](http://localhost:8080/health)。</span><span class="sxs-lookup"><span data-stu-id="0c84e-148">Try to access [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) and [http://localhost:8080/health](http://localhost:8080/health) in your web browser.</span></span> <span data-ttu-id="0c84e-149">如果您看到預期的 "Hello, world!"</span><span class="sxs-lookup"><span data-stu-id="0c84e-149">If you see the expected "Hello, world!"</span></span> <span data-ttu-id="0c84e-150">回應 (以及 [/health](http://localhost:8080/health) 端點的健康情況相關資訊)，表示您已成功將 MicroProfile 應用程式部署到本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="0c84e-150">response (and health-related information for the [/health](http://localhost:8080/health) endpoint), you've successfully deployed the MicroProfile application on your local machine.</span></span>

## <a name="push-the-container-to-the-azure-container-registry-instance"></a><span data-ttu-id="0c84e-151">將容器推送至 Azure Container Registry 執行個體</span><span class="sxs-lookup"><span data-stu-id="0c84e-151">Push the container to the Azure container registry instance</span></span>

<span data-ttu-id="0c84e-152">現在，您已在本機電腦上成功建置與執行 MicroProfile 應用程式，請將此容器推送至您的容器登錄執行個體。</span><span class="sxs-lookup"><span data-stu-id="0c84e-152">Now that you've successfully built and run your MicroProfile application on your local machine, push this container to your container registry instance.</span></span> 

> [!NOTE]
> <span data-ttu-id="0c84e-153">雖然本文使用 Azure Container Registry，但您可以使用任何容器登錄執行個體，只要將 *pom.xml* 檔案編輯為指向相關位置即可。</span><span class="sxs-lookup"><span data-stu-id="0c84e-153">Although this article uses an Azure container registry instance, any container registry instance should work, as long as the *pom.xml* file is edited to point to the relevant location.</span></span>

1. <span data-ttu-id="0c84e-154">若要清除、編譯及建立本機 Docker 映像，請執行 `mvn clean package`。</span><span class="sxs-lookup"><span data-stu-id="0c84e-154">To clean, compile, and create a local Docker image, run `mvn clean package`.</span></span>
2. <span data-ttu-id="0c84e-155">若要將容器推送至 Azure Container Registry 執行個體，請執行 `mvn dockerfile:push`。</span><span class="sxs-lookup"><span data-stu-id="0c84e-155">To push the container to the Azure container registry instance, run `mvn dockerfile:push`.</span></span>

<span data-ttu-id="0c84e-156">現在，您已將 Docker 容器映像上傳至 Azure Container Registry 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0c84e-156">You now have your Docker container image uploaded to the Azure container registry instance.</span></span> <span data-ttu-id="0c84e-157">不過，該映像尚未執行。</span><span class="sxs-lookup"><span data-stu-id="0c84e-157">However, it's not yet running.</span></span> <span data-ttu-id="0c84e-158">現在，您必須將其部署至用於容器的 Azure Web App 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0c84e-158">You now have to deploy it to an Azure Web App for Containers instance.</span></span> 

## <a name="create-an-azure-web-app-for-containers-instance"></a><span data-ttu-id="0c84e-159">建立用於容器的 Azure Web App 執行個體</span><span class="sxs-lookup"><span data-stu-id="0c84e-159">Create an Azure Web App for Containers instance</span></span>

1. <span data-ttu-id="0c84e-160">在 [Azure 入口網站](http://portal.azure.com)的左窗格中，選取 [Web + 行動]  ，然後執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="0c84e-160">In the [Azure portal](http://portal.azure.com), in the left pane, select **Web + Mobile**, and then do the following:</span></span>

   <span data-ttu-id="0c84e-161">a.</span><span class="sxs-lookup"><span data-stu-id="0c84e-161">a.</span></span> <span data-ttu-id="0c84e-162">指定名稱。</span><span class="sxs-lookup"><span data-stu-id="0c84e-162">Specify a name.</span></span> <span data-ttu-id="0c84e-163">此名稱會成為 Web 應用程式的公用 URL，因此最好選擇容易記住的名稱。</span><span class="sxs-lookup"><span data-stu-id="0c84e-163">The name will become the public URL of the web app, so it's a good idea to pick a name that you can easily remember.</span></span> <span data-ttu-id="0c84e-164">如有需要，稍後您可以新增自訂網域名稱。</span><span class="sxs-lookup"><span data-stu-id="0c84e-164">You can add a custom domain name later, if you want.</span></span>

   <span data-ttu-id="0c84e-165">b.</span><span class="sxs-lookup"><span data-stu-id="0c84e-165">b.</span></span> <span data-ttu-id="0c84e-166">在 [設定容器]  區段的 [映像來源]  下方選取 [Azure Container Registry]  ，然後在下拉式清單中選取正確的映像。</span><span class="sxs-lookup"><span data-stu-id="0c84e-166">In the **Configure container** section, under **Image source**, select **Azure Container Registry** and then, in the drop-down list, select the correct image.</span></span>

   <span data-ttu-id="0c84e-167">c.</span><span class="sxs-lookup"><span data-stu-id="0c84e-167">c.</span></span> <span data-ttu-id="0c84e-168">您不需要在 [啟動檔案]  欄位中指定值。</span><span class="sxs-lookup"><span data-stu-id="0c84e-168">You don't need to specify a value in the **Startup File** field.</span></span>

1. <span data-ttu-id="0c84e-169">在建立執行個體之後，請加以選取，然後選取 [應用程式設定]  。</span><span class="sxs-lookup"><span data-stu-id="0c84e-169">After you've created the instance, select it, and then select **Application Settings**.</span></span> <span data-ttu-id="0c84e-170">輸入 **WEBSITES_PORT** 作為 [金鑰]  ，並輸入 **8080** 作為 [值]  。</span><span class="sxs-lookup"><span data-stu-id="0c84e-170">For **Key**, enter **WEBSITES_PORT**, and for **Value**, enter **8080**.</span></span> <span data-ttu-id="0c84e-171">這些設定會向 Azure 指出應在容器中公開的連接埠，並將其對應至外部連接埠 80。</span><span class="sxs-lookup"><span data-stu-id="0c84e-171">These settings tell Azure which port to expose in the container and to map it to port 80 externally.</span></span>

1. <span data-ttu-id="0c84e-172">(選擇性) 選取 [Docker 容器]  連結，然後啟用 [持續部署]  。</span><span class="sxs-lookup"><span data-stu-id="0c84e-172">(Optional) Select the **Docker Container** link, and then enable **Continuous Deployment**.</span></span> <span data-ttu-id="0c84e-173">如此一來，每當您更新 Azure Container Registry 執行個體映像時，Docker 容器就會在用於容器的 Azure Web App 執行個體中自動更新。</span><span class="sxs-lookup"><span data-stu-id="0c84e-173">By doing so, whenever you update the Azure container registry instance image, it's automatically updated in the Azure Web App for Containers instance.</span></span>

1. <span data-ttu-id="0c84e-174">現在，您應該能夠在 `http://<appname>.azurewebsites.net/microprofile/api/helloworld` 和 `http://<appname>.azurewebsites.net/health` 上存取 Azure 裝載的執行個體。</span><span class="sxs-lookup"><span data-stu-id="0c84e-174">You should now be able to access the Azure-hosted instances at `http://<appname>.azurewebsites.net/microprofile/api/helloworld` and `http://<appname>.azurewebsites.net/health`.</span></span>

## <a name="summary"></a><span data-ttu-id="0c84e-175">總結</span><span class="sxs-lookup"><span data-stu-id="0c84e-175">Summary</span></span>

<span data-ttu-id="0c84e-176">在本教學課程中，您已逐步建立簡單的 MicroProfile 型微服務，並將其容器化為 Docker 容器，且您已在本機執行微服務並發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="0c84e-176">In this tutorial, you've stepped through the process of creating a simple MicroProfile-based microservice, containerized it into a Docker container, run it locally, and published it to Azure.</span></span> <span data-ttu-id="0c84e-177">您可以擴充微服務，以提供其他有用的功能。</span><span class="sxs-lookup"><span data-stu-id="0c84e-177">You can extend your microservice to provide additional useful functionality.</span></span> <span data-ttu-id="0c84e-178">如需詳細資訊，請移至 [MicroProfile.io](https://microprofile.io/)。</span><span class="sxs-lookup"><span data-stu-id="0c84e-178">For more information, go to [MicroProfile.io](https://microprofile.io/).</span></span>
