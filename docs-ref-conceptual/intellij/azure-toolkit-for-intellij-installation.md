---
title: 安裝 Azure Toolkit for IntelliJ
description: 了解如何安裝適用於 intelliJ 的 Azure 工具組，以建立雲端應用程式並將其部署至 Azure。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 0f3df5c8cf3c83c1ce9a4ca32c753b5fc39ab1df
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954169"
---
# <a name="installing-the-azure-toolkit-for-intellij"></a><span data-ttu-id="afcdb-103">安裝 Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="afcdb-103">Installing the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="afcdb-104">Azure Toolkit for IntelliJ 提供範本和功能，可讓您輕鬆地使用 IntelliJ IDEA 開發環境建立、開發、測試雲端應用程式，並將其部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="afcdb-104">The Azure Toolkit for IntelliJ provides templates and functionality that allow you to easily create, develop, test, and deploy cloud application to Azure using the IntelliJ IDEA development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="afcdb-105">Azure Toolkit for IntelliJ 是開放原始碼專案，其來源程式碼可從 GitHub 上該專案網站的 MIT License 下取得，URL 如下：</span><span class="sxs-lookup"><span data-stu-id="afcdb-105">The Azure Toolkit for IntelliJ is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <span data-ttu-id="afcdb-106"><https://github.com/microsoft/azure-tools-for-java></span><span class="sxs-lookup"><span data-stu-id="afcdb-106"><https://github.com/microsoft/azure-tools-for-java></span></span> 
> 

<span data-ttu-id="afcdb-107">安裝適用於 IntelliJ 的 Azure 工具組有兩個方法：使用 [設定] 對話方塊，以及使用開始畫面上的 [設定] 功能表。</span><span class="sxs-lookup"><span data-stu-id="afcdb-107">There are two methods of installing the Azure Toolkit for IntelliJ: by using the **Settings** dialog box, and by using the **Configure** menu on the start screen.</span></span> <span data-ttu-id="afcdb-108">下列步驟會示範這兩種安裝方法。</span><span class="sxs-lookup"><span data-stu-id="afcdb-108">Both installation methods will be demonstrated in the following steps.</span></span>

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a><span data-ttu-id="afcdb-109">從 [設定] 對話方塊安裝 Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="afcdb-109">To install the Azure Toolkit for IntelliJ from the settings dialog box</span></span>

1. <span data-ttu-id="afcdb-110">啟動 IntelliJ IDEA。</span><span class="sxs-lookup"><span data-stu-id="afcdb-110">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="afcdb-111">IntelliJ IDEA 開啟時，按一下 [檔案]，然後按一下 [設定]。</span><span class="sxs-lookup"><span data-stu-id="afcdb-111">When the IntelliJ IDEA opens, click **File**, then click **Settings**.</span></span>
   
   ![開啟 IntelliJ IDEA 的 [設定] 對話方塊][01a]

1. <span data-ttu-id="afcdb-113">在 [設定] 對話方塊中，按一下 [外掛程式]，然後按一下 [瀏覽儲存機制]。</span><span class="sxs-lookup"><span data-stu-id="afcdb-113">In the Settings dialog box, click **Plugins**, and then click **Browse repositories**.</span></span>
   
   ![IntelliJ IDEA [設定] 對話方塊][02a]

1. <span data-ttu-id="afcdb-115">在 [瀏覽儲存機制] 對話方塊中，於 [搜尋] 方塊中輸入 "Azure"。</span><span class="sxs-lookup"><span data-stu-id="afcdb-115">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="afcdb-116">反白顯示 [適用於 IntelliJ 的 Azure 工具組]，然後按一下 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="afcdb-116">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![搜尋 Azure Toolkit for IntelliJ][03]
   
   <span data-ttu-id="afcdb-118">IntelliJ IDEA 會在對話方塊中顯示安裝進度。</span><span class="sxs-lookup"><span data-stu-id="afcdb-118">IntelliJ IDEA displays the installation progress in a dialog box.</span></span>
   
   ![安裝進度][04]

1. <span data-ttu-id="afcdb-120">安裝完成後，按一下 [重新啟動 IntelliJ IDEA] 。</span><span class="sxs-lookup"><span data-stu-id="afcdb-120">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![重新啟動 IntelliJ IDEA][05]

1. <span data-ttu-id="afcdb-122">按一下 [確定]  以關閉 [設定] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="afcdb-122">Click **OK** to close the Settings dialog box.</span></span>
   
   ![關閉 IntelliJ IDEA [設定] 對話方塊][06]

1. <span data-ttu-id="afcdb-124">當系統提示您重新啟動 IntelliJ IDEA 或延後，請按一下 [重新啟動] 。</span><span class="sxs-lookup"><span data-stu-id="afcdb-124">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
<span data-ttu-id="afcdb-125">1</span><span class="sxs-lookup"><span data-stu-id="afcdb-125">1</span></span>   ![重新啟動 IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a><span data-ttu-id="afcdb-127">從啟動畫面安裝 Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="afcdb-127">To install the Azure Toolkit for IntelliJ from the start screen</span></span>

1. <span data-ttu-id="afcdb-128">啟動 IntelliJ IDEA。</span><span class="sxs-lookup"><span data-stu-id="afcdb-128">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="afcdb-129">IntelliJ IDEA 啟動畫面出現時，按一下 [設定]，然後按一下 [外掛程式]。</span><span class="sxs-lookup"><span data-stu-id="afcdb-129">When the IntelliJ IDEA start screen appears, click **Configure**, then click **Plugins**.</span></span>
   
   ![安裝 IntelliJ IDEA 外掛程式][01b]

1. <span data-ttu-id="afcdb-131">在 [外掛程式] 對話方塊中，按一下 [瀏覽儲存機制]。</span><span class="sxs-lookup"><span data-stu-id="afcdb-131">In the **Plugins** dialog box, click **Browse repositories**.</span></span>
   
   ![瀏覽 IntelliJ IDEA 外掛程式儲存機制][02b]

1. <span data-ttu-id="afcdb-133">在 [瀏覽儲存機制] 對話方塊中，於 [搜尋] 方塊中輸入 "Azure"。</span><span class="sxs-lookup"><span data-stu-id="afcdb-133">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="afcdb-134">反白顯示 [適用於 IntelliJ 的 Azure 工具組]，然後按一下 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="afcdb-134">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![搜尋 Azure Toolkit for IntelliJ][03]
   
   <span data-ttu-id="afcdb-136">IntelliJ IDEA 會在對話方塊中顯示安裝進度。</span><span class="sxs-lookup"><span data-stu-id="afcdb-136">IntelliJ IDEA will display the installation progress in a dialog box.</span></span>
   
   ![安裝進度][04]

1. <span data-ttu-id="afcdb-138">安裝完成後，按一下 [重新啟動 IntelliJ IDEA] 。</span><span class="sxs-lookup"><span data-stu-id="afcdb-138">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![重新啟動 IntelliJ IDEA][05]

1. <span data-ttu-id="afcdb-140">當系統提示您重新啟動 IntelliJ IDEA 或延後，請按一下 [重新啟動] 。</span><span class="sxs-lookup"><span data-stu-id="afcdb-140">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
   ![重新啟動 IntelliJ IDEA][07]

## <a name="next-steps"></a><span data-ttu-id="afcdb-142">後續步驟</span><span class="sxs-lookup"><span data-stu-id="afcdb-142">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

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
