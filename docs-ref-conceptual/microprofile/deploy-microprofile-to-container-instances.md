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
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a>使用 Docker 與 Azure 將 MicroProfile 應用程式部署到雲端

本文會示範如何將 [MicroProfile.io] 應用程式封裝至 Docker 容器，並在 Azure 容器執行個體上加以執行。

> [!NOTE]
>
> 只要 Docker 容器映像是可自動執行的 (也就是包含執行階段)，此程序就能與 MicroProfile.io 的任何實作搭配使用。

## <a name="prerequisites"></a>必要條件

若要完成本教學課程中的步驟，您必須具備下列必要條件：

* Azure 訂用帳戶；如果您沒有 Azure 訂用帳戶，可以註冊[免費的 Azure 帳戶]。
* [Azure 命令列介面 (CLI)]。
* 最新的 [Java 開發套件] \(JDK\)，1.8 版或更新版本。
* Apache 的 [Maven] 建置工具 (第 3 版以上)。
* [Git] 用戶端。

## <a name="microprofile-hello-azure-sample"></a>MicroProfile Hello Azure 範例

在本文中，我們將使用 [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) 範例：

### <a name="clone-build-and-run-locally"></a>在本機複製、建置並執行

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

您可以藉由呼叫 `curl` 或透過[瀏覽器](http://localhost:8080/api/hello)進行瀏覽，來測試應用程式：

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a>部署至 Azure

現在讓我們使用 [Azure 容器執行個體] 和 [Azure Container Registry] 服務來將此應用程式帶入雲端。

### <a name="build-a-docker-image"></a>建置 Docker 映像

範例專案已提供您可以使用的 Dockerfile。 不過，您不需要安裝 Docker，因為我們將使用 Azure Container Registry Build 功能在雲端中建置映像。

若要建置映像，並讓它能在 Azure 上執行，您必須請遵循下列步驟：

1. 使用 Azure CLI 安裝並登入
1. 建立 Azure 資源群組
1. 建立 Azure Container Registry (ACR)
1. 建置 Docker 映像
1. 將 Docker 映像發佈到之前建立的 ACR
1. (選擇性) 以一個命令建置並發佈至 ACR


#### <a name="set-up-azure-cli"></a>設定 Azure CLI

請確定您有 Azure 訂用帳戶、[已安裝 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)，並且已向您的帳戶進行驗證：

```bash
az login
```

#### <a name="create-a-resource-group"></a>建立資源群組

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a>建立 Azure Container Registry 執行個體

此命令會使用基本名稱和隨機數字來建立應該是全域唯一的容器登錄。

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a>建置 Docker 映像

雖然您可以使用 Docker 本身在本機輕鬆建立 Docker 映像，但您可以考慮在雲端中建立映像，原因如下：

1. 不需要在本機安裝 Docker
1. 速度更快，因為建置作業會在其他地方進行 (不包含內容上傳時間)
1. 雲端中的程序可存取更快速的網際網路，因此下載速度更快
1. 映像會直接進入 Container Registry

基於這些原因，我們將使用 [Azure Container Registry Build] 功能建置映像：

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a>將 Docker 映像從 Azure Container Registry (ACR) 部署至容器執行個體 (ACI)

現在，您的 ACR 上已有映像可用，接著讓我們來推送和具現化 ACI 上的容器個體。 首先，我們要確定我們可以進入 ACR 以進行驗證：

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a>測試已部署的 MicroProfile 應用程式

您的應用程式現在應該已啟動並且在執行中。 若要從命令列中進行測試，請執行下列命令：

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

恭喜！ 您已成功建置 MicroProfile 應用程式，並將其部署為 Microsoft Azure 上的 Docker 容器。

## <a name="next-steps"></a>後續步驟

如需本文所討論之各種技術的詳細資訊，請參閱下列文章：

* [從 Azure CLI 登入 Azure](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure Container Registry Build]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
