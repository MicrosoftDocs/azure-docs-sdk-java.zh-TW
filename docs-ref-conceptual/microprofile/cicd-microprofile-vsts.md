---
title: 使用 Azure DevOps 為 MicroProfile 應用程式設定 CI/CD
description: 了解如何使用 Azure DevOps 設定 CI/CD 發行週期，並將 MicroProfile 應用程式部署至用於容器的 Azure Web App
services: Azure DevOps
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/14/2018
ms.devlang: Java
ms.service: Azure DevOps
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: c2b6bf3370982d26d8d23fede370e0105a70b734
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506329"
---
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a>使用 Azure DevOps 為 MicroProfile 應用程式設定 CI/CD

本教學課程將示範 Java EE 開發人員如何輕鬆地使用 Azure DevOps (先前稱為 VSTS) 設定 CI/CD 發行週期，藉此將他們的 [MicroProfile](http://microprofile.io) 應用程式部署至用於容器的 Web App。  在此範例中，我們使用的 MicroProfile 應用程式會使用 [Payara Micro](https://www.payara.fish/payara_micro) 作為基礎映像。   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
我們將以建置 Docker 映像作為開頭來進行 Azure DevOps 容器化程序，並將容器映像推送至 Azure 容器登錄。  然後藉由 Azure DevOps 發行管線將容器映像部署至 Web 應用程式。

## <a name="prerequisites"></a>必要條件
- 從 [Github](https://github.com/Azure-Samples/microprofile-hello-azure) 複製並儲存 Git URL
- 註冊或登入您的 [Azure DevOps](https://dev.azure.com) 帳戶
- 建立新的 [Azure DevOps 專案](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav)，並使用上述的 Git URL 來**匯入存放庫**
- 建立 [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR)
- 建立用於容器的 Azure Web App
> [!NOTE]
>
> 佈建 Web 應用程式執行個體時，選取容器設定中的 [快速入門]


## <a name="create-a-build-definition"></a>建立組建定義

每當 Java EE 應用程式的來源應用程式中有認可時，Azure DevOps 中的組建定義就會自動執行組建中所有工作。  在此範例中，Azure DevOps 將使用 Maven 來建置 Java MicroProfile 專案。

1. 在 Azure DevOps 專案頁面的頂端按一下 [組建與發行] 索引標籤。  然後，選取 [組建] 連結 

<img src="media/VSTS/Buid-and-Release1.png">

2. 按一下 [新增管線] 按鈕，然後按一下 [繼續] 來開始定義您的組建工作
3. 從範本清單選取 [Maven]，然後按一下 [套用] 按鈕來建立您的 Java 專案
4. 使用 [代理程式集區] 欄位的下拉式功能表來選取 [託管的 Linux 預覽] 選項。
> [!NOTE]
>
> 這會告訴 Azure DevOps 要使用哪個組建伺服器。  您可以使用您私人的自訂組建伺服器

5. 若要設定您的組建以進行持續整合，請選取 [觸發程序] 索引標籤，然後核取 [啟用持續整合] 核取方塊。  

<img src="media/VSTS/Build-Triggers2.png"> 
 
6. 選取 [工作] 索引標籤可返回主要的組建管線頁面
7. 使用 [儲存與佇列] 下拉式功能表來選取 [儲存] 選項
 

## <a name="create-a-docker-build-image"></a>建立 Docker 組建映像

在此工作中，Azure DevOps 會使用 Dockerfile 和來自 Payara Micro 的基礎映像來建立 Docker 映像。  

1. 選取 [工作] 索引標籤可返回主要的組建管線頁面
2. 按一下 [+] 圖示，即可將新工作新增至組建定義
 
<img src="media/VSTS/Tasks-add4.png">
 
3. 從範本清單中選取 [Docker]，然後按一下 [新增] 按鈕
4. 在 [顯示名稱] 欄位中輸入描述性名稱
5. 確認 [容器登錄類型] 的下拉式功能表中已選取 [Azure Container Registry]。
> [!NOTE]
>
>  如果您使用 Docker 中樞或另一個登錄，請改為選取 [容器登錄]。  然後按一下 [+ 新增] 按鈕，為其提供認證和連線資訊。 然後跳至命令區段以繼續。

6. 使用 [Azure 訂用帳戶] 下拉式功能表來選擇您的 Azure 訂用帳戶識別碼。  然後按一下 [授權] 按鈕
7. 在 [Azure 容器登錄] 下拉式功能表中，選取您在 Azure 中建立的登錄名稱。
8. 確認 [命令] 下拉式功能表中已選取 [組建] 選項。
9. 針對 [Dockerfile]，按一下文字方塊旁邊的路徑瀏覽圖示，從 GitHub 專案中選取 Dockerfile。  然後按一下 [確定] 按鈕。

<img src="media/VSTS/Dockerfile5.png">

10. 在 [映像名稱] 底下，核取 [包含最新標記] 核取方塊。 
11. 使用 [儲存與佇列] 下拉式功能表來選取 [儲存] 選項。

## <a name="push-docker-image-to-acr"></a>將 Docker 映像推送至 ACR

在此工作中，Azure DevOps 會將 Docker 映像推送至 Azure Container Registry。  這將用來執行作為容器化 Java Web 應用程式的 MicroProfile API 應用程式。

1. 由於我們將在 Azure DevOps 中使用 Docker，因此請重複上方**建立 Docker 組建映像**一節中的步驟 1-7 來建立新的 Docker 範本。
2. 在 [命令] 下拉式功能表中選取 [推送]。
3. 按一下 [儲存與佇列] 索引標籤，然後選取 [儲存與佇列] 選項。
4. 確認快顯視窗上的代理程式集區中已選取 [託管的 Linux 預覽]。  然後按一下 [儲存與佇列] 按鈕。
5. 按一下組建編號，以確認 Java 專案的組建管線已成功完成。

<img src="media/VSTS/Build-Number6.png">
 

## <a name="create-a-release-definition-for-a-java-app"></a>建立 Java 應用程式的發行定義

一旦組建程序順利完成，Azure DevOps 中的發行管線就會自動觸發目標環境 (例如 Azure) 的組建成品部署。   您可以針對開發、測試、預備或生產環境建立發行管線。

1. 在 Azure DevOps 專案頁面的頂端按一下 [組建與發行] 索引標籤。  然後，選取 [發行] 連結。

<img src="media/VSTS/Release-new-pipeline7.png">
 
2. 按一下 [新增管線] 按鈕
3. 在範本清單中選取 [將 Java 應用程式部署至 Azure App Service]，然後按一下 [套用] 按鈕。

<img src="media/VSTS/deploy-java-template8.png">
 
4. 設定**階段名稱** (例如開發、測試、預備或生產)。  然後按一下 [X] 按鈕來關閉快顯視窗
5. 在成品區段中按一下 [+ 新增] 按鈕。  這會將組建定義中的成品連結至此發行定義。  
6. 使用 [來源 (組建管線)] 的下拉式功能表來選取您的組建定義。 然後按一下 [新增] 按鈕以繼續。

<img src="media/VSTS/add-artifact9.png">
 
7. 按一下管線上的 [工作] 索引標籤。  然後，選取您的階段名稱。
 
<img src="media/VSTS/release-stage10.png">

8. 使用 [Azure 訂用帳戶] 下拉式功能表來選擇您的 Azure 訂用帳戶識別碼。
9. 從 [應用程式類型] 下拉式功能表中選取 [Linux 應用程式]
10. 在 [應用程式服務名稱] 下拉式功能表中，選取先前所建立用於容器的 Web APP 執行個體名稱
11. 在 [登錄或命名空間] 欄位中輸入您的 Azure 容器登錄名稱。  例如 **myregistry.azure.io**
12. 在 [存放庫] 欄位中輸入登錄名稱
13. 按一下 [部署 Azure App Service]。  在 [標記] 文字方塊中輸入容器映像的標記 
14. 按一下 [在代理程式上執行]。  在代理程式集區的下拉式功能表中選取 [託管的 Linux 預覽] 

## <a name="setup-environment-variables"></a>設定環境變數

1. 按一下 [變數] 索引標籤。然後按一下 [+ 新增] 按鈕，以定義您的環境變數
2. 為您的容器登錄 URL、使用者名稱和密碼新增變數名稱和值。   基於安全考量，請按一下上鎖圖示來隱藏密碼值。

例如︰
- registry.password
- registry.url
- registry.username

<img src="media/VSTS/environment-variables12.png">

3. 按一下 [工作] 索引標籤可返回主要的發行管線定義頁面
4. 按一下 [部署 Azure App Service]。 
5. 展開 [應用程式和組態設定] 區段，然後按一下 [應用程式設定] 欄位的瀏覽路徑來新增環境變數，即可在部署期間連線到容器登錄。
6. 按一下 [+ 新增] 按鈕，以定義下列應用程式設定，並指派環境變數
- DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)
- DOCKER_REGISTRY_SERVER_URL = $(registry.url)
- DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)

<img src="media/VSTS/environment-variables14.png">
 
7. 按一下 [確定] 按鈕以繼續

## <a name="setup-continious-deployment--deploy-java-application"></a>設定持續部署與部署 Java 應用程式

1. 若要啟用持續部署，請按一下 [管線] 索引標籤
2. 在 [成品] 區段中，按一下閃電圖示。  然後將 [持續部署觸發程序] 設定為 [已啟用]。

<img src="media/VSTS/release-enable-CD.png">
 
3. 按一下 [儲存] 按鈕，然後按一下 [確定] 按鈕 
4. 按一下 [+ 發行] 下拉式功能表，然後選取 [建立發行] 連結
5. 使用 [觸發程序階段從自動變更為手動] 下拉式功能表，選取您階段名稱的核取方塊
6. 按一下 [建立] 按鈕以繼續
7. 按一下發行編號。  然後將滑鼠游標停留在階段名稱上，並按一下 [部署] 按鈕
8. 接著，在快顯視窗中按一下 [部署] 按鈕，以啟動部署至 Azure 的部署程序


## <a name="test-the-java-web-application"></a>測試 Java Web 應用程式
1. 在 Web 瀏覽器中執行 Web 應用程式 URL：  
https://{your-app-service-name}.azurewebsites.net/api/hello

 
<img src="media/VSTS/web-app16.png">

2. 網頁應該會顯示 **Hello Azure!**
 
<img src="media/VSTS/web-api17.png">
