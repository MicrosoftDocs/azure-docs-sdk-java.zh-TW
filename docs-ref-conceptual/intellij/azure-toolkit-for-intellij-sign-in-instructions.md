---
title: "Azure Toolkit for IntelliJ 的登入指示"
description: "了解如何使用適用於 IntelliJ 的 Azure 工具組登入 Microsoft Azure。"
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
ms.openlocfilehash: 4e24dac285fa38bad4293f1cce2830242c2fe151
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a><span data-ttu-id="998e5-103">Azure Toolkit for IntelliJ 的登入指示</span><span class="sxs-lookup"><span data-stu-id="998e5-103">Sign-in instructions for the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="998e5-104">適用於 IntelliJ 的 Azure 工具組提供兩種登入您 Azure 帳戶的方法︰</span><span class="sxs-lookup"><span data-stu-id="998e5-104">The Azure Toolkit for IntelliJ provides two methods for signing in to your Azure account:</span></span>

  * <span data-ttu-id="998e5-105">**自動化**︰建立認證檔案，用以自動登入 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="998e5-105">**Automated**: You create a credentials file that you can use to automatically sign in to your Azure account.</span></span>
  * <span data-ttu-id="998e5-106">**互動式**︰在每次登入 Azure 帳戶時要輸入 Azure 認證。</span><span class="sxs-lookup"><span data-stu-id="998e5-106">**Interactive**: You enter your Azure credentials each time you sign in to your Azure account.</span></span>

<span data-ttu-id="998e5-107">下列各節會說明如何使用每個方法。</span><span class="sxs-lookup"><span data-stu-id="998e5-107">The following sections describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-automatically"></a><span data-ttu-id="998e5-108">自動登入您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="998e5-108">Sign in to your Azure account automatically</span></span>

<span data-ttu-id="998e5-109">本節會逐步引導您建立認證檔案，並在檔案中包含您的服務主體資料。</span><span class="sxs-lookup"><span data-stu-id="998e5-109">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="998e5-110">在完成此程序後，Eclipse 會使用認證檔案，讓您在每次開啟專案時自動登入 Azure。</span><span class="sxs-lookup"><span data-stu-id="998e5-110">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure each time you open your project.</span></span>

1. <span data-ttu-id="998e5-111">使用 IntelliJ IDEA 開啟您的專案。</span><span class="sxs-lookup"><span data-stu-id="998e5-111">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="998e5-112">在 [工具] 功能表上，指向 [Azure]，然後按一下 [Azure 登入]。</span><span class="sxs-lookup"><span data-stu-id="998e5-112">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![IntelliJ 的 [Azure 登入] 命令][A01]

1. <span data-ttu-id="998e5-114">在 [Azure 登入] 視窗中選取 [自動化]，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="998e5-114">In the **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![選取了 [自動化] 的 [Azure 登入] 視窗][A02]

1. <span data-ttu-id="998e5-116">在 [Azure 登入對話方塊] 視窗中，輸入您的 Azure 認證，然後按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="998e5-116">In the **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![[Azure 登入] 對話方塊視窗][A03]

1. <span data-ttu-id="998e5-118">在 [建立驗證檔案] 視窗中，選取您想要使用的訂用帳戶，選擇您的目的地目錄，然後按一下 [啟動]。</span><span class="sxs-lookup"><span data-stu-id="998e5-118">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![[建立驗證檔案] 視窗][A04]

1. <span data-ttu-id="998e5-120">在 [服務主體建立狀態] 對話方塊中，於系統成功建立檔案之後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="998e5-120">In the **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![[服務主體建立狀態] 對話方塊][A05]

1. <span data-ttu-id="998e5-122">在 [Azure 登入] 視窗中，按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="998e5-122">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![[Azure 登入] 對話方塊][A06]

1. <span data-ttu-id="998e5-124">在 [選取訂用帳戶] 對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="998e5-124">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="998e5-126">在自動登入後登出您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="998e5-126">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="998e5-127">在使用前面的步驟設定好帳戶後，每次您重新啟動 IntelliJ IDEA 時，Azure 工具組都會將您自動登入 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="998e5-127">After you have configured your account by using the preceding steps, the Azure Toolkit automatically signs you in to your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="998e5-128">不過，若要登出您的 Azure 帳戶，並防止 Azure 工具組自動將您登入，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="998e5-128">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, do the following:</span></span>

1. <span data-ttu-id="998e5-129">在 IntelliJ IDEA 的 [工具] 功能表上，指向 [Azure]，然後按一下 [Azure 登出]。</span><span class="sxs-lookup"><span data-stu-id="998e5-129">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![IntelliJ 的 [Azure 登出] 命令][L01]

