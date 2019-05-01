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
# <a name="install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="6affe-103">安裝適用於 Eclipse 的 Azure 工具組</span><span class="sxs-lookup"><span data-stu-id="6affe-103">Install the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="6affe-104">適用於 Eclipse 的 Azure 工具組提供範本和功能，可讓您輕鬆地從 Eclipse 開發環境來建立、開發、測試雲端應用程式並將其部署至 Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6affe-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy cloud applications to Azure from the Eclipse development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="6affe-105">Azure Toolkit for Eclipse 是開放原始碼專案，其來源程式碼可從 GitHub 上該專案網站的 MIT License 下取得，URL 如下：</span><span class="sxs-lookup"><span data-stu-id="6affe-105">The Azure Toolkit for Eclipse is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

<span data-ttu-id="6affe-106">下列步驟示範如何安裝 Azure Toolkit for Eclipse。</span><span class="sxs-lookup"><span data-stu-id="6affe-106">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="6affe-107">安裝 Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="6affe-107">To install the Azure Toolkit for Eclipse</span></span>

1. <span data-ttu-id="6affe-108">啟動 Eclipse。</span><span class="sxs-lookup"><span data-stu-id="6affe-108">Start Eclipse.</span></span>

1. <span data-ttu-id="6affe-109">按一下 [說明] 功能表，然後按一下 [安裝新軟體]，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="6affe-109">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
   ![安裝 Azure Toolkit for Eclipse][01]

