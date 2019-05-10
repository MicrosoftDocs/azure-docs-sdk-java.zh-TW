---
title: 使用適用於 IntelliJ 的 Azure Explorer 來管理 Redis 快取
description: 了解如何使用適用於 IntelliJ 的 Azure Explorer 來管理 Azure Redis 快取。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: ee5d286f3c5979b3a10e3e3be08a68caa6eef31d
ms.sourcegitcommit: 420fcd58e3a907d629c9c15fda56ad818d9dfe7c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65470187"
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="6a138-103">使用適用於 IntelliJ 的 Azure Explorer 來管理 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="6a138-103">Managing Redis Caches using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="6a138-104">Azure Explorer 是適用於 IntelliJ 的 Azure 工具組一部分，可為 Java 開發人員提供易於使用的解決方案，從 IntelliJ IDE 內管理其 Azure 帳戶中的 Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="6a138-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside the IntelliJ IDE.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a><span data-ttu-id="6a138-105">使用 IntelliJ 建立 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="6a138-105">Create a Redis Cache by using IntelliJ</span></span>

<span data-ttu-id="6a138-106">下列步驟將逐步引導您使用 Azure Explorer 建立 Redis 快取的步驟。</span><span class="sxs-lookup"><span data-stu-id="6a138-106">The following steps walk you through the steps to create a redis cache using the Azure Explorer.</span></span>

1. <span data-ttu-id="6a138-107">使用[適用於 IntelliJ 的 Azure 工具組登入指示]文章中的步驟來登入您的 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="6a138-107">Sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="6a138-108">在 [Azure Explorer] 工具視窗中，展開 **Azure** 節點，以滑鼠右鍵按一下 [Redis 快取]，然後按一下 [建立 Redis 快取]。</span><span class="sxs-lookup"><span data-stu-id="6a138-108">In the **Azure Explorer** tool window, expand the **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![建立 Redis 快取功能表][CR01]

1. <span data-ttu-id="6a138-110">當 [新增 Redis 快取] 對話方塊出現時，請指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="6a138-110">When the **New Redis Cache** dialog box appears, specify the following options:</span></span>

   ![建立新的 Redis 快取對話方塊][CR02]

   <span data-ttu-id="6a138-112">a.</span><span class="sxs-lookup"><span data-stu-id="6a138-112">a.</span></span> <span data-ttu-id="6a138-113">**DNS 名稱**：指定新 Redis 快取的 DNS 子網域，會在前面加上 ".redis.cache.windows.net"；例如：wingtiptoys.redis.cache.windows.net。</span><span class="sxs-lookup"><span data-stu-id="6a138-113">**DNS Name**: Specifies the DNS subdomain for the new redis cache, which are prepended to ".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="6a138-114">b.</span><span class="sxs-lookup"><span data-stu-id="6a138-114">b.</span></span> <span data-ttu-id="6a138-115">訂用帳戶：指定您要用於新 Redis 快取的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="6a138-115">**Subscription**: Specifies the Azure subscription you want to use for the new redis cache.</span></span>

   <span data-ttu-id="6a138-116">c.</span><span class="sxs-lookup"><span data-stu-id="6a138-116">c.</span></span> <span data-ttu-id="6a138-117">**資源群組**：指定 Redis 快取的資源群組；您必須選擇下列選項之一︰</span><span class="sxs-lookup"><span data-stu-id="6a138-117">**Resource Group**: Specifies the resource group for your redis cache; you need to choose one of the following options:</span></span> 
      * <span data-ttu-id="6a138-118">**建立新項目**：指定您想要建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="6a138-118">**Create New**: Specifies that you want to create a new resource group.</span></span> 
      * <span data-ttu-id="6a138-119">**使用現有項目**︰指定您將從與您 Azure 帳戶相關聯的資源群組清單中進行選擇。</span><span class="sxs-lookup"><span data-stu-id="6a138-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span> 

   <span data-ttu-id="6a138-120">d.</span><span class="sxs-lookup"><span data-stu-id="6a138-120">d.</span></span> <span data-ttu-id="6a138-121">**位置**：指定要建立 Redis 快取的位置；例如「美國西部」。</span><span class="sxs-lookup"><span data-stu-id="6a138-121">**Location**: Specifies the location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="6a138-122">e.</span><span class="sxs-lookup"><span data-stu-id="6a138-122">e.</span></span> <span data-ttu-id="6a138-123">**定價層**：指定您 Redis 快取所使用的定價層；此設定會決定用戶端連線的數目。</span><span class="sxs-lookup"><span data-stu-id="6a138-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines the number of client connections.</span></span> <span data-ttu-id="6a138-124">(如需詳細資訊，請參閱 [Redis 快取定價]。)</span><span class="sxs-lookup"><span data-stu-id="6a138-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="6a138-125">f.</span><span class="sxs-lookup"><span data-stu-id="6a138-125">f.</span></span> <span data-ttu-id="6a138-126">**非 SSL 連接埠**：指定 Redis 快取是否可允許非 SSL 連線；預設只允許 SSL 連線。</span><span class="sxs-lookup"><span data-stu-id="6a138-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="6a138-127">指定所有的 Redis 快取設定後，請按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="6a138-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="6a138-128">建立 Redis 快取之後，它就會顯示在 Azure Explorer 中。</span><span class="sxs-lookup"><span data-stu-id="6a138-128">After your redis cache has been created, it will be displayed in the Azure Explorer.</span></span>

   ![Azure Explorer 中的 Redis 快取][CR03]

