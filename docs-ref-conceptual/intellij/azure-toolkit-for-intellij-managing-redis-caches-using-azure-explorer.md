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
ms.openlocfilehash: 046ae0428d50a7f173f5ad15be53ffd8e66c11c5
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a>使用適用於 IntelliJ 的 Azure Explorer 來管理 Redis 快取

Azure Explorer 是適用於 IntelliJ 的 Azure 工具組一部分，可為 Java 開發人員提供易於使用的解決方案，從 IntelliJ IDE 內管理其 Azure 帳戶中的 Redis 快取。

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a>使用 IntelliJ 建立 Redis 快取

下列步驟將逐步引導您使用 Azure Explorer 建立 Redis 快取的步驟。

1. 使用[適用於 IntelliJ 的 Azure 工具組登入指示]文章中的步驟來登入您的 Azure 帳戶。

1. 在 [Azure Explorer] 工具視窗中，展開 **Azure** 節點，以滑鼠右鍵按一下 [Redis 快取]，然後按一下 [建立 Redis 快取]。

   ![建立 Redis 快取功能表][CR01]

1. 當 [新增 Redis 快取] 對話方塊出現時，請指定下列選項：

   ![建立新的 Redis 快取對話方塊][CR02]

   a. **DNS 名稱**：指定新 Redis 快取的 DNS 子網域，會在前面加上 ".redis.cache.windows.net"；例如：wingtiptoys.redis.cache.windows.net。

   b. **訂用帳戶**：指定您要用於新 Redis 快取的 Azure 訂用帳戶。

   c. **資源群組**︰指定 Redis 快取的資源群組；您必須選擇下列選項之一︰ 
      * **新建**：指定您想要建立新的資源群組。 
      * **使用現有**︰指定您將從與您 Azure 帳戶相關聯的資源群組清單中進行選擇。 

   d. **位置**︰指定要建立 Redis 快取的位置；例如「美國西部」。

   e. **定價層**：指定您 Redis 快取所使用的定價層；此設定會決定用戶端連線的數目。 (如需詳細資訊，請參閱 [Redis 快取定價]。)

   f. **非 SSL 連接埠**：指定 Redis 快取是否可允許非 SSL 連線；預設只允許 SSL 連線。

1. 指定所有的 Redis 快取設定後，請按一下 [確定]。

建立 Redis 快取之後，它就會顯示在 Azure Explorer 中。

   ![Azure Explorer 中的 Redis 快取][CR03]

> [!NOTE]
>
> 如需設定 Azure Redis 快取設定的詳細資訊，請參閱 [如何設定 Azure Redis 快取]。
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a>在 IntelliJ 中顯示 Redis 快取的屬性

1. 在 Azure Explorer 中，以滑鼠右鍵按一下您的 Redis 快取，然後按一下 [顯示屬性]。

   ![顯示 Redis 快取屬性的 Azure Explorer 操作功能表][SP01]

1. Azure Explorer 會顯示您 Redis 快取的屬性。

   ![Redis 快取屬性][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a>使用 IntelliJ 刪除 Redis 快取

1. 在 Azure Explorer 中，以滑鼠右鍵按一下您的 Redis 快取，然後按一下 [刪除]。

   ![刪除 Redis 快取的 Azure Explorer 操作功能表][DE01]

1. 當系統提示您刪除 Redis 快取時，按一下 [是]。

   ![刪除 Redis 快取提示][DE02]

## <a name="next-steps"></a>後續步驟

如需有關 Azure Redis 快取、組態集和定價的詳細資訊，請參閱下列連結：

* [Azure Redis 快取]
* [Redis 快取文件]
* [Redis 快取定價]
* [如何設定 Azure Redis 快取]

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Redis 快取定價]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis 快取]: https://azure.microsoft.com/services/cache/
[Redis 快取文件]: /azure/redis-cache
[如何設定 Azure Redis 快取]: /azure/redis-cache/cache-configure
[適用於 IntelliJ 的 Azure 工具組登入指示]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
