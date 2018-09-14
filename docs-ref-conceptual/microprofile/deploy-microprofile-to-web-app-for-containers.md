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
ms.openlocfilehash: 1eb0e7d7a718a1c106adebbf89011f6e3fa1504e
ms.sourcegitcommit: c2019ba6da6c7c28b17b5a85f89e49bb5e570ba4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040246"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a><span data-ttu-id="4b6bb-103">將 Java 型 MicroProfile 服務部署至用於容器的 Azure Web App</span><span class="sxs-lookup"><span data-stu-id="4b6bb-103">Deploy a Java-based MicroProfile service to Azure Web App for Containers</span></span>

<span data-ttu-id="4b6bb-104">MicroProfile 非常適合用來建置極小的 Java 應用程式，以便您快速且輕鬆地將其部署到[用於容器的 Azure Web App](https://azure.microsoft.com/services/app-service/containers/) 等服務。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-104">MicroProfile is a great way to build exceedingly tiny Java applications that can be quickly and easily deployed to services such as [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/).</span></span> <span data-ttu-id="4b6bb-105">在本教學課程中，我們將建立 MicroProfile 型的簡單微服務，此微服務接著會容器化為 Docker 容器並部署到 [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)，然後使用用於容器的 Azure Web App 進行託管。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-105">In this tutorial we will create a simple MicroProfile-based microservice that is then containerized into a Docker container, deployed into an [Azure Container Registry](https://azure.microsoft.com/services/container-registry/), and then hosted using Azure Web App for Containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="4b6bb-106">只要 Docker 容器映像是可自動執行的 (也就是包含執行階段)，此程序就能與 MicroProfile.io 的任何實作搭配使用。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-106">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

<span data-ttu-id="4b6bb-107">更具體地來說，此範例會使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile 1.3](https://microprofile.io/) 來建立 Java 的小型 war 檔 (在建立者機器上佔 5,085 位元組)，然後將其封裝成 Docker 映像 (大約 174 MB)。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-107">More concretely, this sample makes use of [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile 1.3](https://microprofile.io/) to create a tiny Java war file (5,085 bytes on the authors machine), and then packages it up into a Docker image (which is approximately 174 megabytes).</span></span> <span data-ttu-id="4b6bb-108">此 Docker 映像包含此 Web 應用程式進行完整容器化部署所需的一切。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-108">This Docker image contains everything necessary for a fully-containerised deployment of this webapp.</span></span>

<span data-ttu-id="4b6bb-109">受惠於 Docker 的運作方式，每當原始程式碼有所變更時，您通常不用重新部署整個 174 MB 的 Docker 映像，因為 Docker 只會上傳有差異的部分 (這明顯小很多)。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-109">Because of the way Docker works, it is often the case that the entire 174 megabyte Docker image does not need to be redeployed whenever the application source code is changed, as Docker will only upload the differences (which is significantly smaller).</span></span> <span data-ttu-id="4b6bb-110">這可讓透過 CI/CD 管線執行新版 MicroProfile 應用程式的程序變得極有效率且快速，並且可減少阻力和啟用快速開發反覆項目。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-110">This makes the process of executing a new release of a MicroProfile application via a CI/CD pipeline extremely efficient and quick, reducing friction and enabling rapid development iteration.</span></span>

<span data-ttu-id="4b6bb-111">在本教學課程中，我們會先在本機建立及執行程式碼，然後將此程式碼部署為 Azure 上的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-111">We will work through this tutorial firstly by creating and running the code locally, and then we will deploy this as a web app on Azure.</span></span> <span data-ttu-id="4b6bb-112">在這兩個案例中，我們將依賴 Docker 來簡化並標準化我們的工作。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-112">In both cases we will depend on Docker to simplify and standardize our efforts.</span></span> <span data-ttu-id="4b6bb-113">開始之前，我們將建立 Azure Container Registry 來儲存 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-113">Before we begin, we will create an Azure Container Registry to store our Docker containers in.</span></span>

## <a name="creating-an-azure-container-registry"></a><span data-ttu-id="4b6bb-114">建立 Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="4b6bb-114">Creating an Azure Container Registry</span></span>

<span data-ttu-id="4b6bb-115">我們將使用 [Azure 入口網站](http://portal.azure.com)來建立 Azure Container Registry，但是請注意，您也可以選擇使用 Azure CLI。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-115">We will use the [Azure Portal](http://portal.azure.com) for creating the Azure Container Registry, but note that there are alternate choices such as the Azure CLI.</span></span> <span data-ttu-id="4b6bb-116">請遵循下列步驟來建立新的 Azure Container Registry：</span><span class="sxs-lookup"><span data-stu-id="4b6bb-116">Follow the steps below to create a new Azure Container Registry:</span></span>

1. <span data-ttu-id="4b6bb-117">登入 [Azure 入口網站](http://portal.azure.com)並建立新的 Azure Container Registry 資源。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-117">Log in to the [Azure Portal](http://portal.azure.com) and create a new Azure Container Registry resource.</span></span> <span data-ttu-id="4b6bb-118">提供登錄名稱 (請注意，此名稱會設定為 `pom.xml` 中的 `docker.registry` 屬性)。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-118">Provide a registry name (note that this is the name that should be set as the `docker.registry` property in `pom.xml`).</span></span> <span data-ttu-id="4b6bb-119">如有需要，請變更預設值，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-119">Change the defaults as you wish, and then click 'create'.</span></span>

1. <span data-ttu-id="4b6bb-120">一旦容器登錄可使用時 (按下 [建立] 後約 30 秒)，按一下容器登錄，然後按一下左側功能表區域中的 [存取金鑰] 連結。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-120">Once the container registry is live (which is about 30 seconds after clicking 'create'), click on the container registry, and click on the 'Access keys' link in the left-menu area.</span></span> <span data-ttu-id="4b6bb-121">在這裡，您需要啟用 [管理使用者] 設定，以便從我們的機器存取此容器登錄 (用來將 Docker 容器推送過去)，並也讓用於容器的 Azure Web APP 執行個體 (即將設定) 可進行存取。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-121">In here, you need to enable the 'admin user' setting, so that this container registry can be accessed from our machines (to push docker containers into), and also to enable access from the Azure Web Apps for Containers instance we will setup soon.</span></span>

1. <span data-ttu-id="4b6bb-122">當您在 [存取金鑰] 區域中的時候，請記下 `username` 和 `password`值。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-122">Whilst you are in the 'Access keys' area, note the `username` and `password` values.</span></span> <span data-ttu-id="4b6bb-123">我們會將這些值複製/貼上到我們的全域 Maven `settings.xml` 檔案 (如需有關 Maven 設定的詳細資訊，請參閱 [Apache Maven 專案](https://maven.apache.org/settings.html)網站)。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-123">We will copy / paste these into our global Maven `settings.xml` file  (for more information on Maven settings, refer to the [Apache Maven Project](https://maven.apache.org/settings.html) website).</span></span> <span data-ttu-id="4b6bb-124">如需參考，以下是建立者系統上 `${user.home}/.m2/settings.xml` 檔案的混淆版本：</span><span class="sxs-lookup"><span data-stu-id="4b6bb-124">For reference, here is an obfuscated version of the `${user.home}/.m2/settings.xml` file on the authors system:</span></span>

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

<span data-ttu-id="4b6bb-125">現在，這項作業已完成，我們可以繼續在本機建置及執行 MicroProfile 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-125">Now that this is complete, we can move on with building and running our MicroProfile application locally.</span></span>

## <a name="creating-our-microprofile-application"></a><span data-ttu-id="4b6bb-126">建立 MicroProfile 應用程式</span><span class="sxs-lookup"><span data-stu-id="4b6bb-126">Creating our MicroProfile application</span></span>

<span data-ttu-id="4b6bb-127">此範例會以 GitHub 上的可用範例應用程式為基礎，因此我們會先將其複製，然後逐步執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-127">This example is based on a sample application available on GitHub, so we will clone that and then step through the code.</span></span> <span data-ttu-id="4b6bb-128">請遵循下列步驟將程式碼複製到您的機器上：</span><span class="sxs-lookup"><span data-stu-id="4b6bb-128">Follow the steps below to get the code cloned onto your machine:</span></span>

1. `git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git`
1. `cd microprofile-docker-helloworld`

<span data-ttu-id="4b6bb-129">此目錄中有一個 `pom.xml` 檔案，它的用處是以 Maven 建置工具所使用的格式來指定專案。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-129">In this directory there is a `pom.xml` file that is used to specify the project in the format used by the Maven build tool.</span></span> <span data-ttu-id="4b6bb-130">您可以根據自己的需求來編輯此檔案。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-130">This file can be edited to suit your own needs.</span></span> <span data-ttu-id="4b6bb-131">特別是，應該將 `docker.registry` 和 `docker.name` 屬性變更為建立 Azure Container Registry 時設定的 `docker.registry` 和 `docker.name`。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-131">In particular, the `docker.registry` and `docker.name` properties should be changed to the `docker.registry` and `docker.name` created when the Azure Container Registry was setup.</span></span>

<span data-ttu-id="4b6bb-132">此目錄中另一個要注意的檔案是 Dockerfile，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4b6bb-132">Another file of note in this directory is the Dockerfile, which is reproduced below:</span></span>

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

<span data-ttu-id="4b6bb-133">此 Dockerfile 會直接以 Payara Micro Docker 容器和建置過程中建立的 .war 檔案副本為基礎來建立新的 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-133">This Dockerfile simply creates a new Docker container based on the Payara Micro Docker Container, and copies in the .war file that is created as part of our build process.</span></span> <span data-ttu-id="4b6bb-134">它也會公開連接埠 8080，如此一來，當服務在 Docker 容器內啟動並執行時，我們就可以存取該服務。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-134">It also exposes port 8080 so that we may access the service once it is up and running within a Docker container.</span></span>

<span data-ttu-id="4b6bb-135">深入探索 `src` 目錄，我們最後會發現 `Application` 類別，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4b6bb-135">Diving into the `src` directory, we will eventually discover the `Application` class reproduced below:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

<span data-ttu-id="4b6bb-136">`@ApplicationPath("/api")` 註解會指定此微服務的基本端點 - 也就是，所有端點會在存取特定 REST 端點所需的 URL 部分前方加上 `/api`。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-136">The `@ApplicationPath("/api")` annotation specifies the base endpoint for this microservice - that is, that all endpoints will have `/api` preceed the rest of the URL required to access any specific REST endpoint.</span></span>

<span data-ttu-id="4b6bb-137">在 `api` 套件內的類別名為 `API`，其中包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="4b6bb-137">Inside the `api` package is a class named `API`, which contains the following code:</span></span>

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

<span data-ttu-id="4b6bb-138">透過使用 `@Path("/helloworld")` 註解，我們可以看到，此 REST 端點結合 `Application` 類別中指定的 `/api` 時會是 `/api/helloworld`。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-138">Through the use of the `@Path("/helloworld")` annotation, we can see that this REST endpoint, when combined with the `/api` specified in the `Application` class, will be `/api/helloworld`.</span></span> <span data-ttu-id="4b6bb-139">使用 HTTP GET 要求來呼叫此端點時，我們可以看到此方法會產生文字/html，事實上就是硬式編碼的 "Hello, world!" 字串。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-139">When this endpoint is called using an HTTP GET request, we can see that the method will produce text/html, and in fact it is simply a hard-coded string "Hello, world!".</span></span>

<span data-ttu-id="4b6bb-140">現在，我們已具有使用 MicroProfile 建立微服務所需的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-140">We have now covered all the code required to create a microservice using MicroProfile.</span></span> <span data-ttu-id="4b6bb-141">我們現在可以使用 Maven 來建置微服務，並把它容器化為 Docker 容器，然後在本機執行。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-141">We can now use Maven to build it, containerize it into a Docker container, and run it locally.</span></span> <span data-ttu-id="4b6bb-142">我們可以使用下列步驟來執行此操作：</span><span class="sxs-lookup"><span data-stu-id="4b6bb-142">We can do that with the following steps:</span></span>

1. <span data-ttu-id="4b6bb-143">執行 `mvn clean package` 並等候其成功完成。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-143">Run `mvn clean package` and wait until it successfully completes.</span></span>

1. <span data-ttu-id="4b6bb-144">執行 `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`，如果您的 `docker.registry` 是 `jogilescr.azurecr.io`，而 `docker.name` 是 `samples/docker-helloworld`，則執行 `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-144">Run `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`, for example, `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`, if your `docker.registry` is `jogilescr.azurecr.io` and `docker.name` is `samples/docker-helloworld`.</span></span>

1. <span data-ttu-id="4b6bb-145">嘗試在 Web 瀏覽器中存取 [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) 和 [http://localhost:8080/health](http://localhost:8080/health)。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-145">Try accessing [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) and [http://localhost:8080/health](http://localhost:8080/health) in your web browser.</span></span> <span data-ttu-id="4b6bb-146">如果您看到預期的 "Hello, world!"</span><span class="sxs-lookup"><span data-stu-id="4b6bb-146">If you see the expected "Hello, world!"</span></span> <span data-ttu-id="4b6bb-147">回應 (以及 [/health](http://localhost:8080/health) 端點的健康情況相關資訊)，表示您已成功將 MicroProfile 應用程式部署到本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-147">response (and health-related information for the [/health](http://localhost:8080/health) endpoint), you have successfully deployed the MicroProfile application on your local machine.</span></span>

## <a name="pushing-to-the-azure-container-registry"></a><span data-ttu-id="4b6bb-148">發佈至 Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="4b6bb-148">Pushing to the Azure Container Registry</span></span>

<span data-ttu-id="4b6bb-149">現在，我們已在本機電腦上成功建置與執行 MicroProfile 應用程式，下一步就是將此容器推送至容器登錄。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-149">Now that we have successfully built and run our MicroProfile application on our local machine, the next step is to push this container into our container registry.</span></span> <span data-ttu-id="4b6bb-150">在本教學課程中，我們會使用 Azure Container Registry，但您可以使用任何容器登錄 (前提是要將 `pom.xml` 檔案編輯為指向相關位置)。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-150">In this tutorial we are using the Azure Container Registry, but any container registry will work (as long as the `pom.xml` file is edited to point to the relevant location).</span></span>

1. <span data-ttu-id="4b6bb-151">執行 `mvn clean package` 來清除、編譯及建立本機 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-151">Run `mvn clean package` to clean, compile, and create a local docker image.</span></span>
2. <span data-ttu-id="4b6bb-152">執行 `mvn dockerfile:push` 來推送至 Azure Container Registry。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-152">Run `mvn dockerfile:push` to push to the Azure Container Registry.</span></span>

<span data-ttu-id="4b6bb-153">在這個階段中，您已經將 Docker 容器映像上傳至 Azure Container Registry 中，但它尚未執行，因為我們現在必須將其部署到用於容器的 Azure Web App 執行個體。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-153">At this stage you now have your docker container image uploaded to the Azure Container Registry, but it is not yet running as we now have to deploy it into an Azure Web App for Containers instance.</span></span> <span data-ttu-id="4b6bb-154">我們現在要進行此操作。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-154">We will now do that.</span></span>

## <a name="creating-an-azure-web-app-for-containers-instance"></a><span data-ttu-id="4b6bb-155">建立用於容器的 Azure Web App 執行個體</span><span class="sxs-lookup"><span data-stu-id="4b6bb-155">Creating an Azure Web App for Containers instance</span></span>

1. <span data-ttu-id="4b6bb-156">返回 [Azure 入口網站](http://portal.azure.com)並新建用於容器的 Azure Web App 執行個體 (位於功能表中的 [Web + 行動] 標題下方)。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-156">Return to the [Azure Portal](http://portal.azure.com) and create a new Web App for Containers instance (located under the 'Web + Mobile' heading in the menu).</span></span> <span data-ttu-id="4b6bb-157">以下是一些提示：</span><span class="sxs-lookup"><span data-stu-id="4b6bb-157">A few pointers:</span></span>

   1. <span data-ttu-id="4b6bb-158">在此指定的名稱會是 Web 應用程式的公用 URL (但若有需要，之後可以新增自訂網域)，因此最好挑選好記的名稱。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-158">The name you specify here will be the public URL of the web app (although a custom domain can be added later if desired), so it is a good idea to pick a name that you can easily remember.</span></span>

   1. <span data-ttu-id="4b6bb-159">當您進入 [設定容器] 區段時，您可以選取 [Azure Container Registry] 作為 [映像來源]，然後從下拉式清單中選取正確的映像。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-159">When you get to the 'Configure container' section, you can select 'Azure Container Registry' for the 'Image source', and then select the correct image from the drop-down lists.</span></span>

   1. <span data-ttu-id="4b6bb-160">您不需要在 [啟動檔案] 欄位中指定任何值。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-160">You do not need to specify any value in the 'Startup File' field.</span></span>

1. <span data-ttu-id="4b6bb-161">建好執行個體後 (同樣地，這非常快速)，按一下該執行個體，然後按一下 [應用程式設定] 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-161">Once the instance is created (again, it is very quick), click on it and then click on the 'Application Settings' menu item.</span></span> <span data-ttu-id="4b6bb-162">您需要在這裡新增應用程式設定，其中金鑰是 `WEBSITES_PORT`，而值是 `8080`。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-162">In here you need to add a new application setting, where the key is `WEBSITES_PORT` and the value is `8080`.</span></span> <span data-ttu-id="4b6bb-163">這會告訴 Azure 您想要在容器中公開哪個連接埠，並會將其對應至外部連接埠 80。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-163">This tells Azure which port you want to expose in the container, and it will be mapped to port 80 externally.</span></span>

1. <span data-ttu-id="4b6bb-164">(選擇性) 按一下 [Docker 容器] 連結，並啟用 [持續部署]，如此一來，每當您更新 Azure Container Registry 映像時，Docker 容器就會在用於容器的 Azure Web App 執行個體中自動更新。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-164">Optionally, click on the 'Docker Container' link, and enable 'Continuous Deployment', so that whenever you update the Azure Container Registry image it is automatically updated in the Azure Web App for Containers instance.</span></span>

1. <span data-ttu-id="4b6bb-165">您應該能夠在 `http://<appname>.azurewebsites.net/microprofile/api/helloworld` 和 `http://<appname>.azurewebsites.net/health` 上存取 Azure 託管的執行個體。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-165">You should be able to access the Azure-hosted instances at `http://<appname>.azurewebsites.net/microprofile/api/helloworld` and `http://<appname>.azurewebsites.net/health`.</span></span>

## <a name="summary"></a><span data-ttu-id="4b6bb-166">總結</span><span class="sxs-lookup"><span data-stu-id="4b6bb-166">Summary</span></span>

<span data-ttu-id="4b6bb-167">在本教學課程中，我們已逐步建立 MicroProfile 型的簡單微服務，並將其容器化為 Docker 容器，而且我們已在本機執行微服務並發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-167">Through this tutorial we have stepped through the process of creating a simple MicroProfile-based microservice, containerized it into a Docker container, and we have run it locally and published it to Azure.</span></span> <span data-ttu-id="4b6bb-168">雖然擴充微服務以提供更有用的功能不屬於本教學課程範圍，但是網路上可找到各式各樣的 MicroProfile 教學課程和建議，讀者可參閱 [MicroProfile.io](https://microprofile.io/) 以了解更多內容。</span><span class="sxs-lookup"><span data-stu-id="4b6bb-168">Extending our microservice to provide more useful functionality is outside the scope of this tutorial, but there is a wealth of tutorials and advice on MicroProfile on the internet, and readers are encouraged to review [MicroProfile.io](https://microprofile.io/) for more content.</span></span>