> [!NOTE]
>
> <span data-ttu-id="6a138-130">如需設定 Azure Redis 快取設定的詳細資訊，請參閱 [如何設定 Azure Redis 快取]。</span><span class="sxs-lookup"><span data-stu-id="6a138-130">For more information about configuring your Azure redis cache settings, see [How to configure Azure Redis Cache].</span></span>
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a><span data-ttu-id="6a138-131">在 IntelliJ 中顯示 Redis 快取的屬性</span><span class="sxs-lookup"><span data-stu-id="6a138-131">Display the properties for your Redis Cache in IntelliJ</span></span>

1. <span data-ttu-id="6a138-132">在 Azure Explorer 中，以滑鼠右鍵按一下您的 Redis 快取，然後按一下 [顯示屬性]。</span><span class="sxs-lookup"><span data-stu-id="6a138-132">In the Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![顯示 Redis 快取屬性的 Azure Explorer 操作功能表][SP01]

1. <span data-ttu-id="6a138-134">Azure Explorer 會顯示您 Redis 快取的屬性。</span><span class="sxs-lookup"><span data-stu-id="6a138-134">The Azure Explorer displays the properties for your redis cache.</span></span>

   ![Redis 快取屬性][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a><span data-ttu-id="6a138-136">使用 IntelliJ 刪除 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="6a138-136">Delete your Redis Cache by using IntelliJ</span></span>

1. <span data-ttu-id="6a138-137">在 Azure Explorer 中，以滑鼠右鍵按一下您的 Redis 快取，然後按一下 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="6a138-137">In the Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![刪除 Redis 快取的 Azure Explorer 操作功能表][DE01]

1. <span data-ttu-id="6a138-139">當系統提示您刪除 Redis 快取時，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="6a138-139">Click **Yes** when prompted to delete your redis cache.</span></span>

   ![刪除 Redis 快取提示][DE02]

## <a name="next-steps"></a><span data-ttu-id="6a138-141">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6a138-141">Next steps</span></span>

<span data-ttu-id="6a138-142">如需有關 Azure Redis 快取、組態集和定價的詳細資訊，請參閱下列連結：</span><span class="sxs-lookup"><span data-stu-id="6a138-142">For more information about Azure redis caches, configuration settings and pricing, see the following links:</span></span>

* <span data-ttu-id="6a138-143">[Azure Redis 快取]</span><span class="sxs-lookup"><span data-stu-id="6a138-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="6a138-144">[Redis 快取文件]</span><span class="sxs-lookup"><span data-stu-id="6a138-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="6a138-145">[Redis 快取定價]</span><span class="sxs-lookup"><span data-stu-id="6a138-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="6a138-146">[如何設定 Azure Redis 快取]</span><span class="sxs-lookup"><span data-stu-id="6a138-146">[How to configure Azure Redis Cache]</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Redis 快取定價]: https://azure.microsoft.com/pricing/details/cache/
[Redis Cache Pricing]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis 快取]: https://azure.microsoft.com/services/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis 快取文件]: /azure/redis-cache
[Redis Cache Documentation]: /azure/redis-cache
[如何設定 Azure Redis 快取]: /azure/redis-cache/cache-configure
[How to configure Azure Redis Cache]: /azure/redis-cache/cache-configure
[適用於 IntelliJ 的 Azure 工具組登入指示]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
