---
title: "安裝 Azure Toolkit for IntelliJ"
description: "了解如何安裝 Azure Toolkit for the IntelliJ IDEA。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 10/19/2017
ms.author: robmcm
ms.openlocfilehash: 497aba02f55383bf0b32461752d6681867cdfac8
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2017
---
# <a name="installing-the-azure-toolkit-for-intellij"></a><span data-ttu-id="bca76-103">安裝 Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="bca76-103">Installing the Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="bca76-104">Azure Toolkit for IntelliJ 提供範本和功能，可讓您輕鬆地使用 IntelliJ IDEA 開發環境來建立、開發、測試及部署 Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bca76-104">The Azure Toolkit for IntelliJ provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the IntelliJ IDEA development environment.</span></span> <span data-ttu-id="bca76-105">Azure Toolkit for IntelliJ 是開放原始碼專案，其來源程式碼可從 GitHub 上該專案網站的 MIT License 下取得，URL 如下：</span><span class="sxs-lookup"><span data-stu-id="bca76-105">The Azure Toolkit for IntelliJ is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span>

<span data-ttu-id="bca76-106"><https://github.com/microsoft/azure-tools-for-java></span><span class="sxs-lookup"><span data-stu-id="bca76-106"><https://github.com/microsoft/azure-tools-for-java></span></span>

<span data-ttu-id="bca76-107">安裝 Azure Toolkit for IntelliJ 的方法有兩種，一種是從 [設定] 對話方塊，另一種是從啟動畫面上的 [設定] 功能表；下列步驟中將會示範這兩種安裝方法。</span><span class="sxs-lookup"><span data-stu-id="bca76-107">There are two methods of installing the Azure Toolkit for IntelliJ, from the Settings dialog box and from the Configure menu on the start screen; both installation methods will be demonstrated in the following steps.</span></span>

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a><span data-ttu-id="bca76-108">從 [設定] 對話方塊安裝 Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="bca76-108">To install the Azure Toolkit for IntelliJ from the settings dialog box</span></span>

1. <span data-ttu-id="bca76-109">啟動 IntelliJ IDEA。</span><span class="sxs-lookup"><span data-stu-id="bca76-109">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="bca76-110">IntelliJ IDEA 開啟時，按一下 [檔案]，然後按一下 [設定]。</span><span class="sxs-lookup"><span data-stu-id="bca76-110">When the IntelliJ IDEA opens, click **File**, then click **Settings**.</span></span>
   
   ![開啟 IntelliJ IDEA 的 [設定] 對話方塊][01a]

1. <span data-ttu-id="bca76-112">在 [設定] 對話方塊中，按一下 [外掛程式]，然後按一下 [瀏覽儲存機制]。</span><span class="sxs-lookup"><span data-stu-id="bca76-112">In the Settings dialog box, click **Plugins**, and then click **Browse repositories**.</span></span>
   
   ![IntelliJ IDEA [設定] 對話方塊][02a]

1. <span data-ttu-id="bca76-114">在 [瀏覽儲存機制] 對話方塊中，於 [搜尋] 方塊中輸入 "Azure"。</span><span class="sxs-lookup"><span data-stu-id="bca76-114">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="bca76-115">反白顯示 [適用於 IntelliJ 的 Azure 工具組]，然後按一下 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="bca76-115">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![搜尋 Azure Toolkit for IntelliJ][03]
   
   <span data-ttu-id="bca76-117">IntelliJ IDEA 會在對話方塊中顯示安裝進度。</span><span class="sxs-lookup"><span data-stu-id="bca76-117">IntelliJ IDEA displays the installation progress in a dialog box.</span></span>
   
   ![安裝進度][04]

1. <span data-ttu-id="bca76-119">安裝完成後，按一下 [重新啟動 IntelliJ IDEA] 。</span><span class="sxs-lookup"><span data-stu-id="bca76-119">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![重新啟動 IntelliJ IDEA][05]

