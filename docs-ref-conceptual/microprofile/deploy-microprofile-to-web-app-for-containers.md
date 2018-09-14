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
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a>將 Java 型 MicroProfile 服務部署至用於容器的 Azure Web App

MicroProfile 非常適合用來建置極小的 Java 應用程式，以便您快速且輕鬆地將其部署到[用於容器的 Azure Web App](https://azure.microsoft.com/services/app-service/containers/) 等服務。 在本教學課程中，我們將建立 MicroProfile 型的簡單微服務，此微服務接著會容器化為 Docker 容器並部署到 [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)，然後使用用於容器的 Azure Web App 進行託管。

> [!NOTE]
>
> 只要 Docker 容器映像是可自動執行的 (也就是包含執行階段)，此程序就能與 MicroProfile.io 的任何實作搭配使用。

更具體地來說，此範例會使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile 1.3](https://microprofile.io/) 來建立 Java 的小型 war 檔 (在建立者機器上佔 5,085 位元組)，然後將其封裝成 Docker 映像 (大約 174 MB)。 此 Docker 映像包含此 Web 應用程式進行完整容器化部署所需的一切。

受惠於 Docker 的運作方式，每當原始程式碼有所變更時，您通常不用重新部署整個 174 MB 的 Docker 映像，因為 Docker 只會上傳有差異的部分 (這明顯小很多)。 這可讓透過 CI/CD 管線執行新版 MicroProfile 應用程式的程序變得極有效率且快速，並且可減少阻力和啟用快速開發反覆項目。

在本教學課程中，我們會先在本機建立及執行程式碼，然後將此程式碼部署為 Azure 上的 Web 應用程式。 在這兩個案例中，我們將依賴 Docker 來簡化並標準化我們的工作。 開始之前，我們將建立 Azure Container Registry 來儲存 Docker 容器。

## <a name="creating-an-azure-container-registry"></a>建立 Azure Container Registry

我們將使用 [Azure 入口網站](http://portal.azure.com)來建立 Azure Container Registry，但是請注意，您也可以選擇使用 Azure CLI。 請遵循下列步驟來建立新的 Azure Container Registry：

1. 登入 [Azure 入口網站](http://portal.azure.com)並建立新的 Azure Container Registry 資源。 提供登錄名稱 (請注意，此名稱會設定為 `pom.xml` 中的 `docker.registry` 屬性)。 如有需要，請變更預設值，然後按一下 [建立]。

1. 一旦容器登錄可使用時 (按下 [建立] 後約 30 秒)，按一下容器登錄，然後按一下左側功能表區域中的 [存取金鑰] 連結。 在這裡，您需要啟用 [管理使用者] 設定，以便從我們的機器存取此容器登錄 (用來將 Docker 容器推送過去)，並也讓用於容器的 Azure Web APP 執行個體 (即將設定) 可進行存取。

1. 當您在 [存取金鑰] 區域中的時候，請記下 `username` 和 `password`值。 我們會將這些值複製/貼上到我們的全域 Maven `settings.xml` 檔案 (如需有關 Maven 設定的詳細資訊，請參閱 [Apache Maven 專案](https://maven.apache.org/settings.html)網站)。 如需參考，以下是建立者系統上 `${user.home}/.m2/settings.xml` 檔案的混淆版本：

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

現在，這項作業已完成，我們可以繼續在本機建置及執行 MicroProfile 應用程式。

## <a name="creating-our-microprofile-application"></a>建立 MicroProfile 應用程式

此範例會以 GitHub 上的可用範例應用程式為基礎，因此我們會先將其複製，然後逐步執行程式碼。 請遵循下列步驟將程式碼複製到您的機器上：

1. `git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git`
1. `cd microprofile-docker-helloworld`

此目錄中有一個 `pom.xml` 檔案，它的用處是以 Maven 建置工具所使用的格式來指定專案。 您可以根據自己的需求來編輯此檔案。 特別是，應該將 `docker.registry` 和 `docker.name` 屬性變更為建立 Azure Container Registry 時設定的 `docker.registry` 和 `docker.name`。

此目錄中另一個要注意的檔案是 Dockerfile，如下所示：

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

此 Dockerfile 會直接以 Payara Micro Docker 容器和建置過程中建立的 .war 檔案副本為基礎來建立新的 Docker 容器。 它也會公開連接埠 8080，如此一來，當服務在 Docker 容器內啟動並執行時，我們就可以存取該服務。

深入探索 `src` 目錄，我們最後會發現 `Application` 類別，如下所示：

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

`@ApplicationPath("/api")` 註解會指定此微服務的基本端點 - 也就是，所有端點會在存取特定 REST 端點所需的 URL 部分前方加上 `/api`。

在 `api` 套件內的類別名為 `API`，其中包含下列程式碼：

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

透過使用 `@Path("/helloworld")` 註解，我們可以看到，此 REST 端點結合 `Application` 類別中指定的 `/api` 時會是 `/api/helloworld`。 使用 HTTP GET 要求來呼叫此端點時，我們可以看到此方法會產生文字/html，事實上就是硬式編碼的 "Hello, world!" 字串。

現在，我們已具有使用 MicroProfile 建立微服務所需的所有程式碼。 我們現在可以使用 Maven 來建置微服務，並把它容器化為 Docker 容器，然後在本機執行。 我們可以使用下列步驟來執行此操作：

1. 執行 `mvn clean package` 並等候其成功完成。

1. 執行 `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`，如果您的 `docker.registry` 是 `jogilescr.azurecr.io`，而 `docker.name` 是 `samples/docker-helloworld`，則執行 `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`。

1. 嘗試在 Web 瀏覽器中存取 [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) 和 [http://localhost:8080/health](http://localhost:8080/health)。 如果您看到預期的 "Hello, world!" 回應 (以及 [/health](http://localhost:8080/health) 端點的健康情況相關資訊)，表示您已成功將 MicroProfile 應用程式部署到本機電腦上。

## <a name="pushing-to-the-azure-container-registry"></a>發佈至 Azure Container Registry

現在，我們已在本機電腦上成功建置與執行 MicroProfile 應用程式，下一步就是將此容器推送至容器登錄。 在本教學課程中，我們會使用 Azure Container Registry，但您可以使用任何容器登錄 (前提是要將 `pom.xml` 檔案編輯為指向相關位置)。

1. 執行 `mvn clean package` 來清除、編譯及建立本機 Docker 映像。
2. 執行 `mvn dockerfile:push` 來推送至 Azure Container Registry。

在這個階段中，您已經將 Docker 容器映像上傳至 Azure Container Registry 中，但它尚未執行，因為我們現在必須將其部署到用於容器的 Azure Web App 執行個體。 我們現在要進行此操作。

## <a name="creating-an-azure-web-app-for-containers-instance"></a>建立用於容器的 Azure Web App 執行個體

1. 返回 [Azure 入口網站](http://portal.azure.com)並新建用於容器的 Azure Web App 執行個體 (位於功能表中的 [Web + 行動] 標題下方)。 以下是一些提示：

   1. 在此指定的名稱會是 Web 應用程式的公用 URL (但若有需要，之後可以新增自訂網域)，因此最好挑選好記的名稱。

   1. 當您進入 [設定容器] 區段時，您可以選取 [Azure Container Registry] 作為 [映像來源]，然後從下拉式清單中選取正確的映像。

   1. 您不需要在 [啟動檔案] 欄位中指定任何值。

1. 建好執行個體後 (同樣地，這非常快速)，按一下該執行個體，然後按一下 [應用程式設定] 功能表項目。 您需要在這裡新增應用程式設定，其中金鑰是 `WEBSITES_PORT`，而值是 `8080`。 這會告訴 Azure 您想要在容器中公開哪個連接埠，並會將其對應至外部連接埠 80。

1. (選擇性) 按一下 [Docker 容器] 連結，並啟用 [持續部署]，如此一來，每當您更新 Azure Container Registry 映像時，Docker 容器就會在用於容器的 Azure Web App 執行個體中自動更新。

1. 您應該能夠在 `http://<appname>.azurewebsites.net/microprofile/api/helloworld` 和 `http://<appname>.azurewebsites.net/health` 上存取 Azure 託管的執行個體。

## <a name="summary"></a>總結

在本教學課程中，我們已逐步建立 MicroProfile 型的簡單微服務，並將其容器化為 Docker 容器，而且我們已在本機執行微服務並發佈至 Azure。 雖然擴充微服務以提供更有用的功能不屬於本教學課程範圍，但是網路上可找到各式各樣的 MicroProfile 教學課程和建議，讀者可參閱 [MicroProfile.io](https://microprofile.io/) 以了解更多內容。
