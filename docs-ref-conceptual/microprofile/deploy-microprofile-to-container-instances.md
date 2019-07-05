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
# <a name="deploy-a-microprofile-app-to-the-cloud-by-using-docker-and-azure"></a>使用 Docker 與 Azure 將 MicroProfile 應用程式部署至雲端

本文會示範如何將 [MicroProfile.io] 應用程式封裝至 Docker 容器，並在 Azure 容器執行個體上加以執行。

> [!NOTE]
> 只要 Docker 容器映像是可自動執行的 (也就是包含執行階段的映像)，此程序就能與 MicroProfile.io 的任何實作搭配使用。

## <a name="prerequisites"></a>必要條件

若要完成本教學課程，您需要下列必要條件：

* Azure 訂用帳戶。 如果您還沒有 Azure 訂用帳戶，您可以註冊[免費的 Azure 帳戶]。
* 已安裝 [Azure CLI]。
* 受支援的 Java 開發套件 (JDK)。 若要進一步了解您在進行 Azure 開發時有哪些可用的 JDK，請參閱 [Java 對於 Azure 和 Azure Stack 的長期支援](https://aka.ms/azure-jdks)。
* [Apache Maven] 建置工具 (第 3 版或更新版本)。
* [Git] 用戶端。

## <a name="microprofile-hello-azure-sample"></a>MicroProfile Hello Azure 範例

在本文中，您將使用 [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) 範例。 使用下列命令在本機加以複製、建置和執行：

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

您可以藉由呼叫 `curl` 或使用[瀏覽器](http://localhost:8080/api/hello)來測試應用程式：

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-the-app-to-azure"></a>將應用程式部署至 Azure

現在，使用 [Azure 容器執行個體]和 [Azure Container Registry] 服務，將此應用程式移至 Azure。

### <a name="build-a-docker-image"></a>建置 Docker 映像

範例專案提供了您可以使用的 Dockerfile。 但您不需要安裝 Docker，因為您將使用 Azure Container Registry Build 功能在雲端中建置映像。

若要建置映像並準備在 Azure 上加以執行，請執行下列作業：

1. 安裝並登入 Azure CLI。
1. 建立 Azure 資源群組。
1. 建立 Azure Container Registry 執行個體。
1. 建置 Docker 映像。
1. 將 Docker 映像發佈至先前建立的容器登錄執行個體。
1. (選擇性) 在單一命令中建置映像並將其發佈至容器登錄執行個體。


#### <a name="set-up-the-azure-cli"></a>設定 Azure CLI

請確定您有 Azure 訂用帳戶、您已安裝 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)，且您已向帳戶進行驗證：

```bash
az login
```

#### <a name="create-a-resource-group"></a>建立資源群組

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-a-container-registry-instance"></a>建立容器登錄執行個體

此命令應會使用基本名稱和隨機數字來建立全域唯一的容器登錄執行個體。

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a>建置 Docker 映像

雖然您可以使用 Docker 本身在本機輕鬆建立 Docker 映像，但您可以考慮在雲端中建立映像，原因如下：

* 您不需要在本機安裝 Docker。
* 速度會快得多，因為建置作業會在其他地方進行 (內容上傳時間除外)。
* 雲端中的程序可更快速地存取網際網路，因此下載速度更快。
* 映像會直接進入容器登錄執行個體。

基於這些原因，您將使用 [Azure Container Registry Build] 功能來建置映像：

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-the-docker-image-from-the-azure-container-registry-instance-to-container-instances"></a>將 Docker 映像從 Azure Container Registry 執行個體部署至容器執行個體

現在，您的容器登錄執行個體上已有可用的映像，接著請推送和具現化容器執行個體上的容器執行個體。 但請先確定您可以向容器登錄執行個體進行驗證：

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a>測試已部署的 MicroProfile 應用程式

您的應用程式現在應該已啟動並且在執行中。 若要從命令列介面進行測試，請使用下列命令：

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

恭喜！ 您已成功將 MicroProfile 應用程式建置為 Docker 容器，並將其部署至 Azure。

## <a name="next-steps"></a>後續步驟

如需本文所討論之各種技術的詳細資訊，請參閱：

* [從 Azure CLI 登入 Azure](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure Container Registry Build]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure CLI]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Apache Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Azure 容器執行個體]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Registry]:  https://docs.microsoft.com/azure/container-registry
