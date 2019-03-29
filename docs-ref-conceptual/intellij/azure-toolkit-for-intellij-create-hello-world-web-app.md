---
title: 使用 IntelliJ 建立 Azure 的 Hello World Web 應用程式
description: 本教學課程將示範如何使用適用於 IntelliJ 的 Azure 工具組來建立 Azure 的 Hello World Web 應用程式。
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 7055751d1b1c37e019ef4ed59f1710ce6905e9f8
ms.sourcegitcommit: a108a82414bd35be896e3c4e7047f5eb7b1518cb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489636"
---
# <a name="create-a-hello-world-web-app-for-azure-using-intellij"></a>使用 IntelliJ 建立 Azure 的 Hello World Web 應用程式

本教學課程示範如何使用 [適用於 IntelliJ 的 Azure 工具組]，建立基本的 Hello World 應用程式，並做為 Web 應用程式部署到 Azure。

> [!NOTE]
>
> 如需使用[適用於 Eclipse 的 Azure 工具組]的此文章版本，請參閱[使用 Eclipse 建立 Azure 的 Hello World Web 應用程式][eclipse-hello-world]。
>

> [!IMPORTANT]
> 
> 適用於 IntelliJ 的 Azure 工具組在 2017 年 8 月更新，有不同的工作流程。 本文說明使用適用於 IntelliJ 的 Azure 工具組版本 3.0.7 (或更新版本) 來建立 Hello World Web 應用程式。 如果您使用 3.0.6 版 (或舊版) 工具組，您必須依照[使用舊版工具組在 IntelliJ 中建立 Azure 的 Hello World Web 應用程式][Legacy Version]中的步驟。
> 

當您完成本教學課程，在網頁瀏覽器中檢視您的應用程式時，看起來會如下圖所示：

![Hello World 應用程式預覽][browse-web-app]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>建立新的 Web 應用程式專案

1. 使用[適用於 IntelliJ 的 Azure 工具組的 Azure 登入指示][intelliJ-sign-in-instructions]文章中的指示，來啟動 IntelliJ 並登入您的 Azure 帳戶。

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

1. 在 IntelliJ 的 [專案總管] 檢視中，依序展開 [src]、[main] 和 [webapp]，然後按兩下 [index.jsp]。
   
   ![開啟索引頁面][open-index-page]

1. 當 index.jsp 檔案在 IntelliJ 中開啟時，加入文字以動態顯示 **Hello World!** (在現有 `<body>` 元素內)。 您已更新的 `<body>` 內容看起來應該與下列範例類似：
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![編輯索引頁面][edit-index-page]

1. 儲存 index.jsp。

## <a name="deploy-your-web-app-to-azure"></a>將 Web 應用程式部署至 Azure

1. 在 IntelliJ 的 [專案總管] 畫面，以滑鼠右鍵按一下您的專案，並選擇 [Azure]，然後選擇 [在 Web 應用程式上執行]。
   
   ![[在 Web 應用程式上執行] 功能表][run-on-web-app-menu]

1. 在 [在 Web 應用程式上執行] 對話方塊中，可以選擇下列其中一個選項：

   * 請選擇現有 Web 應用程式 (如果有)，然後按一下 [執行]。

      ![[在 Web 應用程式上執行] 對話方塊][run-on-web-app-dialog]

   * 在 WebApp 下拉式清單中，按一下 [建立新的 Web 應用程式]。 如果您選擇建立新的 Web 應用程式，請指定 Web 應用程式的必要資訊，然後在建立完Web 應用程式之後按一下 [執行]。

      ![建立新的 Web 應用程式][create-new-web-app-dialog]

1. 工具組成功部署 Web 應用程式時，會顯示狀態訊息，也會顯示已部署的 Web 應用程式本身的 URL。

   ![部署成功][successfully-deployed]

1. 您可以使用狀態訊息中提供的連結，瀏覽至您的 Web 應用程式。

   ![瀏覽 Web 應用程式][browse-web-app]

1. 發佈 Web 應用程式之後，系統會將您的設定儲存為預設值，您可以按一下工具列上的綠色箭號圖示，在 Azure 上執行您的應用程式。 您可以透過按一下 Web 應用程式的下拉式功能表，然後按一下 [編輯組態]，修改您的設定。

   ![[編輯組態] 功能表][edit-configuration-menu]

1. 顯示 [執行/偵錯組態] 對話方塊時，您可以修改任何預設設定，然後按一下 [確定]。

   ![[編輯組態] 對話方塊][edit-configuration-dialog]

## <a name="next-steps"></a>後續步驟

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

如需建立 Azure Web Apps 的詳細資訊，請參閱 [Web 應用程式概觀]。

<!-- URL List -->

[適用於 IntelliJ 的 Azure 工具組]: azure-toolkit-for-intellij.md
[適用於 Eclipse 的 Azure 工具組]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web 應用程式概觀]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[run-on-web-app-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[run-on-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
