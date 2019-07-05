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
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a>將 Java 型 MicroProfile 服務部署至用於容器的 Azure Web App

MicroProfile 非常適合用來建置小型 Java 應用程式，以便您快速且輕鬆地將其部署到[用於容器的 Azure Web App](https://azure.microsoft.com/services/app-service/containers/) 等服務。 在本教學課程中，您將建立簡單的 MicroProfile 型微服務，而此微服務接著會容器化為 Docker 容器，並部署到 [Azure Container Registry 執行個體](https://azure.microsoft.com/services/container-registry/)，然後使用用於容器的 Azure Web App 進行裝載。

> [!NOTE]
> 只要 Docker 容器映像是可自動執行的 (也就是包含執行階段的映像)，此程序就能與 MicroProfile.io 的任何實作搭配使用。

在此範例中，您會使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile 1.3](https://microprofile.io/) 建立小型 (大約 5,000 個位元組) Java Web 應用程式需求 (WAR) 檔案，然後將其封裝成大約 174 MB 的 Docker 映像。 Docker 映像包含 Web 應用程式進行完整容器化部署所需的一切。

當應用程式的原始程式碼有所變更時，並不需要將整個 174 MB 的 Docker 映像重新部署，因為 Docker 只會上傳有差異的部分 (這明顯小很多)。 因此，透過 CI/CD 管線執行新版 MicroProfile 應用程式的程序會極有效率且快速，並且可減少阻力和啟用快速開發反覆項目。

在本教學課程中，您會先在本機建立及執行程式碼，然後將其部署為 Azure 上的 Web 應用程式。 在這兩個案例中，您可以依賴 Docker 來簡化並標準化您的工作。 開始之前，您將建立 Azure Container Registry 執行個體用以儲存 Docker 容器。

## <a name="create-an-azure-container-registry-instance"></a>建立 Azure Container Registry 執行個體

您可以使用 [Azure 入口網站](http://portal.azure.com)來建立 Azure Container Registry 執行個體，但您也有其他選擇，例如 Azure CLI。 若要建立新的 Azure Container Registry 執行個體，請執行下列作業：

1. 登入 [Azure 入口網站](http://portal.azure.com)並建立新的 Azure Container Registry 資源。 提供登錄名稱 (此名稱應設定為 *pom.xml* 檔案中的 `docker.registry` 屬性)。 視需要變更預設值，然後選取 [建立]  。

1. 容器登錄執行個體大約會在 30 秒內啟用，屆時請選取容器登錄執行個體，然後在左窗格中選取 [存取金鑰]  連結。 

    請務必啟用*管理員使用者*設定，以便您可以從電腦存取此容器登錄執行個體，並將 Docker 容器推送至其中。 如此也可讓您從您將即將設定的「用於容器的 Web App」執行個體進行存取。

1. 在 [存取金鑰]  窗格中複製**使用者名稱**和**密碼**值，並將其貼到全域 Maven *settings.xml* 檔案中。 如需 Maven 設定的詳細資訊，請移至 [Apache Maven 專案](https://maven.apache.org/settings.html)網站。 

   以下提供 *${user.home}/.m2/settings.xml* 檔案的範例，供您參考：

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

現在，您已建立容器登錄執行個體，接下來可以在本機建置和執行您的 MicroProfile 應用程式。

## <a name="create-your-microprofile-application"></a>建立 MicroProfile 應用程式

此範例以 GitHub 上的可用範例應用程式為基礎。 若要將程式碼複製到您的電腦上，請輸入下列命令：

```
git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git

cd microprofile-docker-helloworld
```

此目錄中包含 *pom.xml* 檔案，它可讓您以 Maven 建置工具所使用的格式來指定專案。 您可以根據自己的需求來編輯此檔案。 特別是，`docker.registry` 和 `docker.name` 屬性應變更為您在設定 Azure Container Registry 執行個體時所建立的 `docker.registry` 和 `docker.name` 屬性。

此目錄中另一個要注意的檔案是 Dockerfile，如下所示：

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

Dockerfile 會建立以 Payara Micro Docker 容器為基礎的新 Docker 容器。 它會複製已建立作為組建程序一部分的 WAR 檔案。 它也會公開連接埠 8080，如此一來，當服務在 Docker 容器內啟動並執行時，您就可以存取該服務。

當您開啟 *src* 目錄時，您最終將會發現在此處重現的 `Application` 類別：

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

*@ApplicationPath("/api")* 註釋會指定此微服務的基底端點。 也就是說，對於所有的端點，存取任何特定 REST 端點所需的其餘 URL 部分前面都會加上 */api*。

*api* 套件內會有名為 `API` 的類別，其中包含下列程式碼：

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

藉由使用 *@Path("/helloworld")* 註釋，您可以看到，此 REST 端點在與指定於 `Application` 類別中的 */api* 結合時，將會是 */api/helloworld*。 使用 HTTP GET 要求來呼叫此端點時，您可以看到此方法會產生文字/html，這事實上就是硬式編碼的 "Hello, world!" 字串。

至此，本文已說明了您要使用 MicroProfile 建立微服務所需的所有程式碼。 現在，您可以使用 Maven 來建置微服務，並將其容器化為 Docker 容器，然後在本機執行；其步驟如下：

1. 執行 `mvn clean package` 並等候它完成。

1. 執行 `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`。 例如 *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*，其中， *\<docker.registry>* 是 *jogilescr.azurecr.io*，而 *\<docker.name>* 是 *samples/docker-helloworld*。

1. 嘗試在網頁瀏覽器中存取 [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) 和 [http://localhost:8080/health](http://localhost:8080/health)。 如果您看到預期的 "Hello, world!" 回應 (以及 [/health](http://localhost:8080/health) 端點的健康情況相關資訊)，表示您已成功將 MicroProfile 應用程式部署到本機電腦上。

## <a name="push-the-container-to-the-azure-container-registry-instance"></a>將容器推送至 Azure Container Registry 執行個體

現在，您已在本機電腦上成功建置與執行 MicroProfile 應用程式，請將此容器推送至您的容器登錄執行個體。 

> [!NOTE]
> 雖然本文使用 Azure Container Registry，但您可以使用任何容器登錄執行個體，只要將 *pom.xml* 檔案編輯為指向相關位置即可。

1. 若要清除、編譯及建立本機 Docker 映像，請執行 `mvn clean package`。
2. 若要將容器推送至 Azure Container Registry 執行個體，請執行 `mvn dockerfile:push`。

現在，您已將 Docker 容器映像上傳至 Azure Container Registry 執行個體。 不過，該映像尚未執行。 現在，您必須將其部署至用於容器的 Azure Web App 執行個體。 

## <a name="create-an-azure-web-app-for-containers-instance"></a>建立用於容器的 Azure Web App 執行個體

1. 在 [Azure 入口網站](http://portal.azure.com)的左窗格中，選取 [Web + 行動]  ，然後執行下列作業：

   a. 指定名稱。 此名稱會成為 Web 應用程式的公用 URL，因此最好選擇容易記住的名稱。 如有需要，稍後您可以新增自訂網域名稱。

   b. 在 [設定容器]  區段的 [映像來源]  下方選取 [Azure Container Registry]  ，然後在下拉式清單中選取正確的映像。

   c. 您不需要在 [啟動檔案]  欄位中指定值。

1. 在建立執行個體之後，請加以選取，然後選取 [應用程式設定]  。 輸入 **WEBSITES_PORT** 作為 [金鑰]  ，並輸入 **8080** 作為 [值]  。 這些設定會向 Azure 指出應在容器中公開的連接埠，並將其對應至外部連接埠 80。

1. (選擇性) 選取 [Docker 容器]  連結，然後啟用 [持續部署]  。 如此一來，每當您更新 Azure Container Registry 執行個體映像時，Docker 容器就會在用於容器的 Azure Web App 執行個體中自動更新。

1. 現在，您應該能夠在 `http://<appname>.azurewebsites.net/microprofile/api/helloworld` 和 `http://<appname>.azurewebsites.net/health` 上存取 Azure 裝載的執行個體。

## <a name="summary"></a>總結

在本教學課程中，您已逐步建立簡單的 MicroProfile 型微服務，並將其容器化為 Docker 容器，且您已在本機執行微服務並發佈至 Azure。 您可以擴充微服務，以提供其他有用的功能。 如需詳細資訊，請移至 [MicroProfile.io](https://microprofile.io/)。
