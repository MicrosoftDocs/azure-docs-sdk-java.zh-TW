---
title: "適用於 Eclipse 的 Azure 工具組登入指示"
description: "了解如何使用適用於 Eclipse 的 Azure 工具組登入 Microsoft Azure。"
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
ms.openlocfilehash: 498a22c455e54b8038169cf4e9f6ac7d7287c0bb
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>適用於 Eclipse 的 Azure 工具組的 Azure 登入指示

適用於 Eclipse 的 Azure 工具組提供兩種登入您 Azure 帳戶的方法︰

  * **自動化** - 當您使用這個方法時，您所建立的認證檔案會包含您的服務主體資料，此後您可以使用認證檔案自動登入 Azure 帳戶。
  * **互動式** - 當您使用這個方法時，每次登入 Azure 帳戶都要輸入 Azure 認證。

下列各節中的步驟將說明如何使用每個方法。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a>自動登入 Azure 帳戶，並建立要在未來使用的認證檔案

下列步驟將逐步引導您建立的認證檔案會包含您的服務主體資料。 一旦完成這些步驟後，Eclipse 會自動使用認證檔案，讓您每次開啟專案時都會自動登入 Azure。

1. 使用 Eclipse 開啟您的專案。

1. 依序按一下 [工具]、[Azure] 以及 [登入]。

   ![Azure 登入的 Eclipse 功能表][A01]

1. 當 [Azure 登入] 對話方塊出現時，請選取 [自動化]，然後按一下 [新增]。

   ![[登入] 對話方塊][A02]

1. 當 [Azure 登入] 對話方塊出現時，請輸入您的 Azure 認證，然後按一下 [登入]。

   ![[Azure 登入] 對話方塊][A03]

1. 當 [建立驗證檔案] 對話方塊出現時，請選取您想要使用的訂用帳戶，選擇您的目的地目錄，然後按一下 [啟動]。

   ![[Azure 登入] 對話方塊][A04]

1. [服務主體建立狀態]對話方塊隨即顯示，並在您的檔案成功建立之後，按一下 [確定]。

   ![[服務主體建立狀態] 對話方塊][A05]

1. 當 [Azure 登入]對話方塊出現時，按一下 [登入]。

   ![[Azure 登入] 對話方塊][A06]

1. 當 [選取訂用帳戶]對話方塊出現時，請選取您要使用的訂用帳戶，然後按一下 [確定]。

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>當您以自動化方式登入時，登出您的 Azure 帳戶

在上一節中設定步驟之後，每次您重新啟動 Eclipse 時，Azure 工具組都會自動將您登入 Azure 帳戶。 不過，若要登出您的 Azure 帳戶，並防止 Azure 工具組自動將您登入，請使用下列步驟。

1. 在 Eclipse 中，依序按一下 [工具]、[Azure] 以及 [登出]。

   ![Azure 登出的 Eclipse 功能表][L01]

1. 當 [Azure 登出] 對話方塊出現時，按一下 [是]。

   ![[登出] 對話方塊][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>使用已建立的認證檔案來自動登入 Azure 帳戶

如果您在使用 Eclipse 時登出 Azure，必須重新設定適用於 Eclipse 的 Azure 工具組來使用已建立的認證檔案之後，才可以自動登入 Azure 帳戶。 下列步驟將引導您將 Azure 工具組設定為使用現有的認證檔案。

1. 使用 Eclipse 開啟您的專案。

1. 依序按一下 [工具]、[Azure] 以及 [登入]。

   ![Azure 登入的 Eclipse 功能表][A01]

1. 當 [Azure 登入] 對話方塊出現時，請選取 [自動化]，然後按一下 [瀏覽]。

   ![[登入] 對話方塊][A02]

1. 當 [選取驗證檔案] 對話方塊出現時，請選取您稍早建立的認證檔案，然後按一下 [選取]。

   ![[登入] 對話方塊][A08]

1. 當 [Azure 登入]對話方塊出現時，按一下 [登入]。

   ![[Azure 登入] 對話方塊][A06]

1. 當 [選取訂用帳戶]對話方塊出現時，請選取您要使用的訂用帳戶，然後按一下 [確定]。

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="signing-into-your-azure-account-interactively"></a>以互動方式登入您的 Azure 帳戶

下列步驟將說明如何以手動方式輸入您的 Azure 認證來登入 Azure。

1. 使用 Eclipse 開啟您的專案。

1. 依序按一下 [工具]、[Azure] 以及 [登入]。

   ![Azure 登入的 Eclipse 功能表][I01]

1. 當 [Azure 登入] 對話方塊出現時，請選取 [互動式]，然後按一下 [登入]。

   ![[登入] 對話方塊][I02]

1. 當 [Azure 登入] 對話方塊出現時，請輸入您的 Azure 認證，然後按一下 [登入]。

   ![[Azure 登入] 對話方塊][I03]

1. 當 [選取訂用帳戶]對話方塊出現時，請選取您要使用的訂用帳戶，然後按一下 [確定]。

   ![[選取訂用帳戶] 對話方塊][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>當您以互動方式登入時，登出您的 Azure 帳戶

在上一節中設定步驟之後，每次您重新啟動 Eclipse 時，都會自動將您登出 Azure 帳戶。 不過，如果您想要登出您的 Azure 帳戶而不重新啟動 Eclipse，請使用下列步驟。

1. 在 Eclipse 中，依序按一下 [工具]、[Azure] 以及 [登出]。

   ![Azure 登出的 Eclipse 功能表][L01]

1. 當 [Azure 登出] 對話方塊出現時，按一下 [是]。

   ![[登出] 對話方塊][L02]

## <a name="next-steps"></a>後續步驟

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
