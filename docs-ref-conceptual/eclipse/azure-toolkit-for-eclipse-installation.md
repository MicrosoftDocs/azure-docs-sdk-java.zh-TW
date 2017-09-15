---
title: "安裝 Azure Toolkit for Eclipse"
description: "了解如何安裝 Azure Toolkit for Eclipse。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 59a8bfb6ab4db8ea8c6c9025ca3ced8a13192628
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>安裝 Azure Toolkit for Eclipse

Azure Toolkit for Eclipse 提供範本和功能，可讓您輕鬆地使用 Eclipse 開發環境來建立、開發、測試及部署 Azure 應用程式。 適用於 Eclipse 的 Azure 工具組是開放原始碼專案。 原始程式碼位於 <https://github.com/microsoft/azure-tools-for-java> 的 MIT 授權下方。

下列步驟示範如何安裝 Azure Toolkit for Eclipse。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>安裝 Azure Toolkit for Eclipse

1. 啟動 Eclipse。

1. 按一下 [說明] 功能表，然後按一下 [安裝新軟體]，如下圖所示。
   
   ![安裝 Azure Toolkit for Eclipse][01]

1. 在 [可用軟體] 對話方塊的 [使用] 文字方塊中，鍵入 `http://dl.microsoft.com/eclipse/`，然後按一下 **Enter** 鍵。

1. 在 [名稱] 窗格中，核取 [Azure Toolkit for Eclipse]，然後取消核取 [在安裝期間連絡所有更新網站來尋找必要軟體]。 您的畫面看起來應該像下面這樣：
   
   ![安裝 Azure Toolkit for Eclipse][02]

1. 如果您展開 [適用於 Eclipse 的 Azure 工具組]，則會看到下列項目清單：
   
   * **Application Insights Plugin for Java**：這個元件可讓您將 Azure 的遙測記錄和分析服務用於應用程式和伺服器執行個體上。
   * **Azure Access Control Services Filter**：此元件提供利用 Azure ACS 驗證應用程式使用者、啟用單一登入案例以及從應用程式外部化身分識別邏輯的支援。
   * **Azure Common Plugin**：此元件提供其他工具組元件所需的一般功能。
   * **Azure Explorer for Eclipse**：此元件提供其他工具組元件所需的一般功能。
   * **Azure Plugin for Eclipse with Java**：此元件提供在 Eclipse 中與透過命令列開發可協助建置、測試和部署適用於 Microsoft Azure 雲端的 Java 應用程式之專案的支援。
   * **Azure Web Apps Plugin with Java**：此元件提供將 Java Web 應用程式部署到 Microsoft Azure Web 應用程式容器的支援。
   * **Microsoft JDBC Driver 4.2 for SQL Server**：這個元件提供適用於 SQL Server 的 JDBC API 以及適用於 Java Platform Enterprise Edition 8 的 Microsoft Azure SQL Database。
   * **Package for Apache Qpid Client Libraries for JMS**：此元件提供 Apache Qpid 專案的 JMS 用戶端元件，讓您的應用程式能在 Microsoft Azure 中使用 AMQP 訊息。
   * **Package for Microsoft Azure Libraries for Java**：此元件提供用來存取 Microsoft Azure 服務 (例如儲存體、服務匯流排、服務執行階段等) 的 API。

1. 按一下 [下一步] 。 (如果您在安裝此工具組時遇到不尋常的延遲，請確認已取消勾選 [在安裝期間連絡所有更新網站來尋找必要軟體]。)

1. 在 [安裝詳細資料] 對話方塊中，按 [下一步]。
   
   ![檢閱安裝詳細資料][03]

1. 在 [檢閱授權] 對話方塊中，檢閱授權合約的條款。 如果您接受授權合約的條款，請按一下 [我接受授權合約的條款]，然後按一下 [完成]。 (其餘的步驟假設您接受授權合約的條款。 如果您不接受授權合約的條款，請結束安裝程序。)
   
   ![檢閱授權][04]
   
   Eclipse 會下載並安裝必要的封裝。
   
   ![安裝進度][05]

1. 如果系統提示您重新啟動 Eclipse 以完成安裝，請按一下 [是] 。
   
   ![重新啟動提示][06]

## <a name="next-steps"></a>後續步驟

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->

[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
