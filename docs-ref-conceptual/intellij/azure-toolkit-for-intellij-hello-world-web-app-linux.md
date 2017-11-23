---
title: "使用適用於 IntelliJ 的 Azure 工具組，在 Linux 容器中執行 Hello World Web 應用程式"
description: "了解如何在 Linux 容器中建立基本的 Hello World Web 應用程式，並使用適用於 IntelliJ 的 Azure 工具組將其發佈至 Azure。"
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: 2c0bfc581b5bb033f301d5a1e632377442821392
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2017
---
# <a name="run-a-hello-world-web-app-in-a-linux-container-by-using-the-azure-toolkit-for-intellij"></a>使用適用於 IntelliJ 的 Azure 工具組，在 Linux 容器中執行 Hello World Web 應用程式

[Docker] 容器是部署 Web 應用程式的常用方法。 藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。 適用於 IntelliJ 的 Azure 工具組在部署容器至 Microsoft Azure 方面新增了功能，藉此為 Java 開發人員簡化部署程序。

本文示範建立基本的 Hello World Web 應用程式，並使用適用於 IntelliJ 的 Azure 工具組在 Linux 容器中發佈該 Web 應用程式所需的步驟。

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* [Docker] 用戶端。

> [!NOTE]
>
> 為了完成本教學課程中的步驟，您需要設定 [Docker] 才能在沒有 TLS 的連接埠 2375 上公開精靈。 您可以在安裝 Docker 時或透過 Docker 設定功能表進行設定。
>
> ![Docker 設定功能表][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a>建立新的 Web 應用程式專案

1. 使用 [適用於 IntelliJ 的 Azure 工具組登入指示] 文章中的步驟來啟動 IntelliJ 並登入您的 Azure 帳戶。

1. 按一下 [檔案] 功能表，按一下 [新增]，然後按一下 [專案]。
   
   ![建立新專案][file-new-project]

1. 在 **新增專案** 對話方塊中，選取 **Maven** ，然後選取 **maven-archetype-webapp** ，再按一下 [下一步]。
   
   ![選擇 Maven archetype webapp][maven-archetype-webapp]
   
1. 指定 Web 應用程式的 [GroupId] 和 [ArtifactId]，然後按一下 [下一步]。
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. 自訂任何 Maven 設定或接受預設值，然後按一下 [下一步]。
   
   ![指定 Maven 設定][maven-options]

1. 指定專案名稱和位置，然後按一下 [完成]。
   
   ![指定專案名稱][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>建立 Azure Container Registry 以用作私人 Docker 登錄

下列步驟會逐步引導您使用 Azure 入口網站來建立 Azure Container Registry。

> [!NOTE]
>
> 如果您想要使用 Azure CLI 而不是 Azure 入口網站，請遵循下列[使用 Azure CLI 2.0 建立私人 Docker 容器登錄][Create Docker Registry using Azure CLI]中的步驟。
>

1. 瀏覽至 [Azure 入口網站]並登入。

   一旦您已在 Azure 入口網站登入您的帳戶後，就可以遵循[使用 Azure 入口網站建立私人 Docker 容器登錄]文章中的步驟，為便於了解，會在下列步驟中加以釋義。

1. 依序按一下功能表的 [+ 新增] 圖示、[容器]，以及 [Azure Container Registry]。
   
   ![建立新的 Azure Container Registry][AR01]

1. 當 Azure Container Registry 範本的資訊頁面顯示時，按一下 [建立]。 

   ![建立新的 Azure Container Registry][AR02]

1. 當 [建立容器登錄] 頁面顯示時，輸入您的 [登錄名稱] 和 [資源群組]，針對 [管理使用者] 選擇 [啟用]，然後按一下 [建立]。

   ![設定 Azure Container Registry 設定][AR03]

1. 一旦建立容器登錄之後，瀏覽至 Azure 入口網站中的容器登錄，然後按一下 [存取金鑰]。 將後續步驟的使用者名稱和密碼記下。

   ![Azure Container Registry 存取金鑰][AR04]

## <a name="deploy-your-web-app-in-a-docker-container"></a>在 Docker 容器中部署 Web 應用程式

1. 在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [新增 Docker 支援]。

   這會自動建立具有預設設定的 Docker 檔案。

   ![新增 Docker 支援][add-docker-support]

1. 新增 Docker 支援之後，請在專案總管中以滑鼠右鍵按一下專案，選擇 [Azure]，然後按一下 [在 Web 應用程式 (Linux) 上執行]。

   ![在 Web 應用程式 (Linux) 上執行][run-on-web-app-linux]

1. 顯示 [在 Web 應用程式 (Linux) 上執行] 對話方塊時，請填寫必要資訊：

   * [名稱]：這會指定將顯示於 Azure 工具組的易記名稱。 

   * [伺服器 URL]：這會指定本文先前章節中容器登錄的 URL；這通常會使用下列語法："*registry*.azurecr.io"。 

   * [使用者名稱] 和 [密碼]：指定本文先前章節中容器登錄的存取金鑰。 

   * [映像與標記]：指定容器映像名稱；這通常會使用下列語法："*registry*.azurecr.io/*appname*:latest"，其中： 
      * registry 是本文先前章節中的容器登錄 
      * appname 是 Web 應用程式的名稱 

   * [使用現有的 Web 應用程式] 或 [建立新的 Web 應用程式]：指定您是否要將容器部署至現有的 Web 應用程式或建立新的 Web 應用程式。 

   * [資源群組]：指定您是否要使用現有的資源群組或建立新的資源群組。 

   * [App Service 方案]：指定您是否要使用現有的 App Service 方案或建立新的 App Service 方案。 

1. 完成以上所列設定時，請按一下 [執行]。

   ![建立 Web 應用程式][create-web-app]

1. 發佈 Web 應用程式之後，系統會將您的設定儲存為預設值，您可以按一下工具列上的綠色箭號圖示，在 Azure 上執行您的應用程式。 您可以按一下 Web 應用程式的下拉式功能表，然後按一下 [編輯組態]，修改這些設定。

   ![[編輯組態] 功能表][edit-configuration-menu]

1. 顯示 [執行/偵錯組態] 對話方塊時，您可以修改任何預設設定，然後按一下 [確定]。

   ![[編輯組態] 對話方塊][edit-configuration-dialog]

## <a name="next-steps"></a>後續步驟

如需 Docker 的其他資源，請參閱 [Docker 的官方網站][Docker]。

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Azure 入口網站]: https://portal.azure.com/
[使用 Azure 入口網站建立私人 Docker 容器登錄]: /azure/container-registry/container-registry-get-started-portal
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[AR01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR01.png
[AR02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR02.png
[AR03]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR03.png
[AR04]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR04.png

[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[create-web-app]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-web-app.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
