---
title: Azure Toolkit for Eclipse 的登入指示
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
ms.openlocfilehash: b4b13de38913ae6e7ae2bb09210ac742efc9d0ad
ms.sourcegitcommit: d18d9dce22b7f7af178f756bd341433d24e3c3b5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2019
ms.locfileid: "66575315"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="20794-103">Azure Toolkit for Eclipse 的登入指示</span><span class="sxs-lookup"><span data-stu-id="20794-103">Sign-in instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="20794-104">適用於 Eclipse 的 Azure 工具組提供兩種登入您 Azure 帳戶的方法︰</span><span class="sxs-lookup"><span data-stu-id="20794-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  - [<span data-ttu-id="20794-105">使用裝置登入來登入您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="20794-105">Sign in to your Azure account by Device Login</span></span>](#sign-in-to-your-azure-account-by-device-login)
  - [<span data-ttu-id="20794-106">使用服務主體來登入您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="20794-106">Sign in to your Azure account by Service Principal</span></span>](#sign-in-to-your-azure-account-by-service-principal)

<span data-ttu-id="20794-107">此外也提供[**登出**](#sign-out-of-your-azure-account)方法。</span><span class="sxs-lookup"><span data-stu-id="20794-107">[**Sign out**](#sign-out-of-your-azure-account) methods are also provided.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a><span data-ttu-id="20794-108">使用裝置登入來登入您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="20794-108">Sign in to your Azure account by Device Login</span></span>

<span data-ttu-id="20794-109">若要使用裝置登入來登入 Azure，請執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="20794-109">To sign in Azure by device login, do the following:</span></span>

1. <span data-ttu-id="20794-110">使用 Eclipse 開啟您的專案。</span><span class="sxs-lookup"><span data-stu-id="20794-110">Open your project with Eclipse.</span></span>

2. <span data-ttu-id="20794-111">依序按一下 [工具]  、[Azure]  以及 [登入]  。</span><span class="sxs-lookup"><span data-stu-id="20794-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="20794-112">![Azure 登入的 Eclipse 功能表][I01]</span><span class="sxs-lookup"><span data-stu-id="20794-112">![Eclipse Menu for Azure Sign In][I01]</span></span>

3. <span data-ttu-id="20794-113">在 [Azure 登入]  視窗中，選取 [裝置登入]  ，然後按一下 [登入]  。</span><span class="sxs-lookup"><span data-stu-id="20794-113">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in**.</span></span>

   ![選取了 [裝置登入] 的 [Azure 登入] 視窗][I02]

4. <span data-ttu-id="20794-115">按一下 [Azure 裝置登入]  對話方塊中的 [複製並開啟]  。</span><span class="sxs-lookup"><span data-stu-id="20794-115">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![[Azure 登入] 對話方塊視窗][I03]

> [!NOTE]
>
> <span data-ttu-id="20794-117">如果瀏覽器未開啟，請將 Eclipse 設定為使用外部瀏覽器，如 Internet Explorer、Firefox 或 Chrome：</span><span class="sxs-lookup"><span data-stu-id="20794-117">If the browser doesn't open, configure Eclipse to use an external browser like Internet Explorer, Firefox, or Chrome:</span></span>
>
> 1. <span data-ttu-id="20794-118">在 Eclipse 中開啟 [喜好設定] -> [一般] -> [網頁瀏覽器] -> [使用外部網頁瀏覽器]</span><span class="sxs-lookup"><span data-stu-id="20794-118">Open Preferences -> General -> Web Browser -> Use external web browser in Eclipse</span></span>
>
> 2. <span data-ttu-id="20794-119">選取您想要使用的瀏覽器</span><span class="sxs-lookup"><span data-stu-id="20794-119">Select the browser you prefer to use</span></span>
>

5. <span data-ttu-id="20794-120">在瀏覽器中，貼上您的裝置程式碼 (您在上一個步驟中按一下 [複製並開啟]  時所複製)，然後按 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="20794-120">In the browser, paste your device code (which has been copied when you clicked **Copy&Open** in last step) and then click **Next**.</span></span>

   ![裝置登入瀏覽器][I04]

6. <span data-ttu-id="20794-122">最後，在 [選取訂用帳戶]  對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="20794-122">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a><span data-ttu-id="20794-124">使用服務主體來登入您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="20794-124">Sign in to your Azure account by Service Principal</span></span>

<span data-ttu-id="20794-125">本節會逐步引導您建立認證檔案，並在檔案中包含您的服務主體資料。</span><span class="sxs-lookup"><span data-stu-id="20794-125">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="20794-126">在完成此程序後，Eclipse 會使用認證檔案，讓您在開啟專案時自動登入 Azure。</span><span class="sxs-lookup"><span data-stu-id="20794-126">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure when open your project.</span></span>

1. <span data-ttu-id="20794-127">使用 Eclipse 開啟您的專案。</span><span class="sxs-lookup"><span data-stu-id="20794-127">Open your project with Eclipse.</span></span>

2. <span data-ttu-id="20794-128">依序按一下 [工具]  、[Azure]  以及 [登入]  。</span><span class="sxs-lookup"><span data-stu-id="20794-128">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="20794-129">![Eclipse Azure 登入命令][A01]</span><span class="sxs-lookup"><span data-stu-id="20794-129">![The Eclipse Azure Sign In command][A01]</span></span>

3. <span data-ttu-id="20794-130">在 [Azure 登入]  視窗中，選取 [服務主體]  。</span><span class="sxs-lookup"><span data-stu-id="20794-130">In the **Azure Sign In** window, select **Service Principal**.</span></span> <span data-ttu-id="20794-131">如果您還沒有服務主體驗證檔案，請按一下 [新增]  建立一個。</span><span class="sxs-lookup"><span data-stu-id="20794-131">If you do not have the service principal authentication file yet, click **New** to create one.</span></span> <span data-ttu-id="20794-132">否則，您可以按一下 [瀏覽]  加以開啟，並跳到步驟 8。</span><span class="sxs-lookup"><span data-stu-id="20794-132">Otherwise you can click **Browse** to open it and jump to step 8.</span></span>

   ![選取了 [服務主體] 的 [Azure 登入] 視窗][A02]

4. <span data-ttu-id="20794-134">按一下 [Azure 裝置登入]  對話方塊中的 [複製並開啟]  。</span><span class="sxs-lookup"><span data-stu-id="20794-134">Click **Copy&Open** in **Azure Device Login** dialog.</span></span>

   ![[Azure 登入] 對話方塊視窗][A08]

> [!NOTE]
>
> <span data-ttu-id="20794-136">如果瀏覽器未開啟，請將 Eclipse 設定為使用外部瀏覽器，如 IE 或 Chrome：</span><span class="sxs-lookup"><span data-stu-id="20794-136">If the browser doesn't open, configure eclipse to use an external browser like IE or Chrome:</span></span>
>
> 1. <span data-ttu-id="20794-137">在 Eclipse 中開啟 [喜好設定] -> [一般] -> [網頁瀏覽器] -> [使用外部網頁瀏覽器]</span><span class="sxs-lookup"><span data-stu-id="20794-137">Open Preferences -> General -> Web Browser -> Use external web browser in Eclipse</span></span>
>
> 2. <span data-ttu-id="20794-138">選取您想要使用的瀏覽器</span><span class="sxs-lookup"><span data-stu-id="20794-138">Select the browser you prefer to use</span></span>
>

5. <span data-ttu-id="20794-139">在瀏覽器中，貼上您的裝置程式碼 (您在上一個步驟中按一下 [複製並開啟]  時所複製)，然後按 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="20794-139">In the browser, paste your device code (which has been copied when you click **Copy&Open** in last step) and then click **Next**.</span></span>

   ![裝置登入瀏覽器][A03]

6. <span data-ttu-id="20794-141">在 [建立驗證檔案]  視窗中，選取您想要使用的訂用帳戶，選擇您的目的地目錄，然後按一下 [啟動]  。</span><span class="sxs-lookup"><span data-stu-id="20794-141">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![[建立驗證檔案] 視窗][A04]

7. <span data-ttu-id="20794-143">在 [服務主體建立狀態]  對話方塊中，於系統成功建立檔案之後按一下 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="20794-143">In the **Service Principal Creation Status** dialog box, click **OK** after your files have been created successfully.</span></span>

   ![[服務主體建立狀態] 對話方塊][A05]

8. <span data-ttu-id="20794-145">已建立檔案的位址會自動填入 [Azure 登入]  視窗中，此時請按一下 [登入]  。</span><span class="sxs-lookup"><span data-stu-id="20794-145">Address of the created file will be automatically filled in the **Azure Sign In** window, now click **Sign in**.</span></span>

   ![[Azure 登入] 對話方塊][A06]

9. <span data-ttu-id="20794-147">最後，在 [選取訂用帳戶]  對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="20794-147">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="sign-out-of-your-azure-account"></a><span data-ttu-id="20794-149">登出您的 Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="20794-149">Sign out of your Azure account</span></span>

<span data-ttu-id="20794-150">在前述步驟中設定好帳戶後，每當您啟動 Eclipse 時，系統就會將您自動登入。</span><span class="sxs-lookup"><span data-stu-id="20794-150">After you have configured your account by preceding steps, you will be automatically signed in each time you start Eclipse.</span></span> <span data-ttu-id="20794-151">不過，如果您想要登出 Azure 帳戶，請使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="20794-151">However, if you want to sign out of your Azure account, use the following steps.</span></span>

1. <span data-ttu-id="20794-152">在 Eclipse 中，依序按一下 [工具]  、[Azure]  以及 [登出]  。</span><span class="sxs-lookup"><span data-stu-id="20794-152">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Azure 登出的 Eclipse 功能表][L01]

2. <span data-ttu-id="20794-154">當 [Azure 登出]  對話方塊出現時，按一下 [是]  。</span><span class="sxs-lookup"><span data-stu-id="20794-154">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![[登出] 對話方塊][L02]

## <a name="next-steps"></a><span data-ttu-id="20794-156">後續步驟</span><span class="sxs-lookup"><span data-stu-id="20794-156">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

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
