---
title: 適用於 Eclipse 的 Azure 工具組登入指示
description: 了解如何使用適用於 Eclipse 的 Azure 工具組登入 Microsoft Azure。
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
ms.openlocfilehash: 498a22c455e54b8038169cf4e9f6ac7d7287c0bb
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="dae13-103">適用於 Eclipse 的 Azure 工具組的 Azure 登入指示</span><span class="sxs-lookup"><span data-stu-id="dae13-103">Azure Sign In Instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="dae13-104">適用於 Eclipse 的 Azure 工具組提供兩種登入您 Azure 帳戶的方法︰</span><span class="sxs-lookup"><span data-stu-id="dae13-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  * <span data-ttu-id="dae13-105">**自動化** - 當您使用這個方法時，您所建立的認證檔案會包含您的服務主體資料，此後您可以使用認證檔案自動登入 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="dae13-105">**Automated** - when you are using this method, you will create a credentials file which contains your service principal data, after which you can use the credentials file to automatically sign into your Azure account.</span></span>
  * <span data-ttu-id="dae13-106">**互動式** - 當您使用這個方法時，每次登入 Azure 帳戶都要輸入 Azure 認證。</span><span class="sxs-lookup"><span data-stu-id="dae13-106">**Interactive** - when you are using this method, you will enter your Azure credentials each time you sign into your Azure account.</span></span>

<span data-ttu-id="dae13-107">下列各節中的步驟將說明如何使用每個方法。</span><span class="sxs-lookup"><span data-stu-id="dae13-107">The steps in the following sections will describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a><span data-ttu-id="dae13-108">自動登入 Azure 帳戶，並建立要在未來使用的認證檔案</span><span class="sxs-lookup"><span data-stu-id="dae13-108">Signing into your Azure account automatically and creating a credentials file to use in the future</span></span>

<span data-ttu-id="dae13-109">下列步驟將逐步引導您建立的認證檔案會包含您的服務主體資料。</span><span class="sxs-lookup"><span data-stu-id="dae13-109">The following steps will walk you through creating a credentials file which contains your service principal data.</span></span> <span data-ttu-id="dae13-110">一旦完成這些步驟後，Eclipse 會自動使用認證檔案，讓您每次開啟專案時都會自動登入 Azure。</span><span class="sxs-lookup"><span data-stu-id="dae13-110">Once you have completed these steps, Eclipse will automatically use the credentials file to automatically sign you into Azure each time you open your project.</span></span>

1. <span data-ttu-id="dae13-111">使用 Eclipse 開啟您的專案。</span><span class="sxs-lookup"><span data-stu-id="dae13-111">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="dae13-112">依序按一下 [工具]、[Azure] 以及 [登入]。</span><span class="sxs-lookup"><span data-stu-id="dae13-112">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Azure 登入的 Eclipse 功能表][A01]

1. <span data-ttu-id="dae13-114">當 [Azure 登入] 對話方塊出現時，請選取 [自動化]，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="dae13-114">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **New**.</span></span>

   ![[登入] 對話方塊][A02]

1. <span data-ttu-id="dae13-116">當 [Azure 登入] 對話方塊出現時，請輸入您的 Azure 認證，然後按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="dae13-116">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![[Azure 登入] 對話方塊][A03]

1. <span data-ttu-id="dae13-118">當 [建立驗證檔案] 對話方塊出現時，請選取您想要使用的訂用帳戶，選擇您的目的地目錄，然後按一下 [啟動]。</span><span class="sxs-lookup"><span data-stu-id="dae13-118">When the **Create authentication files** dialog box appears, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![[Azure 登入] 對話方塊][A04]

1. <span data-ttu-id="dae13-120">[服務主體建立狀態]對話方塊隨即顯示，並在您的檔案成功建立之後，按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="dae13-120">The **Service Principal Creatation Status** dialog box will be displayed, and after your files have been created successfully, click **OK**.</span></span>

   ![[服務主體建立狀態] 對話方塊][A05]

1. <span data-ttu-id="dae13-122">當 [Azure 登入]對話方塊出現時，按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="dae13-122">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![[Azure 登入] 對話方塊][A06]

1. <span data-ttu-id="dae13-124">當 [選取訂用帳戶]對話方塊出現時，請選取您要使用的訂用帳戶，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="dae13-124">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a><span data-ttu-id="dae13-126">當您以自動化方式登入時，登出您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="dae13-126">Signing out of your Azure account when you signed in automatically</span></span>

<span data-ttu-id="dae13-127">在上一節中設定步驟之後，每次您重新啟動 Eclipse 時，Azure 工具組都會自動將您登入 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="dae13-127">After you have configured the steps in the previous section, the Azure Toolkit will automatically sign you into your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="dae13-128">不過，若要登出您的 Azure 帳戶，並防止 Azure 工具組自動將您登入，請使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="dae13-128">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, use the following steps.</span></span>

1. <span data-ttu-id="dae13-129">在 Eclipse 中，依序按一下 [工具]、[Azure] 以及 [登出]。</span><span class="sxs-lookup"><span data-stu-id="dae13-129">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Azure 登出的 Eclipse 功能表][L01]

