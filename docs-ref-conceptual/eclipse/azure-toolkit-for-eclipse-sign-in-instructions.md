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
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse 的登入指示

適用於 Eclipse 的 Azure 工具組提供兩種登入您 Azure 帳戶的方法︰

  - [使用裝置登入來登入您的 Azure 帳戶](#sign-in-to-your-azure-account-by-device-login)
  - [使用服務主體來登入您的 Azure 帳戶](#sign-in-to-your-azure-account-by-service-principal)

此外也提供[**登出**](#sign-out-of-your-azure-account)方法。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>使用裝置登入來登入您的 Azure 帳戶

若要使用裝置登入來登入 Azure，請執行下列作業：

1. 使用 Eclipse 開啟您的專案。

2. 依序按一下 [工具]  、[Azure]  以及 [登入]  。
   ![Azure 登入的 Eclipse 功能表][I01]

3. 在 [Azure 登入]  視窗中，選取 [裝置登入]  ，然後按一下 [登入]  。

   ![選取了 [裝置登入] 的 [Azure 登入] 視窗][I02]

4. 按一下 [Azure 裝置登入]  對話方塊中的 [複製並開啟]  。

   ![[Azure 登入] 對話方塊視窗][I03]

> [!NOTE]
>
> 如果瀏覽器未開啟，請將 Eclipse 設定為使用外部瀏覽器，如 Internet Explorer、Firefox 或 Chrome：
>
> 1. 在 Eclipse 中開啟 [喜好設定] -> [一般] -> [網頁瀏覽器] -> [使用外部網頁瀏覽器]
>
> 2. 選取您想要使用的瀏覽器
>

5. 在瀏覽器中，貼上您的裝置程式碼 (您在上一個步驟中按一下 [複製並開啟]  時所複製)，然後按 [下一步]  。

   ![裝置登入瀏覽器][I04]

6. 最後，在 [選取訂用帳戶]  對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]  。

   ![[選取訂用帳戶] 對話方塊][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>使用服務主體來登入您的 Azure 帳戶

本節會逐步引導您建立認證檔案，並在檔案中包含您的服務主體資料。 在完成此程序後，Eclipse 會使用認證檔案，讓您在開啟專案時自動登入 Azure。

1. 使用 Eclipse 開啟您的專案。

2. 依序按一下 [工具]  、[Azure]  以及 [登入]  。
   ![Eclipse Azure 登入命令][A01]

3. 在 [Azure 登入]  視窗中，選取 [服務主體]  。 如果您還沒有服務主體驗證檔案，請按一下 [新增]  建立一個。 否則，您可以按一下 [瀏覽]  加以開啟，並跳到步驟 8。

   ![選取了 [服務主體] 的 [Azure 登入] 視窗][A02]

4. 按一下 [Azure 裝置登入]  對話方塊中的 [複製並開啟]  。

   ![[Azure 登入] 對話方塊視窗][A08]

> [!NOTE]
>
> 如果瀏覽器未開啟，請將 Eclipse 設定為使用外部瀏覽器，如 IE 或 Chrome：
>
> 1. 在 Eclipse 中開啟 [喜好設定] -> [一般] -> [網頁瀏覽器] -> [使用外部網頁瀏覽器]
>
> 2. 選取您想要使用的瀏覽器
>

5. 在瀏覽器中，貼上您的裝置程式碼 (您在上一個步驟中按一下 [複製並開啟]  時所複製)，然後按 [下一步]  。

   ![裝置登入瀏覽器][A03]

6. 在 [建立驗證檔案]  視窗中，選取您想要使用的訂用帳戶，選擇您的目的地目錄，然後按一下 [啟動]  。

   ![[建立驗證檔案] 視窗][A04]

7. 在 [服務主體建立狀態]  對話方塊中，於系統成功建立檔案之後按一下 [確定]  。

   ![[服務主體建立狀態] 對話方塊][A05]

8. 已建立檔案的位址會自動填入 [Azure 登入]  視窗中，此時請按一下 [登入]  。

   ![[Azure 登入] 對話方塊][A06]

9. 最後，在 [選取訂用帳戶]  對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]  。

   ![[選取訂用帳戶] 對話方塊][A07]

## <a name="sign-out-of-your-azure-account"></a>登出您的 Azure 帳戶

在前述步驟中設定好帳戶後，每當您啟動 Eclipse 時，系統就會將您自動登入。 不過，如果您想要登出 Azure 帳戶，請使用下列步驟。

1. 在 Eclipse 中，依序按一下 [工具]  、[Azure]  以及 [登出]  。

   ![Azure 登出的 Eclipse 功能表][L01]

2. 當 [Azure 登出]  對話方塊出現時，按一下 [是]  。

   ![[登出] 對話方塊][L02]

## <a name="next-steps"></a>後續步驟

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
