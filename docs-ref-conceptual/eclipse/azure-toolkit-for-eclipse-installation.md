---
title: 安裝適用於 Eclipse 的 Azure 工具組
description: 了解如何安裝適用於 Eclipse 外掛程式的 Azure 工具組，以建立雲端應用程式並將其部署至 Azure。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: bcc5ed04143eebaff89e5688a818e464f077390e
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61590663"
---
# <a name="install-the-azure-toolkit-for-eclipse"></a>安裝適用於 Eclipse 的 Azure 工具組

適用於 Eclipse 的 Azure 工具組提供範本和功能，可讓您輕鬆地從 Eclipse 開發環境來建立、開發、測試雲端應用程式並將其部署至 Azure 應用程式。

> [!NOTE] 
> 
> Azure Toolkit for Eclipse 是開放原始碼專案，其來源程式碼可從 GitHub 上該專案網站的 MIT License 下取得，URL 如下： 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

下列步驟示範如何安裝 Azure Toolkit for Eclipse。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>安裝 Azure Toolkit for Eclipse

1. 啟動 Eclipse。

1. 按一下 [說明] 功能表，然後按一下 [安裝新軟體]，如下圖所示。
   
   ![安裝 Azure Toolkit for Eclipse][01]

1. 在 [可用軟體] 對話方塊的 [使用] 文字方塊中，鍵入 `http://dl.microsoft.com/eclipse/`，然後按一下 **Enter** 鍵。

1. 在 [名稱] 窗格中，核取 [Azure Toolkit for Java]，然後取消核取 [在安裝期間連絡所有更新網站來尋找必要軟體]。 您的畫面看起來應該像下面這樣：
   
   ![安裝 Azure Toolkit for Eclipse][02]

1. 如果您展開**適用於 Eclipse 的 Azure 工具組**，您會看到將要安裝的元件清單，例如：

   | 功能 | 說明 | 
   |---|---| 
   | **適用於 Java 的 Application Insights 外掛程式** | 可讓您將 Azure 的遙測記錄和分析服務用於應用程式和伺服器執行個體上。 | 
   | **Azure Common 外掛程式** | 提供其他工具組元件所需的一般功能。 | 
   | **適用於 Eclipse 的 Azure 容器工具** | 可讓您將 .WAR 作為 Docker 容器建置及部署至 Docker 機器。 | 
   | **適用於 Eclipse 的 Azure 容器** | 可讓您將 .WAR 或 .JAR 構件作為 Docker 容器，部署至 Azure 虛擬機器。 | 
   | **適用於 Eclipse 的 Azure Explorer** | 提供用來管理 Azure 資源的總管樣式介面。 | 
   | **適用於 SQL Server 的 Microsoft JDBC Driver 6.1** | 提供適用於 SQL Server 的 JDBC API 以及適用於 Java Platform Enterprise Edition 8 的 Microsoft Azure SQL Database。 | 
   | **適用於 Java 的 Microsoft Azure 程式庫的套件** | 提供用來存取 Microsoft Azure 服務 (例如儲存體、服務匯流排、服務執行階段等) 的 API。 | 

1. 按 [下一步] 。 (如果您在安裝此工具組時遇到不尋常的延遲，請確認已取消勾選 [在安裝期間連絡所有更新網站來尋找必要軟體]。)

1. 在 [安裝詳細資料] 對話方塊中，按 [下一步]。
   
   ![檢閱安裝詳細資料][03]

1. 在 [檢閱授權] 對話方塊中，檢閱授權合約的條款。 如果您接受授權合約的條款，請按一下 [我接受授權合約的條款]，然後按一下 [完成]。 (其餘的步驟假設您接受授權合約的條款。 如果您不接受授權合約的條款，請結束安裝程序。)
   
   ![檢閱授權][04]
   
   Eclipse 會下載並安裝必要的封裝。
   
   ![安裝進度][05]

1. 如果系統提示您重新啟動 Eclipse 以完成安裝，請按一下 [是] 。
   
   ![重新啟動提示][06]

## <a name="next-steps"></a>後續步驟

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->

[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
