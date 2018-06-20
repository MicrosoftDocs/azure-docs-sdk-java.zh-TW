---
title: Azure Toolkit for IntelliJ 的登入指示
description: 了解如何使用適用於 IntelliJ 的 Azure 工具組登入 Microsoft Azure。
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
ms.openlocfilehash: 4e24dac285fa38bad4293f1cce2830242c2fe151
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954389"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ 的登入指示

適用於 IntelliJ 的 Azure 工具組提供兩種登入您 Azure 帳戶的方法︰

  * **自動化**︰建立認證檔案，用以自動登入 Azure 帳戶。
  * **互動式**︰在每次登入 Azure 帳戶時要輸入 Azure 認證。

下列各節會說明如何使用每個方法。

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-automatically"></a>自動登入您的 Azure 帳戶

本節會逐步引導您建立認證檔案，並在檔案中包含您的服務主體資料。 在完成此程序後，Eclipse 會使用認證檔案，讓您在每次開啟專案時自動登入 Azure。

1. 使用 IntelliJ IDEA 開啟您的專案。

1. 在 [工具] 功能表上，指向 [Azure]，然後按一下 [Azure 登入]。

   ![IntelliJ 的 [Azure 登入] 命令][A01]

1. 在 [Azure 登入] 視窗中選取 [自動化]，然後按一下 [新增]。

   ![選取了 [自動化] 的 [Azure 登入] 視窗][A02]

1. 在 [Azure 登入對話方塊] 視窗中，輸入您的 Azure 認證，然後按一下 [登入]。

   ![[Azure 登入] 對話方塊視窗][A03]

1. 在 [建立驗證檔案] 視窗中，選取您想要使用的訂用帳戶，選擇您的目的地目錄，然後按一下 [啟動]。

   ![[建立驗證檔案] 視窗][A04]

1. 在 [服務主體建立狀態] 對話方塊中，於系統成功建立檔案之後按一下 [確定]。

   ![[服務主體建立狀態] 對話方塊][A05]

1. 在 [Azure 登入] 視窗中，按一下 [登入]。

   ![[Azure 登入] 對話方塊][A06]

1. 在 [選取訂用帳戶] 對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]。

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a>在自動登入後登出您的 Azure 帳戶

在使用前面的步驟設定好帳戶後，每次您重新啟動 IntelliJ IDEA 時，Azure 工具組都會將您自動登入 Azure 帳戶。 不過，若要登出您的 Azure 帳戶，並防止 Azure 工具組自動將您登入，請執行下列作業︰

1. 在 IntelliJ IDEA 的 [工具] 功能表上，指向 [Azure]，然後按一下 [Azure 登出]。

   ![IntelliJ 的 [Azure 登出] 命令][L01]

1. 在 [Azure 登出] 確認視窗中，按一下 [是]。

   ![[Azure 登出] 確認視窗][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a>使用現有的認證檔案自動登入 Azure 帳戶

如果您在使用 IntelliJ IDEA 時登出了 Azure 帳戶，您必須使用現有的認證檔案來自動重新登入帳戶。 若要將適用於 Eclipse 的 Azure 工具組設定為使用現有認證檔案，請執行下列動作︰

1. 使用 IntelliJ IDEA 開啟您的專案。

1. 在 [工具] 功能表上，指向 [Azure]，然後按一下 [Azure 登入]。

   ![IntelliJ 的 [Azure 登入] 命令][A01]

1. 在 [Azure 登入] 視窗中選取 [自動化]，然後按一下 [瀏覽]。

   ![選取了 [自動化] 的 [Azure 登入] 視窗][A02]

1. 在 [選取驗證檔案] 對話方塊中，選取先前建立的認證檔案，然後按一下 [選取]。

   ![[選取驗證檔案] 對話方塊][A08]

1. 在 [Azure 登入] 視窗中，按一下 [登入]。

   ![選取了 [自動化] 的 [Azure 登入] 視窗][A06]

1. 在 [選取訂用帳戶] 對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]。

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="sign-in-to-your-azure-account-interactively"></a>以互動方式登入您的 Azure 帳戶

若要以手動方式輸入您的 Azure 認證來登入 Azure，請執行下列作業︰

1. 使用 IntelliJ IDEA 開啟您的專案。

1. 按一下 [工具]，指向 [Azure]，然後按一下 [Azure 登入]。

   ![IntelliJ 的 [Azure 登入] 命令][I01]

1. 在 [Azure 登入] 視窗中，選取 [互動式]，然後按一下 [登入]。

   ![選取了 [互動式] 的 [Azure 登入] 視窗][I02]

1. 在 [Azure 登入] 對話方塊出現時，輸入您的 Azure 認證，然後按一下 [登入]。

   ![[Azure 登入] 對話方塊視窗][I03]

1. 在 [選取訂用帳戶] 對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]。

   ![[選取訂用帳戶] 對話方塊][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a>在以互動方式登入後，登出您的 Azure 帳戶

在使用前面的步驟設定好帳戶後，每次您重新啟動 IntelliJ IDEA 時，系統都會自動將您登出 Azure 帳戶。 不過，如果您想要登出 Azure 帳戶而不重新啟動 IntelliJ IDEA，請使用下列步驟。

1. 在 IntelliJ IDEA 的 [工具] 功能表上，指向 [Azure]，然後按一下 [Azure 登出]。

   ![IntelliJ 的 [Azure 登出] 命令][L01]

1. 在 [Azure 登出] 確認視窗中，按一下 [是]。

   ![[Azure 登出] 確認視窗][L02]

## <a name="next-steps"></a>後續步驟

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
