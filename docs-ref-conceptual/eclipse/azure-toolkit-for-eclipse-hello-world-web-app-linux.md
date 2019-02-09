---
title: 使用 Azure Toolkit for Eclipse，在雲端中部署 Linux 容器中執行的 Hello World Web 應用程式
description: 使用 Azure Toolkit for Eclipse，在 Linux 容器中執行基本的 Hello World Web 應用程式並部署到雲端。
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/20/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 799f21a282956f9a88aa35743157fc1292197569
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55649244"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-eclipse"></a>使用 Azure Toolkit for Eclipse 將 Hello World Web 應用程式部署到雲端的 Linux 容器

[Docker] 容器是部署 Web 應用程式的常用方法。 藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。 Azure Toolkit for Eclipse 在部署容器至 Microsoft Azure 方面新增了功能，藉此為 Java 開發人員簡化部署程序。

本文示範建立基本的 Hello World Web 應用程式，並使用 Azure Toolkit for Eclipse 在 Linux 容器中發佈該 Web 應用程式所需的步驟。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]
* [Docker] 用戶端。

> [!NOTE]
>
> 為了完成本教學課程中的步驟，您需要設定 [Docker] 才能在沒有 TLS 的連接埠 2375 上公開精靈。 您可以在安裝 Docker 時或透過 Docker 設定功能表進行設定。
>
> ![Docker 設定功能表][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a>建立新的 Web 應用程式專案

1. 使用 [[Azure Toolkit for Eclipse 登入指示](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)] 文章中的步驟，來啟動 Eclipse 並登入您的 Azure 帳戶。

1. 依序按一下 [檔案] 功能表、[新增] 和 [動態 Web 專案]。
   
   ![建立新專案][file-new-project]

1. 在 [新增動態 Web 專案] 對話方塊中，指定專案名稱和位置，然後按一下 [完成]。
   
   ![指定專案名稱][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>建立 Azure Container Registry 以用作私人 Docker 登錄

下列步驟會逐步引導您使用 Azure 入口網站來建立 Azure Container Registry。

> [!NOTE]
>
> 如果您想要使用 Azure CLI 而不是 Azure 入口網站，請遵循下列[使用 Azure CLI 2.0 建立私人 Docker 容器登錄][Create Docker Registry using Azure CLI]中的步驟。
>

1. 瀏覽至 [Azure 入口網站]並登入。

   一旦已在 Azure 入口網站登入您的帳戶後，就可以遵循[使用 Azure 入口網站建立私人 Docker 容器登錄]文章中的步驟，為便於了解，會在下列步驟中加以釋義。

1. 依序按一下功能表的 [+ 建立資源] 圖示、[容器]，以及 [Container Registry]。
   
   ![建立新的 Azure Container Registry][create-container-registry-01]

1. 當 [建立容器登錄] 頁面顯示時，輸入您的 [登錄名稱] 和 [資源群組]，針對 [管理使用者] 選擇 [啟用]，然後按一下 [建立]。

   ![設定 Azure Container Registry 設定][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a>在 Docker 容器中部署 Web 應用程式

1. 在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [新增 Docker 支援]。

   這會自動建立具有預設設定的 Docker 檔案。

   ![新增 Docker 支援][add-docker-support]

1. 新增 Docker 支援之後，請在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [發佈至用於容器的 Web App]。

   ![發佈至用於容器的 Web App][run-on-web-app-for-containers]

1. 顯示 [在用於容器的 Web App 上執行] 對話方塊時，請填寫必要資訊：

   * **Docker 檔案**：這會指定將 Docker 支援新增至專案時所建立的 Docker 檔案路徑。 

   * **Container Registry**：從您在本文的上一節內建立的下拉式選單中，選取容器登錄。 系統會自動填入 [伺服器 URL]、[使用者名稱]和 [密碼] 欄位。

   * **映像與標記**：指定容器映像名稱；這通常會使用下列語法："*registry*.azurecr.io/*appname*:latest"，其中： 
      * registry 是本文先前章節中的容器登錄 
      * appname 是 Web 應用程式的名稱 

   * **用於容器的 Web App**：選擇 [使用現有的] 或 [建立新的] 來指定是否要將容器部署至現有的 Web 應用程式或建立新的 Web 應用程式。  您指定的**應用程式名稱**將為 Web 應用程式建立 URL；例如：*wingtiptoys.azurewebsites.net* 。

   * **資源群組**：指定是否要使用現有的資源群組或建立新的資源群組。 

   * **App Service 方案**：指定是否要使用現有的應用程式服務方案或建立新的應用程式服務方案。 

   ![在用於容器的 Web App 上執行][run-on-web-app-linux]

1. 完成以上所列設定時，請按一下 [確定] 以發佈 Web 應用程式至 Azure。

1. 發佈 Web 應用程式之後，您可以瀏覽至稍早為 Web 應用程式指定的 URL；例如：*wingtiptoys.azurewebsites.net*。

   ![瀏覽至您的 Web 應用程式][browsing-to-web-app]

## <a name="next-steps"></a>後續步驟

如需 Docker 的其他資源，請參閱 [Docker 的官方網站][Docker]。

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure 入口網站]: https://portal.azure.com/
[使用 Azure 入口網站建立私人 Docker 容器登錄]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[add-docker-support]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-eclipse-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/file-new-project.png
[project-name]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-linux.png
