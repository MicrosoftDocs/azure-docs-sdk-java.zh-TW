---
title: "使用適用於 IntelliJ 的 Azure Explorer 來管理儲存體帳戶"
description: "了解如何使用適用於 IntelliJ 的 Azure 工具組來管理 Azure 儲存體帳戶。"
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
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 44d2ade8a0900b60222f06dfb9f93c1df17228c8
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="70a29-103">使用適用於 IntelliJ 的 Azure Explorer 來管理儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70a29-103">Manage storage accounts by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="70a29-104">Azure Explorer 是適用於 IntelliJ 的 Azure 工具組一部分，可為 Java 開發人員提供易於使用的解決方案，從 IntelliJ 整合式開發環境 (IDE) 內管理其 Azure 帳戶中的儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="70a29-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a><span data-ttu-id="70a29-105">在 IntelliJ 中建立儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70a29-105">Create a storage account in IntelliJ</span></span>

<span data-ttu-id="70a29-106">若要使用 Azure Explorer 來建立儲存體帳戶，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="70a29-106">To create a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="70a29-107">使用 [適用於 IntelliJ 的 Azure 工具組登入指示] 來登入您的 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="70a29-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for IntelliJ].</span></span> 

2. <span data-ttu-id="70a29-108">在 [Azure Explorer] 檢視中，展開 [Azure] 節點，以滑鼠右鍵按一下 [儲存體帳戶]，然後按一下 [建立儲存體帳戶]。</span><span class="sxs-lookup"><span data-stu-id="70a29-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![[建立儲存體帳戶] 命令][CS01]

3. <span data-ttu-id="70a29-110">在 [建立儲存體帳戶] 對話方塊中，指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="70a29-110">In the **Create Storage Account** dialog box, specify the following options:</span></span>

   ![[新建儲存體帳戶] 對話方塊][CS02]

   * <span data-ttu-id="70a29-112">**名稱**：指定新儲存體帳戶的名稱。</span><span class="sxs-lookup"><span data-stu-id="70a29-112">**Name**: Specifies the name for the new storage account.</span></span>

   * <span data-ttu-id="70a29-113">**帳戶種類**︰指定要建立的儲存體帳戶類型 (例如「Blob 儲存體」)。</span><span class="sxs-lookup"><span data-stu-id="70a29-113">**Account kind**: Specifies the type of storage account to create (for example, "Blob storage").</span></span> <span data-ttu-id="70a29-114">如需詳細資訊，請參閱[關於 Azure 儲存體帳戶]。</span><span class="sxs-lookup"><span data-stu-id="70a29-114">For more information, see [About Azure storage accounts].</span></span> 

   * <span data-ttu-id="70a29-115">**效能**︰從選取的發行者指定要使用的儲存體帳戶供應項目 (例如「進階」)。</span><span class="sxs-lookup"><span data-stu-id="70a29-115">**Performance**: Specifies which storage account offering to use from the selected publisher (for example, "Premium").</span></span> <span data-ttu-id="70a29-116">如需詳細資訊，請參閱 [Azure 儲存體延展性和效能目標]。</span><span class="sxs-lookup"><span data-stu-id="70a29-116">For more information, see [Azure storage scalability and performance targets].</span></span> 

   * <span data-ttu-id="70a29-117">**複寫**︰指定儲存體帳戶的複寫 (例如「區域備援」)。</span><span class="sxs-lookup"><span data-stu-id="70a29-117">**Replication**: Specifies the replication for the storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="70a29-118">如需詳細資訊，請參閱 [Azure 儲存體複寫]。</span><span class="sxs-lookup"><span data-stu-id="70a29-118">For more information, see [Azure storage replication].</span></span> 

   * <span data-ttu-id="70a29-119">**訂用帳戶**：指定您要用於新儲存體帳戶的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="70a29-119">**Subscription**: Specifies the Azure subscription that you want to use for the new storage account.</span></span>

   * <span data-ttu-id="70a29-120">**位置**︰指定要建立儲存體帳戶的位置 (例如「美國西部」)。</span><span class="sxs-lookup"><span data-stu-id="70a29-120">**Location**: Specifies the location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="70a29-121">**資源群組**︰指定虛擬機器的資源群組。</span><span class="sxs-lookup"><span data-stu-id="70a29-121">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="70a29-122">選取下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="70a29-122">Select one of the following options:</span></span>
      * <span data-ttu-id="70a29-123">**新建**：指定您想要建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="70a29-123">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="70a29-124">**使用現有項目**︰指定您會從 Azure 帳戶相關聯的資源群組清單中進行選擇。</span><span class="sxs-lookup"><span data-stu-id="70a29-124">**Use existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

