---
title: "使用 Azure Explorer for Eclipse 來管理虛擬機器"
description: "了解如何使用適用於 Eclipse 的 Azure 工具組來管理 Azure 虛擬機器。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: 41bb9b8009c0cbae01fec42c56d6a74d84cc166b
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2017
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a><span data-ttu-id="9ff5f-103">使用 Azure Explorer for Eclipse 來管理虛擬機器</span><span class="sxs-lookup"><span data-stu-id="9ff5f-103">Manage virtual machines by using the Azure Explorer for Eclipse</span></span>

<span data-ttu-id="9ff5f-104">Azure Explorer 是 Azure Toolkit for Eclipse 一部分，可為 Java 開發人員提供易於使用的解決方案，從 Eclipse 整合式開發環境 (IDE) 內管理其 Azure 帳戶中的虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-104">The Azure Explorer, which is part of the Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="9ff5f-105">在 Eclipse 中建立虛擬機器</span><span class="sxs-lookup"><span data-stu-id="9ff5f-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="9ff5f-106">若要使用 Azure Explorer 來建立虛擬機器，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="9ff5f-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="9ff5f-107">使用 [適用於 Eclipse 的 Azure 工具組的登入指示] 來登入您的 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse].</span></span>

1. <span data-ttu-id="9ff5f-108">在 [Azure Explorer] 檢視中，展開 [Azure] 節點，以滑鼠右鍵按一下 [虛擬機器]，然後按一下 [建立 VM]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   ![建立 VM 命令][CR01]  

   <span data-ttu-id="9ff5f-110">[建立新的虛擬機器] 精靈隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-110">The **Create new Virtual Machine** wizard opens.</span></span>

1. <span data-ttu-id="9ff5f-111">在 [選擇訂用帳戶] 視窗中選取您的訂用帳戶，然後按 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![選擇訂用帳戶視窗][CR02]

1. <span data-ttu-id="9ff5f-113">在 [選取虛擬機器映像] 視窗中，輸入下列資訊︰</span><span class="sxs-lookup"><span data-stu-id="9ff5f-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="9ff5f-114">**位置**︰指定要建立虛擬機器的位置 (例如「美國西部」)。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="9ff5f-115">**發行者**︰指定是哪個發行者建立您將用於建立虛擬機器的映像 (例如「Microsoft」)。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-115">**Publisher**: Specifies the publisher that created the image you'll use to create your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="9ff5f-116">**供應項目**︰從選取的發行者中指定要使用的虛擬機器供應項目 (例如「JDK」)。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-116">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="9ff5f-117">**SKU**︰從選取的供應項目中指定要使用的 Stockkeeping 單元 (SKU) (例如「JDK_8」)。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-117">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="9ff5f-118">**版本 #**︰指定所選 SKU 要使用的版本。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-118">**Version #**: Specifies which version of the selected SKU to use.</span></span>

   ![選取虛擬機器映像視窗][CR03]

1. <span data-ttu-id="9ff5f-120">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-120">Click **Next**.</span></span>

1. <span data-ttu-id="9ff5f-121">在 [虛擬機器基本設定] 視窗中，輸入下列資訊︰</span><span class="sxs-lookup"><span data-stu-id="9ff5f-121">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="9ff5f-122">**虛擬機器名稱**：指定您新虛擬機器的名稱，其必須以字母開頭，且只能包含字母、數字及連字號。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-122">**Virtual Machine Name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="9ff5f-123">**大小**︰指定要配置給虛擬機器的核心和記憶體數目。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-123">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="9ff5f-124">**使用者名稱**︰指定要管理您的虛擬機器所建立的系統管理員帳戶。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-124">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="9ff5f-125">**密碼**和**確認**︰指定您系統管理員帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-125">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

   ![虛擬機器基本設定視窗][CR04]

1. <span data-ttu-id="9ff5f-127">按一下 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-127">Click **Next**.</span></span>

1. <span data-ttu-id="9ff5f-128">在 [建立新的儲存體帳戶] 視窗中輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="9ff5f-128">In the **Create New Storage Account** window, enter the following information:</span></span>

   * <span data-ttu-id="9ff5f-129">**資源群組**︰指定虛擬機器的資源群組。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-129">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="9ff5f-130">選取下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="9ff5f-130">Select one of the following options:</span></span>
      * <span data-ttu-id="9ff5f-131">**新建**：指定您想要建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-131">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="9ff5f-132">**使用現有項目**︰指定您要選取已與您 Azure 帳戶相關聯的資源群組。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-132">**Use existing**: Specifies that you want to select a resource group that is already associated with your Azure account.</span></span>

      ![新建儲存體帳戶對話方塊][CR05]

   * <span data-ttu-id="9ff5f-134">**儲存體帳戶**︰指定要用來儲存虛擬機器的儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-134">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="9ff5f-135">您可以使用現有的儲存體帳戶或建立新帳戶。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="9ff5f-136">**虛擬網路**和**子網路**︰指定虛擬機器所要連線的虛擬網路和子網路。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-136">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="9ff5f-137">您可以使用現有的網路和子網路，也可以建立新的網路和子網路。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="9ff5f-138">如果您選取 [新建]，畫面上會顯示下列對話方塊︰</span><span class="sxs-lookup"><span data-stu-id="9ff5f-138">If you select **Create new**, the following dialog box is displayed:</span></span>

      ![新建虛擬網路對話方塊][CR06]

