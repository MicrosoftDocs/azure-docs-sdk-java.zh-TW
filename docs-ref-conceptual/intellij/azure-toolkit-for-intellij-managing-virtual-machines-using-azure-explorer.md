---
title: 使用適用於 IntelliJ 的 Azure Explorer 來管理虛擬機器
description: 了解如何使用適用於 IntelliJ 的 Azure 工具組來管理 Azure 虛擬機器。
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
ms.openlocfilehash: 213efa7fc31705b0ffcba6f2fe40e7186a365fae
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-intellij"></a>使用適用於 IntelliJ 的 Azure Explorer 來管理虛擬機器

Azure Explorer 是適用於 IntelliJ 的 Azure 工具組一部分，可為 Java 開發人員提供易於使用的解決方案，從 IntelliJ 整合式開發環境 (IDE) 內管理其 Azure 帳戶中的虛擬機器。

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a>在 IntelliJ 中建立虛擬機器

若要使用 Azure Explorer 來建立虛擬機器，請執行下列作業︰ 

1. 使用[適用於 IntelliJ 的 Azure 工具組登入指示]文章中的步驟來登入您的 Azure 帳戶。

2. 在 [Azure Explorer] 檢視中，展開 [Azure] 節點，以滑鼠右鍵按一下 [虛擬機器]，然後按一下 [建立 VM]。 

   ![建立 VM 命令][CR01]  
    [建立新的虛擬機器] 精靈隨即開啟。

3. 在 [選擇訂用帳戶] 視窗中選取您的訂用帳戶，然後按 [下一步]。 

   ![選擇訂用帳戶視窗][CR02]

4. 在 [選取虛擬機器映像] 視窗中，輸入下列資訊︰

   * **位置**︰指定要建立虛擬機器的位置 (例如「美國西部」)。 

   * **建議的映像**︰指定您會從常用映像的簡短清單中選擇映像。

   * **自訂映像**︰指定您會藉由提供下列資訊來選擇自訂映像︰

      * **發行者**︰指定要讓虛擬機器使用的映像是由哪個發行者所建立 (例如「Microsoft」)。

      * **供應項目**︰從選取的發行者中指定要使用的虛擬機器供應項目 (例如「JDK」)。

      * **SKU**︰從選取的供應項目中指定要使用的 Stockkeeping 單元 (SKU) (例如「JDK_8」)。

      * **版本 #**︰指定所選 SKU 要使用的版本。

   ![選取虛擬機器映像視窗][CR03]

5. 按 [下一步] 。 

6. 在 [虛擬機器基本設定] 視窗中，輸入下列資訊︰

   * **虛擬機器名稱**：指定您新虛擬機器的名稱，其必須以字母開頭，且只能包含字母、數字及連字號。

   * **大小**︰指定要配置給虛擬機器的核心和記憶體數目。

   * **使用者名稱**︰指定要管理您的虛擬機器所建立的系統管理員帳戶。

   * **密碼**和**確認**︰指定您系統管理員帳戶的密碼。

   ![虛擬機器基本設定視窗][CR04]

7. 按 [下一步] 。 

8. 在 [相關聯的資源] 視窗中，輸入下列資訊：

   * **資源群組**︰指定虛擬機器的資源群組。 選取下列其中一個選項：
      * **新建**：指定您想要建立新的資源群組。
      * **使用現有項目**︰指定您要從 Azure 帳戶相關聯的資源群組清單中進行選擇。

       ![[相關聯的資源] 視窗][CR07]

   * **儲存體帳戶**︰指定要用來儲存虛擬機器的儲存體帳戶。 您可以選擇現有儲存體帳戶或建立新的帳戶。 如果您選擇 [新建]，畫面上會隨即顯示下列對話方塊︰

      ![[建立儲存體帳戶] 對話方塊][CR05]

   * **虛擬網路**和**子網路**︰指定虛擬機器所要連線的虛擬網路和子網路。 您可以使用現有的網路和子網路，也可以建立新的網路和子網路。 如果您選取 [新建]，畫面上會隨即顯示下列對話方塊︰

      ![[建立虛擬網路] 對話方塊][CR06]

   * **公用 IP 位址**︰指定虛擬機器的對外 IP 位址。 您可以選擇建立新的 IP 位址，但如果虛擬機器不會有公用 IP 位址，您也可以選取 [(無)]。 

   * **網路安全性群組**︰為虛擬機器指定選用的網路防火牆。 您可以選取現有防火牆，但如果虛擬機器不會使用網路防火牆，您也可以選取 [(無)]。 

   * **可用性設定組**︰指定虛擬機器可以加入的選用可用性設定組。 您可以選取現有的可用性設定組、建立新的可用性設定組，也可以在虛擬機器不會加入任何可用性設定組時選取 [(無)]。

9. 按一下 [完成] 。  
    [Azure Explorer] 工具視窗中便會出現新的虛擬機器。 

   ![[Azure Explorer] 檢視中的新虛擬機器][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a>在 IntelliJ 中重新啟動虛擬機器

若要在 IntelliJ 中使用 Azure Explorer 重新啟動虛擬機器，請執行下列作業︰

1. 在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下虛擬機器，然後選取 [重新啟動]。

   ![虛擬機器重新啟動命令][RE01]

2. 在確認視窗中，按一下 [是]。 

   ![重新啟動虛擬機器的確認視窗][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a>在 IntelliJ 中將虛擬機器關機

若要在 IntelliJ 中使用 Azure Explorer 將執行中的虛擬機器關機，請執行下列作業︰

1. 在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下虛擬機器，然後選取 [關機]。

   ![虛擬機器關機命令][SH01]

2. 在確認視窗中，按一下 [是]。 

   ![將虛擬機器關機的確認視窗][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a>在 IntelliJ 中刪除虛擬機器

若要在 IntelliJ 中使用 Azure Explorer 刪除虛擬機器，請執行下列作業︰

1. 在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下虛擬機器，然後選取 [刪除]。

   ![虛擬機器刪除命令][DE01]

2. 在確認視窗中，按一下 [是]。 

   ![刪除虛擬機器的確認視窗][DE02]

## <a name="next-steps"></a>後續步驟

如需 Azure 虛擬機器大小與定價的詳細資訊，請參閱下列資源︰

* Azure 虛擬機器大小
  * [Azure 中的 Windows 虛擬機器大小]
  * [Azure 中的 Linux 虛擬機器大小]
* Azure 虛擬機器定價
  * [Windows 虛擬機器定價]
  * [Linux 虛擬機器定價]

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[適用於 IntelliJ 的 Azure 工具組登入指示]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Azure 中的 Windows 虛擬機器大小]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure 中的 Linux 虛擬機器大小]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 虛擬機器定價]: /pricing/details/virtual-machines/windows/
[Linux 虛擬機器定價]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR08.png