4. <span data-ttu-id="70a29-125">當您指定好上述所有選項時，按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="70a29-125">When you have specified all of the preceding options, click **OK**.</span></span>

## <a name="create-a-storage-container-in-intellij"></a><span data-ttu-id="70a29-126">在 IntelliJ 中建立儲存體容器</span><span class="sxs-lookup"><span data-stu-id="70a29-126">Create a storage container in IntelliJ</span></span>

<span data-ttu-id="70a29-127">若要使用 Azure Explorer 來建立儲存體容器，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="70a29-127">To create a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="70a29-128">在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下您要在其中建立容器的儲存體帳戶，然後按一下 [建立 Blob 容器]。</span><span class="sxs-lookup"><span data-stu-id="70a29-128">In the Azure Explorer view, right-click the storage account where you want to create a container, and then click **Create blob container**.</span></span>

   ![[建立 Blob 容器] 命令][CC01]

2. <span data-ttu-id="70a29-130">在 [建立 Blob 容器] 對話方塊中指定容器名稱，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="70a29-130">In the **Create blob container** dialog box, specify the name for your container, and then click **OK**.</span></span> <span data-ttu-id="70a29-131">如需為儲存體容器命名的詳細資訊，請參閱[命名和參考容器、Blob 及中繼資料]。</span><span class="sxs-lookup"><span data-stu-id="70a29-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![[建立儲存體容器] 對話方塊][CC02]

## <a name="delete-a-storage-container-in-intellij"></a><span data-ttu-id="70a29-133">在 IntelliJ 中刪除儲存體容器</span><span class="sxs-lookup"><span data-stu-id="70a29-133">Delete a storage container in IntelliJ</span></span>

<span data-ttu-id="70a29-134">若要使用 Azure Explorer 來刪除儲存體容器，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="70a29-134">To delete a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="70a29-135">在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下儲存體容器，然後按一下 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="70a29-135">In the Azure Explorer view, right-click the storage container, and then click **Delete**.</span></span>

   ![[刪除儲存體容器] 命令][DC01]

2. <span data-ttu-id="70a29-137">在確認視窗中，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="70a29-137">In the confirmation window, click **Yes**.</span></span>

   ![刪除儲存體容器的確認視窗][DC02]

## <a name="delete-a-storage-account-in-intellij"></a><span data-ttu-id="70a29-139">在 IntelliJ 中刪除儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70a29-139">Delete a storage account in IntelliJ</span></span>

<span data-ttu-id="70a29-140">若要使用 Azure Explorer 來刪除儲存體帳戶，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="70a29-140">To delete a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="70a29-141">在 [Azure Explorer] 檢視中，以滑鼠右鍵按一下儲存體帳戶，然後選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="70a29-141">In the **Azure Explorer** view, right-click the storage account, and then select **Delete**.</span></span>

   ![[刪除] 儲存體帳戶功能表][DS01]

2. <span data-ttu-id="70a29-143">在確認視窗中，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="70a29-143">In the confirmation window, click **Yes**.</span></span>

   ![刪除儲存體帳戶的確認視窗][DS02]

## <a name="next-steps"></a><span data-ttu-id="70a29-145">後續步驟</span><span class="sxs-lookup"><span data-stu-id="70a29-145">Next steps</span></span>

<span data-ttu-id="70a29-146">如需 Azure 儲存體帳戶、大小與定價的詳細資訊，請參閱下列資源︰</span><span class="sxs-lookup"><span data-stu-id="70a29-146">For more information about Azure storage accounts, sizes, and pricing, see the following resources:</span></span>

* <span data-ttu-id="70a29-147">[Microsoft Azure 儲存體簡介]</span><span class="sxs-lookup"><span data-stu-id="70a29-147">[Introduction to Microsoft Azure Storage]</span></span>
* <span data-ttu-id="70a29-148">[關於 Azure 儲存體帳戶]</span><span class="sxs-lookup"><span data-stu-id="70a29-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="70a29-149">Azure 儲存體帳戶大小</span><span class="sxs-lookup"><span data-stu-id="70a29-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="70a29-150">[Azure 中的 Windows 儲存體帳戶大小]</span><span class="sxs-lookup"><span data-stu-id="70a29-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="70a29-151">[Azure 中的 Linux 儲存體帳戶大小]</span><span class="sxs-lookup"><span data-stu-id="70a29-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="70a29-152">Azure 儲存體帳戶定價</span><span class="sxs-lookup"><span data-stu-id="70a29-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="70a29-153">[Windows 儲存體帳戶定價]</span><span class="sxs-lookup"><span data-stu-id="70a29-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="70a29-154">[Linux 儲存體帳戶定價]</span><span class="sxs-lookup"><span data-stu-id="70a29-154">[Linux storage-account pricing]</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

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