1. <span data-ttu-id="998e5-131">在 [Azure 登出] 確認視窗中，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="998e5-131">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![[Azure 登出] 確認視窗][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="998e5-133">使用現有的認證檔案自動登入 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="998e5-133">Sign in to your Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="998e5-134">如果您在使用 IntelliJ IDEA 時登出了 Azure 帳戶，您必須使用現有的認證檔案來自動重新登入帳戶。</span><span class="sxs-lookup"><span data-stu-id="998e5-134">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file to automatically sign back in to the account.</span></span> <span data-ttu-id="998e5-135">若要將適用於 Eclipse 的 Azure 工具組設定為使用現有認證檔案，請執行下列動作︰</span><span class="sxs-lookup"><span data-stu-id="998e5-135">To configure the Azure Toolkit for Eclipse to use an existing credentials file, do the following:</span></span>

1. <span data-ttu-id="998e5-136">使用 IntelliJ IDEA 開啟您的專案。</span><span class="sxs-lookup"><span data-stu-id="998e5-136">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="998e5-137">在 [工具] 功能表上，指向 [Azure]，然後按一下 [Azure 登入]。</span><span class="sxs-lookup"><span data-stu-id="998e5-137">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![IntelliJ 的 [Azure 登入] 命令][A01]

1. <span data-ttu-id="998e5-139">在 [Azure 登入] 視窗中選取 [自動化]，然後按一下 [瀏覽]。</span><span class="sxs-lookup"><span data-stu-id="998e5-139">In the **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![選取了 [自動化] 的 [Azure 登入] 視窗][A02]

1. <span data-ttu-id="998e5-141">在 [選取驗證檔案] 對話方塊中，選取先前建立的認證檔案，然後按一下 [選取]。</span><span class="sxs-lookup"><span data-stu-id="998e5-141">In the **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![[選取驗證檔案] 對話方塊][A08]

1. <span data-ttu-id="998e5-143">在 [Azure 登入] 視窗中，按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="998e5-143">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![選取了 [自動化] 的 [Azure 登入] 視窗][A06]

1. <span data-ttu-id="998e5-145">在 [選取訂用帳戶] 對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="998e5-145">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="sign-in-to-your-azure-account-interactively"></a><span data-ttu-id="998e5-147">以互動方式登入您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="998e5-147">Sign in to your Azure account interactively</span></span>

<span data-ttu-id="998e5-148">若要以手動方式輸入您的 Azure 認證來登入 Azure，請執行下列作業︰</span><span class="sxs-lookup"><span data-stu-id="998e5-148">To sign in to Azure by manually entering your Azure credentials, do the following:</span></span>

1. <span data-ttu-id="998e5-149">使用 IntelliJ IDEA 開啟您的專案。</span><span class="sxs-lookup"><span data-stu-id="998e5-149">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="998e5-150">按一下 [工具]，指向 [Azure]，然後按一下 [Azure 登入]。</span><span class="sxs-lookup"><span data-stu-id="998e5-150">Click **Tools**, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![IntelliJ 的 [Azure 登入] 命令][I01]

1. <span data-ttu-id="998e5-152">在 [Azure 登入] 視窗中，選取 [互動式]，然後按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="998e5-152">In the **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![選取了 [互動式] 的 [Azure 登入] 視窗][I02]

1. <span data-ttu-id="998e5-154">在 [Azure 登入] 對話方塊出現時，輸入您的 Azure 認證，然後按一下 [登入]。</span><span class="sxs-lookup"><span data-stu-id="998e5-154">In the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![[Azure 登入] 對話方塊視窗][I03]

1. <span data-ttu-id="998e5-156">在 [選取訂用帳戶] 對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="998e5-156">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="998e5-158">在以互動方式登入後，登出您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="998e5-158">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="998e5-159">在使用前面的步驟設定好帳戶後，每次您重新啟動 IntelliJ IDEA 時，系統都會自動將您登出 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="998e5-159">After you have configured your account by using the preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="998e5-160">不過，如果您想要登出 Azure 帳戶而不重新啟動 IntelliJ IDEA，請使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="998e5-160">However, if you want to sign out of your Azure account *without* restarting IntelliJ IDEA, do the following.</span></span>

1. <span data-ttu-id="998e5-161">在 IntelliJ IDEA 的 [工具] 功能表上，指向 [Azure]，然後按一下 [Azure 登出]。</span><span class="sxs-lookup"><span data-stu-id="998e5-161">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![IntelliJ 的 [Azure 登出] 命令][L01]

1. <span data-ttu-id="998e5-163">在 [Azure 登出] 確認視窗中，按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="998e5-163">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![[Azure 登出] 確認視窗][L02]

## <a name="next-steps"></a><span data-ttu-id="998e5-165">後續步驟</span><span class="sxs-lookup"><span data-stu-id="998e5-165">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
