---
title: "使用適用於 IntelliJ 的 Azure Explorer 來管理儲存體帳戶"
description: "了解如何使用適用於 IntelliJ 的 Azure 工具組來管理 Azure 儲存體帳戶。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 4edb8c1ceef508dd251db693ccc3b98d77ec452b
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>使用適用於 IntelliJ 的 Azure Explorer 來管理儲存體帳戶

Azure Explorer 是適用於 IntelliJ 的 Azure 工具組一部分，可為 Java 開發人員提供易於使用的解決方案，從 IntelliJ 整合式開發環境 (IDE) 內管理其 Azure 帳戶中的儲存體帳戶。

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a>在 IntelliJ 中建立儲存體帳戶

若要使用 Azure Explorer 來建立儲存體帳戶，請執行下列作業︰

1. 使用 [適用於 IntelliJ 的 Azure 工具組登入指示] 來登入您的 Azure 帳戶。 

2. 在 [Azure Explorer] 檢視中，展開 [Azure] 節點，以滑鼠右鍵按一下 [儲存體帳戶]，然後按一下 [建立儲存體帳戶]。

   ![[建立儲存體帳戶] 命令][CS01]

3. 在 [建立儲存體帳戶] 對話方塊中，指定下列選項：

   ![[新建儲存體帳戶] 對話方塊][CS02]

   * **名稱**：指定新儲存體帳戶的名稱。

   * **帳戶種類**︰指定要建立的儲存體帳戶類型 (例如「Blob 儲存體」)。 如需詳細資訊，請參閱[關於 Azure 儲存體帳戶]。 

   * **效能**︰從選取的發行者指定要使用的儲存體帳戶供應項目 (例如「進階」)。 如需詳細資訊，請參閱 [Azure 儲存體延展性和效能目標]。 

   * **複寫**︰指定儲存體帳戶的複寫 (例如「區域備援」)。 如需詳細資訊，請參閱 [Azure 儲存體複寫]。 

   * **訂用帳戶**：指定您要用於新儲存體帳戶的 Azure 訂用帳戶。

   * **位置**︰指定要建立儲存體帳戶的位置 (例如「美國西部」)。

   * **資源群組**︰指定虛擬機器的資源群組。 選取下列其中一個選項：
      * **新建**：指定您想要建立新的資源群組。
      * **使用現有項目**︰指定您會從 Azure 帳戶相關聯的資源群組清單中進行選擇。

4. 當您指定好上述所有選項時，按一下 [確定]。

## <a name="create-a-storage-container-in-intellij"></a>在 IntelliJ 中建立儲存體容器

若要使用 Azure Explorer 來建立儲存體容器，請執行下列作業︰

1. 在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下您要在其中建立容器的儲存體帳戶，然後按一下 [建立 Blob 容器]。

   ![[建立 Blob 容器] 命令][CC01]

2. 在 [建立 Blob 容器] 對話方塊中指定容器名稱，然後按一下 [確定]。 如需為儲存體容器命名的詳細資訊，請參閱[命名和參考容器、Blob 及中繼資料]。

   ![[建立儲存體容器] 對話方塊][CC02]

## <a name="delete-a-storage-container-in-intellij"></a>在 IntelliJ 中刪除儲存體容器

若要使用 Azure Explorer 來刪除儲存體容器，請執行下列作業︰

1. 在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下儲存體容器，然後按一下 [刪除]。

   ![[刪除儲存體容器] 命令][DC01]

2. 在確認視窗中，按一下 [是]。

   ![刪除儲存體容器的確認視窗][DC02]

## <a name="delete-a-storage-account-in-intellij"></a>在 IntelliJ 中刪除儲存體帳戶

若要使用 Azure Explorer 來刪除儲存體帳戶，請執行下列作業︰

1. 在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下儲存體帳戶，然後選取 [刪除]。

   ![[刪除] 儲存體帳戶功能表][DS01]

2. 在確認視窗中，按一下 [是]。

   ![刪除儲存體帳戶的確認視窗][DS02]

## <a name="next-steps"></a>後續步驟

如需 Azure 儲存體帳戶、大小與定價的詳細資訊，請參閱下列資源︰

* [Microsoft Azure 儲存體簡介]
* [關於 Azure 儲存體帳戶]
* Azure 儲存體帳戶大小
  * [Azure 中的 Windows 儲存體帳戶大小]
  * [Azure 中的 Linux 儲存體帳戶大小]
* Azure 儲存體帳戶定價
  * [Windows 儲存體帳戶定價]
  * [Linux 儲存體帳戶定價]

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[適用於 IntelliJ 的 Azure 工具組登入指示]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Microsoft Azure 儲存體簡介]: /azure/storage/storage-introduction
[關於 Azure 儲存體帳戶]: /azure/storage/storage-create-storage-account
[Azure 儲存體複寫]: /azure/storage/storage-redundancy
[Azure 儲存體延展性和效能目標]: /azure/storage/storage-scalability-targets
[命名和參考容器、Blob 及中繼資料]: http://go.microsoft.com/fwlink/?LinkId=255555

[Azure 中的 Windows 儲存體帳戶大小]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure 中的 Linux 儲存體帳戶大小]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 儲存體帳戶定價]: /pricing/details/virtual-machines/windows/
[Linux 儲存體帳戶定價]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
