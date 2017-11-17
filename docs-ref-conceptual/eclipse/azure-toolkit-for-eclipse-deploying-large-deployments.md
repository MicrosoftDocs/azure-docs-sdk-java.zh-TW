---
title: "部署大型部署"
description: "了解如何運用適用於 Eclipse 的 Azure 工具組部署大型部署。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 0e367e74d038043f1626dbf19aab87db6451bc02
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2017
---
# <a name="deploying-large-deployments"></a><span data-ttu-id="c3483-103">部署大型部署</span><span class="sxs-lookup"><span data-stu-id="c3483-103">Deploying Large Deployments</span></span>

<span data-ttu-id="c3483-104">如果您的部署太大，預設的 approot 資料夾無法容納時，可以使用本機儲存資源，作為 JDK 和應用程式伺服器的部署根資料夾。</span><span class="sxs-lookup"><span data-stu-id="c3483-104">If your deployment is too large to be contained in the default approot folder, you can use a local storage resource as the deployment root folder for your JDK and application server.</span></span>

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a><span data-ttu-id="c3483-105">使用本機儲存資源作為大型部署的部署根資料夾</span><span class="sxs-lookup"><span data-stu-id="c3483-105">To use a local storage resource as the deployment root folder for large deployments</span></span>

1. <span data-ttu-id="c3483-106">建立新的本機儲存資源。</span><span class="sxs-lookup"><span data-stu-id="c3483-106">Create a new local storage resource.</span></span> <span data-ttu-id="c3483-107">資源名稱不重要。</span><span class="sxs-lookup"><span data-stu-id="c3483-107">The name of the resource does not matter.</span></span> <span data-ttu-id="c3483-108">儲存資源以角色層級定義。</span><span class="sxs-lookup"><span data-stu-id="c3483-108">Storage resources are defined at the role level.</span></span> <span data-ttu-id="c3483-109">存取 [本機儲存組態] 對話方塊 (您可透過此對話方塊建立新的本機儲存資源) 最快速的方式是執行下列步驟：以滑鼠右鍵按一下 [專案總管] 檢視中的角色 (若未看見該角色，請展開 Azure 專案節點)，按一下 [Azure]，再按一下 [本機儲存體]。</span><span class="sxs-lookup"><span data-stu-id="c3483-109">The quickest way to access the local storage configuration dialog, from which you could create a new local storage resource, is by using the following steps: Right-click the role in the **Project Explorer** view (expand your Azure project node if you don't see the role), click **Azure**, and then click **Local Storage**.</span></span> <span data-ttu-id="c3483-110">在 [本機儲存體] 對話方塊中，按一下 [新增] 建立新的本機儲存資源。</span><span class="sxs-lookup"><span data-stu-id="c3483-110">Within the **Local Storage** dialog, click **Add** to create a new local storage resource.</span></span>

1. <span data-ttu-id="c3483-111">將所需的儲存容量設為至少 2048 MB (若小於此設定值，可能會發生 approot 中相同的容量問題)。</span><span class="sxs-lookup"><span data-stu-id="c3483-111">Set the desired size to at least 2048 MB (anything less may cause the same file size problems as you would encounter in the approot).</span></span>

1. <span data-ttu-id="c3483-112">請確定已勾選 [ **回收角色執行個體時清除內容** ]，這有助於回收角色執行個體時，避免部署的啟動邏輯與資源中既存的檔案發生衝突。</span><span class="sxs-lookup"><span data-stu-id="c3483-112">Ensure that **Clean the contents when the role instance is recycled** is checked; this will help prevent the deployment's startup logic from running into conflicts with pre-existing files in the resource when the role instance is recycled.</span></span>

1. <span data-ttu-id="c3483-113">請確定 [部署後儲存資源目錄路徑的環境變數] 值設為字串 **DEPLOYROOT**。</span><span class="sxs-lookup"><span data-stu-id="c3483-113">Ensure that the **Environment variable storing the resource's directory path after deployment** value is set to the string **DEPLOYROOT**.</span></span> <span data-ttu-id="c3483-114">[本機儲存資源] 對話方塊如下所示。</span><span class="sxs-lookup"><span data-stu-id="c3483-114">Your local storage resource dialog will look similar to the following.</span></span>

   ![][ic667943]

<span data-ttu-id="c3483-115">或者，若您使用 **DEPLOYROOT** 作為您本機資源的「名稱」，且您未變更自動產生的環境變數名稱 (此例將該名稱設為 **DEPLOYROOT_PATH**)，這也適用於您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c3483-115">Alternatively, if you use **DEPLOYROOT** as the *name* of your local resource and you do not change the automatically-generated environment variable name (which will be set to **DEPLOYROOT_PATH** in that case), that would work for your application as well.</span></span>

<span data-ttu-id="c3483-116">如需有關建立本機儲存資源的其他資訊，請參閱[本機儲存體屬性][Local storage properties]。</span><span class="sxs-lookup"><span data-stu-id="c3483-116">Additional information about creating a local storage resource can be found at [Local storage properties][Local storage properties].</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3483-117">後續步驟</span><span class="sxs-lookup"><span data-stu-id="c3483-117">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