1. <span data-ttu-id="bca76-121">按一下 [確定]  以關閉 [設定] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="bca76-121">Click **OK** to close the Settings dialog box.</span></span>
   
   ![關閉 IntelliJ IDEA [設定] 對話方塊][06]

1. <span data-ttu-id="bca76-123">當系統提示您重新啟動 IntelliJ IDEA 或延後，請按一下 [重新啟動] 。</span><span class="sxs-lookup"><span data-stu-id="bca76-123">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
<span data-ttu-id="bca76-124">1</span><span class="sxs-lookup"><span data-stu-id="bca76-124">1</span></span>   ![重新啟動 IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a><span data-ttu-id="bca76-126">從啟動畫面安裝 Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="bca76-126">To install the Azure Toolkit for IntelliJ from the start screen</span></span>

1. <span data-ttu-id="bca76-127">啟動 IntelliJ IDEA。</span><span class="sxs-lookup"><span data-stu-id="bca76-127">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="bca76-128">IntelliJ IDEA 啟動畫面出現時，按一下 [設定]，然後按一下 [外掛程式]。</span><span class="sxs-lookup"><span data-stu-id="bca76-128">When the IntelliJ IDEA start screen appears, click **Configure**, then click **Plugins**.</span></span>
   
   ![安裝 IntelliJ IDEA 外掛程式][01b]

1. <span data-ttu-id="bca76-130">在 [外掛程式] 對話方塊中，按一下 [瀏覽儲存機制]。</span><span class="sxs-lookup"><span data-stu-id="bca76-130">In the **Plugins** dialog box, click **Browse repositories**.</span></span>
   
   ![瀏覽 IntelliJ IDEA 外掛程式儲存機制][02b]

1. <span data-ttu-id="bca76-132">在 [瀏覽儲存機制] 對話方塊中，於 [搜尋] 方塊中輸入 "Azure"。</span><span class="sxs-lookup"><span data-stu-id="bca76-132">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="bca76-133">反白顯示 [適用於 IntelliJ 的 Azure 工具組]，然後按一下 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="bca76-133">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![搜尋 Azure Toolkit for IntelliJ][03]
   
   <span data-ttu-id="bca76-135">IntelliJ IDEA 會在對話方塊中顯示安裝進度。</span><span class="sxs-lookup"><span data-stu-id="bca76-135">IntelliJ IDEA will display the installation progress in a dialog box.</span></span>
   
   ![安裝進度][04]

1. <span data-ttu-id="bca76-137">安裝完成後，按一下 [重新啟動 IntelliJ IDEA] 。</span><span class="sxs-lookup"><span data-stu-id="bca76-137">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![重新啟動 IntelliJ IDEA][05]

1. <span data-ttu-id="bca76-139">當系統提示您重新啟動 IntelliJ IDEA 或延後，請按一下 [重新啟動] 。</span><span class="sxs-lookup"><span data-stu-id="bca76-139">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
   ![重新啟動 IntelliJ IDEA][07]

## <a name="next-steps"></a><span data-ttu-id="bca76-141">後續步驟</span><span class="sxs-lookup"><span data-stu-id="bca76-141">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[01a]: media/azure-toolkit-for-intellij-installation/01-intellij-file-settings.png
[01b]: media/azure-toolkit-for-intellij-installation/01-intellij-configure-dropdown.png
[02a]: media/azure-toolkit-for-intellij-installation/02-intellij-settings-dialog.png
[02b]: media/azure-toolkit-for-intellij-installation/02-intellij-plugins-dialog.png
[03]: media/azure-toolkit-for-intellij-installation/03-intellij-browse-repositories.png
[04]: media/azure-toolkit-for-intellij-installation/04-install-progress.png
[05]: media/azure-toolkit-for-intellij-installation/05-restart-intellij.png
[06]: media/azure-toolkit-for-intellij-installation/06-intellij-settings-dialog.png
[07]: media/azure-toolkit-for-intellij-installation/07-restart-intellij.png