1. <span data-ttu-id="6affe-111">在 [可用軟體] 對話方塊的 [使用] 文字方塊中，鍵入 `http://dl.microsoft.com/eclipse/`，然後按一下 **Enter** 鍵。</span><span class="sxs-lookup"><span data-stu-id="6affe-111">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse/` followed by the **Enter** key.</span></span>

1. <span data-ttu-id="6affe-112">在 [名稱] 窗格中，核取 [Azure Toolkit for Java]，然後取消核取 [在安裝期間連絡所有更新網站來尋找必要軟體]。</span><span class="sxs-lookup"><span data-stu-id="6affe-112">In the **Name** pane, check **Azure Toolkit for Java**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="6affe-113">您的畫面看起來應該像下面這樣：</span><span class="sxs-lookup"><span data-stu-id="6affe-113">Your screen should appear similar to the following:</span></span>
   
   ![安裝 Azure Toolkit for Eclipse][02]

1. <span data-ttu-id="6affe-115">如果您展開**適用於 Eclipse 的 Azure 工具組**，您會看到將要安裝的元件清單，例如：</span><span class="sxs-lookup"><span data-stu-id="6affe-115">If you expand **Azure Toolkit for Eclipse**, you will see a list of components that will be installed; for example:</span></span>

   | <span data-ttu-id="6affe-116">功能</span><span class="sxs-lookup"><span data-stu-id="6affe-116">Feature</span></span> | <span data-ttu-id="6affe-117">說明</span><span class="sxs-lookup"><span data-stu-id="6affe-117">Description</span></span> | 
   |---|---| 
   | <span data-ttu-id="6affe-118">**適用於 Java 的 Application Insights 外掛程式**</span><span class="sxs-lookup"><span data-stu-id="6affe-118">**Application Insights Plugin for Java**</span></span> | <span data-ttu-id="6affe-119">可讓您將 Azure 的遙測記錄和分析服務用於應用程式和伺服器執行個體上。</span><span class="sxs-lookup"><span data-stu-id="6affe-119">Allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span> | 
   | <span data-ttu-id="6affe-120">**Azure Common 外掛程式**</span><span class="sxs-lookup"><span data-stu-id="6affe-120">**Azure Common Plugin**</span></span> | <span data-ttu-id="6affe-121">提供其他工具組元件所需的一般功能。</span><span class="sxs-lookup"><span data-stu-id="6affe-121">provides the common functionality needed by other toolkit components.</span></span> | 
   | <span data-ttu-id="6affe-122">**適用於 Eclipse 的 Azure 容器工具**</span><span class="sxs-lookup"><span data-stu-id="6affe-122">**Azure Container Tools for Eclipse**</span></span> | <span data-ttu-id="6affe-123">可讓您將 .WAR 作為 Docker 容器建置及部署至 Docker 機器。</span><span class="sxs-lookup"><span data-stu-id="6affe-123">Enables you to build and deploy a .WAR as a Docker container to a docker machine.</span></span> | 
   | <span data-ttu-id="6affe-124">**適用於 Eclipse 的 Azure 容器**</span><span class="sxs-lookup"><span data-stu-id="6affe-124">**Azure Containers for Eclipse**</span></span> | <span data-ttu-id="6affe-125">可讓您將 .WAR 或 .JAR 構件作為 Docker 容器，部署至 Azure 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="6affe-125">Enables you to deploy a .WAR or .JAR artifact as a Docker container to an Azure virtual machine.</span></span> | 
   | <span data-ttu-id="6affe-126">**適用於 Eclipse 的 Azure Explorer**</span><span class="sxs-lookup"><span data-stu-id="6affe-126">**Azure Explorer for Eclipse**</span></span> | <span data-ttu-id="6affe-127">提供用來管理 Azure 資源的總管樣式介面。</span><span class="sxs-lookup"><span data-stu-id="6affe-127">Provides an explorer-style interface for managing your Azure resources.</span></span> | 
   | <span data-ttu-id="6affe-128">**適用於 SQL Server 的 Microsoft JDBC Driver 6.1**</span><span class="sxs-lookup"><span data-stu-id="6affe-128">**Microsoft JDBC Driver 6.1 for SQL Server**</span></span> | <span data-ttu-id="6affe-129">提供適用於 SQL Server 的 JDBC API 以及適用於 Java Platform Enterprise Edition 8 的 Microsoft Azure SQL Database。</span><span class="sxs-lookup"><span data-stu-id="6affe-129">Provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span> | 
   | <span data-ttu-id="6affe-130">**適用於 Java 的 Microsoft Azure 程式庫的套件**</span><span class="sxs-lookup"><span data-stu-id="6affe-130">**Package for Microsoft Azure Libraries for Java**</span></span> | <span data-ttu-id="6affe-131">提供用來存取 Microsoft Azure 服務 (例如儲存體、服務匯流排、服務執行階段等) 的 API。</span><span class="sxs-lookup"><span data-stu-id="6affe-131">Provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span> | 

1. <span data-ttu-id="6affe-132">按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="6affe-132">Click **Next**.</span></span> <span data-ttu-id="6affe-133">(如果您在安裝此工具組時遇到不尋常的延遲，請確認已取消勾選 [在安裝期間連絡所有更新網站來尋找必要軟體]。)</span><span class="sxs-lookup"><span data-stu-id="6affe-133">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>

1. <span data-ttu-id="6affe-134">在 [安裝詳細資料] 對話方塊中，按 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="6affe-134">In the **Install Details** dialog, click **Next**.</span></span>
   
   ![檢閱安裝詳細資料][03]

1. <span data-ttu-id="6affe-136">在 [檢閱授權] 對話方塊中，檢閱授權合約的條款。</span><span class="sxs-lookup"><span data-stu-id="6affe-136">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="6affe-137">如果您接受授權合約的條款，請按一下 [我接受授權合約的條款]，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="6affe-137">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="6affe-138">(其餘的步驟假設您接受授權合約的條款。</span><span class="sxs-lookup"><span data-stu-id="6affe-138">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="6affe-139">如果您不接受授權合約的條款，請結束安裝程序。)</span><span class="sxs-lookup"><span data-stu-id="6affe-139">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
   ![檢閱授權][04]
   
   <span data-ttu-id="6affe-141">Eclipse 會下載並安裝必要的封裝。</span><span class="sxs-lookup"><span data-stu-id="6affe-141">Eclipse will download and install the requisite packages.</span></span>
   
   ![安裝進度][05]

1. <span data-ttu-id="6affe-143">如果系統提示您重新啟動 Eclipse 以完成安裝，請按一下 [是] 。</span><span class="sxs-lookup"><span data-stu-id="6affe-143">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
   ![重新啟動提示][06]

## <a name="next-steps"></a><span data-ttu-id="6affe-145">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6affe-145">Next steps</span></span>

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