1. <span data-ttu-id="dae13-131">當 [Azure 登出] 對話方塊出現時，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="dae13-131">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![[登出] 對話方塊][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a><span data-ttu-id="dae13-133">使用已建立的認證檔案來自動登入 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="dae13-133">Signing into your Azure account automatically using a credentials file which you have already created</span></span>

<span data-ttu-id="dae13-134">如果您在使用 Eclipse 時登出 Azure，必須重新設定適用於 Eclipse 的 Azure 工具組來使用已建立的認證檔案之後，才可以自動登入 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="dae13-134">If you sign out of Azure when you are using Eclipse, you will need to reconfigure the Azure Toolkit for Eclipse to use a credentials file which have created before you can automatically sign into your Azure acccount.</span></span> <span data-ttu-id="dae13-135">下列步驟將引導您將 Azure 工具組設定為使用現有的認證檔案。</span><span class="sxs-lookup"><span data-stu-id="dae13-135">The following steps will walk you through configuring the Azure Toolkit to use an existing credentials file.</span></span>

1. <span data-ttu-id="dae13-136">使用 Eclipse 開啟您的專案。</span><span class="sxs-lookup"><span data-stu-id="dae13-136">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="dae13-137">依序按一下 [工具]、[Azure] 以及 [登入]。</span><span class="sxs-lookup"><span data-stu-id="dae13-137">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Azure 登入的 Eclipse 功能表][A01]

1. <span data-ttu-id="dae13-139">當 [Azure 登入] 對話方塊出現時，請選取 [自動化]，然後按一下 [瀏覽]。</span><span class="sxs-lookup"><span data-stu-id="dae13-139">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **Browse**.</span></span>

   ![[登入] 對話方塊][A02]

1. <span data-ttu-id="dae13-141">當 [選取驗證檔案] 對話方塊出現時，請選取您稍早建立的認證檔案，然後按一下 [選取]。</span><span class="sxs-lookup"><span data-stu-id="dae13-141">When the **Select Authenticated File** dialog box appears, select a credentials file which you created earlier, and then click **Select**.</span></span>

   ![[登入] 對話方塊][A08]

1. <span data-ttu-id="dae13-143">當 [Azure 登入]對話方塊出現時，按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="dae13-143">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![[Azure 登入] 對話方塊][A06]

1. <span data-ttu-id="dae13-145">當 [選取訂用帳戶]對話方塊出現時，請選取您要使用的訂用帳戶，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="dae13-145">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="signing-into-your-azure-account-interactively"></a><span data-ttu-id="dae13-147">以互動方式登入您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="dae13-147">Signing into your Azure account interactively</span></span>

<span data-ttu-id="dae13-148">下列步驟將說明如何以手動方式輸入您的 Azure 認證來登入 Azure。</span><span class="sxs-lookup"><span data-stu-id="dae13-148">The following steps will illustrate how to sign into Azure by manually entering your Azure credentials.</span></span>

1. <span data-ttu-id="dae13-149">使用 Eclipse 開啟您的專案。</span><span class="sxs-lookup"><span data-stu-id="dae13-149">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="dae13-150">依序按一下 [工具]、[Azure] 以及 [登入]。</span><span class="sxs-lookup"><span data-stu-id="dae13-150">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Azure 登入的 Eclipse 功能表][I01]

1. <span data-ttu-id="dae13-152">當 [Azure 登入] 對話方塊出現時，請選取 [互動式]，然後按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="dae13-152">When the **Azure Sign In** dialog box appears, select **Interactive**, and then click **Sign In**.</span></span>

   ![[登入] 對話方塊][I02]

1. <span data-ttu-id="dae13-154">當 [Azure 登入] 對話方塊出現時，請輸入您的 Azure 認證，然後按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="dae13-154">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![[Azure 登入] 對話方塊][I03]

1. <span data-ttu-id="dae13-156">當 [選取訂用帳戶]對話方塊出現時，請選取您要使用的訂用帳戶，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="dae13-156">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a><span data-ttu-id="dae13-158">當您以互動方式登入時，登出您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="dae13-158">Signing out of your Azure account when you signed in interactively</span></span>

<span data-ttu-id="dae13-159">在上一節中設定步驟之後，每次您重新啟動 Eclipse 時，都會自動將您登出 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="dae13-159">After you have configured the steps in the previous section, you will automatically signed out of your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="dae13-160">不過，如果您想要登出您的 Azure 帳戶而不重新啟動 Eclipse，請使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="dae13-160">However, if you want to sign out of your Azure account without restarting Eclipse, use the following steps.</span></span>

1. <span data-ttu-id="dae13-161">在 Eclipse 中，依序按一下 [工具]、[Azure] 以及 [登出]。</span><span class="sxs-lookup"><span data-stu-id="dae13-161">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Azure 登出的 Eclipse 功能表][L01]

1. <span data-ttu-id="dae13-163">當 [Azure 登出] 對話方塊出現時，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="dae13-163">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![[登出] 對話方塊][L02]

## <a name="next-steps"></a><span data-ttu-id="dae13-165">後續步驟</span><span class="sxs-lookup"><span data-stu-id="dae13-165">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
