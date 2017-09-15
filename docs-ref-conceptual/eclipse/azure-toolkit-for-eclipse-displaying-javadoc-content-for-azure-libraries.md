---
title: "在 Eclipse 中顯示 Azure Libraries for Java 封裝的 Javadoc 內容"
description: "如何在 Eclipse 中顯示 Azure Libraries 的 Javadoc 內容。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 2d1f03f993f0c88401efaa9985f773b9a4b92cc4
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a><span data-ttu-id="a98f3-103">在 Eclipse 中顯示 Azure Libraries for Java 封裝的 Javadoc 內容</span><span class="sxs-lookup"><span data-stu-id="a98f3-103">Displaying Javadoc Content in Eclipse for the Azure Libraries Package for Java</span></span>

<span data-ttu-id="a98f3-104">Javadoc 內容與 Azure Libraries for Java 產生關聯時，即可在 Eclipse 環境中檢視 Azure Libraries for Java 的 Javadoc 內容。</span><span class="sxs-lookup"><span data-stu-id="a98f3-104">The Javadoc content for the Azure Libraries for Java can be viewed within your Eclipse environment by associating the Javadoc content to the Azure Libraries for Java.</span></span> <span data-ttu-id="a98f3-105">下列步驟示範該如何在 Eclipse 中使用此功能。</span><span class="sxs-lookup"><span data-stu-id="a98f3-105">The following steps show you how to use this functionality within Eclipse.</span></span>

<span data-ttu-id="a98f3-106">此程序假設您已將 Azure Library for Java 新增至您的組建路徑。</span><span class="sxs-lookup"><span data-stu-id="a98f3-106">This procedure assumes you have already added the Azure Library for Java to your build path.</span></span>

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a><span data-ttu-id="a98f3-107">在 Eclipse 中顯示 Azure Libraries for Java 的 Javadoc 內容</span><span class="sxs-lookup"><span data-stu-id="a98f3-107">To display Javadoc content in Eclipse for the Azure Libraries for Java</span></span>

1. <span data-ttu-id="a98f3-108">於 Eclipse 的 [專案總管] 中，在您專案的 [ **所參考的資料庫** ] 區段中，開啟 Azure library for Java JAR 的內容功能表。</span><span class="sxs-lookup"><span data-stu-id="a98f3-108">Within Eclipse's Project Explorer, in the **Referenced Libraries** section of your project, open the context menu for the Azure Library for Java JAR.</span></span> <span data-ttu-id="a98f3-109">例如， **microsoft-windowsazure-api-0.1.0.jar** (視您所安裝的版本而定，版本號碼可能會有所不同)。</span><span class="sxs-lookup"><span data-stu-id="a98f3-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (the version number may be different, dependent upon which version you have installed).</span></span>

1. <span data-ttu-id="a98f3-110">按一下 [內容] 。</span><span class="sxs-lookup"><span data-stu-id="a98f3-110">Click **Properties**.</span></span>

1. <span data-ttu-id="a98f3-111">在 [內容] 對話方塊的左側窗格中，按一下 [Javadoc 位置]。</span><span class="sxs-lookup"><span data-stu-id="a98f3-111">Within the **Properties** dialog, in the left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="a98f3-112">隨即顯示 [ **Javadoc 位置** ] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="a98f3-112">The **Javadoc Location** dialog is displayed.</span></span>

1. <span data-ttu-id="a98f3-113">您可以指定 [Javadoc URL]或 [封存檔案中的 Javadoc]。</span><span class="sxs-lookup"><span data-stu-id="a98f3-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="a98f3-114">如果您選擇指定 [Javadoc URL]，請使用 URL，例如 **http://dl.windowsazure.com/javadoc** 或 **http://dl.windowsazure.com/storage/javadoc**。</span><span class="sxs-lookup"><span data-stu-id="a98f3-114">If you choose to specify a **Javadoc URL**, use the URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="a98f3-115">若選擇使用 **封存檔案中的 Javadoc**，可以指定外部檔案或工作區檔案。</span><span class="sxs-lookup"><span data-stu-id="a98f3-115">If you choose to use **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="a98f3-116">選擇後視需要進行瀏覽/驗證。</span><span class="sxs-lookup"><span data-stu-id="a98f3-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="a98f3-117">下列範例中，Azure Libraries for Java 會與已下載至本機 **c:\MyAzureJARs** 資料夾的對應 Javadoc JAR 產生關聯。</span><span class="sxs-lookup"><span data-stu-id="a98f3-117">The following example associates the Azure Libraries for Java with the corresponding Javadoc JAR that has been downloaded locally to a folder named **c:\MyAzureJARs**.</span></span>

   ![顯示 Javadoc 內容。][ic553487]

1. <span data-ttu-id="a98f3-119">選擇性步驟：按一下 [驗證]。</span><span class="sxs-lookup"><span data-stu-id="a98f3-119">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="a98f3-120">這裡可能會顯示 Javadoc JAR 可能發生的問題。</span><span class="sxs-lookup"><span data-stu-id="a98f3-120">Potential issues with the Javadoc JAR could be displayed here.</span></span>

1. <span data-ttu-id="a98f3-121">按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="a98f3-121">Click **OK**.</span></span>

<span data-ttu-id="a98f3-122">與程式庫產生關聯後，Javadoc 內容隨即會顯示在 Eclipse 整合式開發環境 (IDE) 中。</span><span class="sxs-lookup"><span data-stu-id="a98f3-122">Once associated with the library, the Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="a98f3-123">例如，如果 `blob` 在程式碼中定義為 `CloudBlockBlob` 類型，以下為您在程式碼中輸入 `blob.acquireLease` 時，會顯示的 Javadoc 內容的範例：</span><span class="sxs-lookup"><span data-stu-id="a98f3-123">For example, if `blob` is defined of type `CloudBlockBlob` within your code, the following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![顯示 Javadoc 內容。][ic553488]

## <a name="next-steps"></a><span data-ttu-id="a98f3-125">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a98f3-125">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

<!-- IMG List -->

[ic553487]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