1. <span data-ttu-id="9ff5f-140">在 [相關聯的資源] 視窗中，輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="9ff5f-140">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="9ff5f-141">**公用 IP 位址**︰指定虛擬機器的對外 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="9ff5f-142">您可以選擇建立新的 IP 位址，但如果虛擬機器不會有公用 IP 位址，您也可以選取 [(無)]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-142">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="9ff5f-143">**網路安全性群組**︰為虛擬機器指定選用的網路防火牆。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="9ff5f-144">您可以選取現有防火牆，但如果虛擬機器不會使用網路防火牆，您也可以選取 [(無)]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="9ff5f-145">**可用性設定組**︰指定虛擬機器可以加入的選用可用性設定組。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="9ff5f-146">您可以選取現有的可用性設定組、建立新的可用性設定組，如果虛擬機器將不屬於任何可用性設定組，則可選取 [(無)]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong to an availability set, you can select **(None)**.</span></span>

   ![相關聯的資源視窗][CR07]

1. <span data-ttu-id="9ff5f-148">按一下 [完成] 。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-148">Click **Finish**.</span></span>  

   <span data-ttu-id="9ff5f-149">Azure Explorer 工具視窗中便會顯示新的虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-149">Your new virtual machine is displayed in the Azure Explorer tool window.</span></span>

   ![新虛擬機器][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="9ff5f-151">在 Eclipse 中重新啟動虛擬機器</span><span class="sxs-lookup"><span data-stu-id="9ff5f-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="9ff5f-152">若要在 Eclipse 中使用 Azure Explorer 重新啟動虛擬機器，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="9ff5f-152">To restart a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="9ff5f-153">在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下虛擬機器，然後選取 [重新啟動]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-153">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![虛擬機器重新啟動命令][RE01]

1. <span data-ttu-id="9ff5f-155">在確認視窗中，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-155">In the confirmation window, click **Yes**.</span></span>

   ![重新啟動確認視窗][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="9ff5f-157">在 Eclipse 中將虛擬機器關機</span><span class="sxs-lookup"><span data-stu-id="9ff5f-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="9ff5f-158">若要在 Eclipse 中使用 Azure Explorer 將執行中的虛擬機器關機，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="9ff5f-158">To shut down a running virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="9ff5f-159">在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下虛擬機器，然後選取 [關機]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-159">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![虛擬機器關機命令][SH01]

1. <span data-ttu-id="9ff5f-161">在確認視窗中，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-161">In the confirmation window, click **Yes**.</span></span>

   ![虛擬機器關機確認視窗][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="9ff5f-163">在 Eclipse 中刪除虛擬機器</span><span class="sxs-lookup"><span data-stu-id="9ff5f-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="9ff5f-164">若要在 Eclipse 中使用 Azure Explorer 刪除虛擬機器，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="9ff5f-164">To delete a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="9ff5f-165">在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下虛擬機器，然後選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-165">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![虛擬機器刪除命令][DE01]

1. <span data-ttu-id="9ff5f-167">在確認視窗中，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="9ff5f-167">In the confirmation window, click **Yes**.</span></span>

   ![虛擬機器刪除確認視窗][DE02]

## <a name="next-steps"></a><span data-ttu-id="9ff5f-169">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9ff5f-169">Next steps</span></span>

<span data-ttu-id="9ff5f-170">如需 Azure 虛擬機器大小與定價的詳細資訊，請參閱下列資源︰</span><span class="sxs-lookup"><span data-stu-id="9ff5f-170">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="9ff5f-171">Azure 虛擬機器大小</span><span class="sxs-lookup"><span data-stu-id="9ff5f-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="9ff5f-172">[Azure 中的 Windows 虛擬機器大小]</span><span class="sxs-lookup"><span data-stu-id="9ff5f-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="9ff5f-173">[Azure 中的 Linux 虛擬機器大小]</span><span class="sxs-lookup"><span data-stu-id="9ff5f-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="9ff5f-174">Azure 虛擬機器定價</span><span class="sxs-lookup"><span data-stu-id="9ff5f-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="9ff5f-175">[Windows 虛擬機器定價]</span><span class="sxs-lookup"><span data-stu-id="9ff5f-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="9ff5f-176">[Linux 虛擬機器定價]</span><span class="sxs-lookup"><span data-stu-id="9ff5f-176">[Linux virtual-machine pricing]</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure 中的 Windows 虛擬機器大小]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure 中的 Linux 虛擬機器大小]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 虛擬機器定價]: /pricing/details/virtual-machines/windows/
[Linux 虛擬機器定價]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
