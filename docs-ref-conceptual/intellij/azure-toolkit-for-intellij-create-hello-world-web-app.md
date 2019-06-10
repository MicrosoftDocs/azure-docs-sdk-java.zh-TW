---
title: 使用 IntelliJ 建立 Azure App Service 的 Hello World Web 應用程式
description: 本教學課程將示範如何使用適用於 IntelliJ 的 Azure 工具組來建立 Azure 的 Hello World Web 應用程式。
services: app-service
keywords: java, IntelliJ, web 應用程式, azure app service, hello world, 快速入門
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
ms.openlocfilehash: ae0749ce1ddab971f1a83e2e5e58492fd8ccb287
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65626108"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a>使用 IntelliJ 建立 Azure App Service 的 Hello World Web 應用程式

使用開放原始碼的[適用於 IntelliJ 的 Azure 工具組](https://plugins.jetbrains.com/plugin/8053)外掛程式建立基本的 Hello World 應用程式，並將其部署至 Azure App Service 作為 Web 應用程式，只需短短幾分鐘即可完成。

> [!NOTE]
>
> 如果您偏好使用 Eclipse，請參閱我們[適用於 Eclipse 的類似教學課程][eclipse-hello-world]。
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> 在完成本教學課程之後，別忘了清除資源。 如此，執行此指南時就不會超過您的免費帳戶配額。
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>安裝和登入

1. 在 IntelliJ IDEA 的 [設定/喜好設定] 對話方塊 (Ctrl+Alt+S) 中，選取 [外掛程式]  。 然後，在 **Marketplace** 中尋找**適用於 IntelliJ 的 Azure 工具組**，然後按一下 [安裝]  。 安裝之後，按一下 [重新啟動]  以啟動此外掛程式。 

   ![Marketplace 中適用於 IntelliJ 的 Azure 工具組外掛程式][marketplace]

2. 若要登入您的 Azure 帳戶，請開啟 **Azure 總管**資訊看板，然後按一下頂端列中的 [Azure 登入]  圖示 (或從 IDEA 功能表**工具/Azure/Azure 登入**點選)。

   ![IntelliJ 的 [Azure 登入] 命令][I01]

3. 在 [Azure 登入]  視窗中選取 [裝置登入]  ，然後按一下 [登入]  ([其他登入選項](azure-toolkit-for-intellij-sign-in-instructions.md))。

   ![選取了 [裝置登入] 的 [Azure 登入] 視窗][I02]

4. 按一下 [Azure 裝置登入]  對話方塊中的 [複製並開啟]  。

   ![[Azure 登入] 對話方塊視窗][I03]

5. 在瀏覽器中，貼上您的裝置程式碼 (您在上一個步驟中按一下 [複製並開啟]  時所複製)，然後按 [下一步]  。

   ![裝置登入瀏覽器][I04]

6. 在 [選取訂用帳戶]  對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]  。

   ![[選取訂用帳戶] 對話方塊][I05]

## <a name="creating-web-app-project"></a>建立 Web 應用程式專案

1. 在 IntelliJ 中，依序按一下 [檔案]  功能表、[新增]  和 [專案]  。

   ![建立新專案][file-new-project]

2. 在 **新增專案** 對話方塊中，選取 **Maven** ，然後選取 **maven-archetype-webapp** ，再按一下 [下一步]  。

   ![選擇 Maven archetype Webapp][maven-archetype-webapp]

3. 指定 Web 應用程式的 [GroupId]  和 [ArtifactId]  ，然後按一下 [下一步]  。

   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

4. 自訂任何 Maven 設定或接受預設值，然後按一下 [下一步]  。

   ![指定 Maven 設定][maven-options]

5. 指定專案名稱和位置，然後按一下 [完成]  。

   ![指定專案名稱][project-name]

6. 在 [專案總管] 檢視下開啟檔案 **src/main/webapp/index.jsp**，並依照下列方式加以編輯，然後**儲存變更**：

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```

   ![編輯索引頁面][edit-index-page]

## <a name="deploying-web-app-to-azure"></a>將 Web 應用程式部署至 Azure

1. 在 [專案總管] 檢視中，以滑鼠右鍵按一下您的專案，並展開 **Azure**，然後按一下 [部署至 Azure]  。

   ![部署至 Azure 功能表][deploy-to-azure-menu]

1. 在 [部署至 Azure] 對話方塊中，您可以直接將應用程式部署至現有的 Tomcat WebApp (如果已經有的話)，否則您應先新建一個。
   1. 按一下 [沒有可用的 WebApp，請按一下以建立一個新的]  連結以建立新的 Web 應用程式，如果您的訂用帳戶中有現有的 Web 應用程式，則可以從 WebApp 下拉式清單中選擇 [建立新的 WebApp]  。

      ![[部署至 Azure] 對話方塊][deploy-to-azure-dialog]

   1. 在快顯對話方塊中，選擇 **TOMCAT 8.5-jre8** 作為 Web 容器，並指定其他必要的資訊，然後按一下 [確定]  以建立 WebApp。

      ![建立新的 Web 應用程式][create-new-web-app-dialog]

   1. 從 WebApp 下拉式清單中選擇 Web 應用程式，然後按一下 [執行]  。(如果您想要部署至現有的 WebApp，可以從這裡開始)

      ![部署至現有的 WebApp][deploy-to-existing-webapp]

1. 工具組成功部署 Web 應用程式時會顯示狀態訊息，以及已部署的 Web 應用程式本身的 URL。

   ![部署成功][successfully-deployed]

1. 您可以使用狀態訊息中提供的連結，瀏覽至您的 Web 應用程式。

   ![瀏覽 Web 應用程式][browse-web-app]

## <a name="managing-deploy-configurations"></a>管理部署組態

1. 發佈 Web 應用程式之後，系統會將您的設定儲存為預設值，您可以按一下工具列上的綠色箭號圖示以執行部署。 您可以透過按一下 Web 應用程式的下拉式功能表，然後按一下 [編輯組態]  ，修改您的設定。

   ![[編輯組態] 功能表][edit-configuration-menu]

1. 顯示 [執行/偵錯組態]  對話方塊時，您可以修改任何預設設定，然後按一下 [確定]  。

   ![[編輯組態] 對話方塊][edit-configuration-dialog]

## <a name="cleaning-up-resources"></a>清除資源

1. 在 Azure 總管中刪除 Web 應用程式

     ![清除資源][clean-resources]

## <a name="next-steps"></a>後續步驟

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

如需建立 Azure Web Apps 的詳細資訊，請參閱 [Web 應用程式概觀]。

<!-- URL List -->

[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web 應用程式概觀]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->
[marketplace]:./media/azure-toolkit-for-intellij-create-hello-world-web-app/marketplace.png
[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[deploy-to-azure-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[deploy-to-azure-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[deploy-to-existing-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/deploy-to-existing-webapp.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
[clean-resources]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/clean-resource.png
[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png
