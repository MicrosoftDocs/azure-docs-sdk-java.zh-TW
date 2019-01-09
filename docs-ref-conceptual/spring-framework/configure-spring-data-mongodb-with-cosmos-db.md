---
title: 如何搭配使用 Spring Data MongoDB API 和 Azure Cosmos DB
description: 了解如何搭配使用 Spring Data MongoDB API 和 Azure Cosmos DB。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 55fb356820e2cc5eb8d1fe4030053d9e580583af
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992160"
---
# <a name="how-to-use-spring-data-mongodb-api-with-azure-cosmos-db"></a><span data-ttu-id="db116-103">如何搭配使用 Spring Data MongoDB API 和 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="db116-103">How to use Spring Data MongoDB API with Azure Cosmos DB</span></span>

## <a name="overview"></a><span data-ttu-id="db116-104">概觀</span><span class="sxs-lookup"><span data-stu-id="db116-104">Overview</span></span>

<span data-ttu-id="db116-105">本文示範如何建立使用 [Spring Data] 的應用程式範例，以使用 [Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction) 儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="db116-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information using the [Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db116-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="db116-106">Prerequisites</span></span>

<span data-ttu-id="db116-107">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="db116-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="db116-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="db116-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="db116-109">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="db116-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="db116-110">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="db116-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="db116-111">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="db116-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="db116-112">[Git](https://git-scm.com/downloads) 用戶端。</span><span class="sxs-lookup"><span data-stu-id="db116-112">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="db116-113">建立 Azure Cosmos DB 帳戶</span><span class="sxs-lookup"><span data-stu-id="db116-113">Create an Azure Cosmos DB account</span></span>

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a><span data-ttu-id="db116-114">使用 Azure 入口網站建立 Cosmos DB 帳戶</span><span class="sxs-lookup"><span data-stu-id="db116-114">Create a Cosmos DB account using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="db116-115">您可以在 [Azure Cosmos DB 文件](/azure/cosmos-db/)中閱讀更多有關如何建立 Azure Cosmos DB 帳戶的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="db116-115">You can read more detailed information about creating Azure Cosmos DB accounts in [Azure Cosmos DB Documentation](/azure/cosmos-db/).</span></span>

1. <span data-ttu-id="db116-116">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="db116-116">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="db116-117">按一下 [+建立資源]、[資料庫]，然後按一下 [Azure Cosmos DB]。</span><span class="sxs-lookup"><span data-stu-id="db116-117">Click **+Create a resource**, then **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![建立 Azure Cosmos DB 帳戶][COSMOSDB01]

1. <span data-ttu-id="db116-119">指定下列資訊：</span><span class="sxs-lookup"><span data-stu-id="db116-119">Specify the following information:</span></span>

   - <span data-ttu-id="db116-120">訂用帳戶：指定您要使用的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="db116-120">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="db116-121">**資源群組**：指定是要建立新的資源群組，還是選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="db116-121">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="db116-122">**帳戶名稱**：為 Cosmos DB 帳戶選擇唯一的名稱；此名稱將用來建立完整網域名稱，例如 wingtiptoysmongodb.documents.azure.com。</span><span class="sxs-lookup"><span data-stu-id="db116-122">**Account name**: Choose a unique name for your Cosmos DB account; this will be used to create a fully-qualified domain name like *wingtiptoysmongodb.documents.azure.com*.</span></span>
   - <span data-ttu-id="db116-123">**API**：在此教學課程中指定 `Azure Cosmos DB for MongoDB API`。</span><span class="sxs-lookup"><span data-stu-id="db116-123">**API**: Specify `Azure Cosmos DB for MongoDB API` for this tutorial.</span></span>
   - <span data-ttu-id="db116-124">**位置**：指定與資料庫最為接近的地理區域。</span><span class="sxs-lookup"><span data-stu-id="db116-124">**Location**: Specify the closest geographic region for your database.</span></span>

   ![指定 Cosmos DB 帳戶的設定][COSMOSDB02]
   
1. <span data-ttu-id="db116-126">上述所有資訊皆輸入完成時，按一下 [檢閱 + 建立]。</span><span class="sxs-lookup"><span data-stu-id="db116-126">When you have entered all of the above information, click **Review + create**.</span></span>

1. <span data-ttu-id="db116-127">如果 [檢閱] 頁面上的所有資訊皆正確，按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="db116-127">If everything looks correct on the review page, click **Create**.</span></span>

   ![檢閱 Cosmos DB 帳戶的設定][COSMOSDB03]

### <a name="retrieve-the-connection-string-for-your-azure-cosmos-db-account"></a><span data-ttu-id="db116-129">擷取 Azure Cosmos DB 帳戶的連接字串</span><span class="sxs-lookup"><span data-stu-id="db116-129">Retrieve the connection string for your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="db116-130">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="db116-130">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="db116-131">按一下 [所有資源]，然後按一下您剛才建立的 Azure Cosmos DB 帳戶。</span><span class="sxs-lookup"><span data-stu-id="db116-131">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![選取 Azure Cosmos DB 帳戶][COSMOSDB04]

1. <span data-ttu-id="db116-133">按一下 [連接字串]，然後複製 [主要連接字串] 欄位的值；您之後會使用該值來設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="db116-133">Click **Connection strings**, and copy the value for the **Primary Connection String** field; you will use that value to configure your application later.</span></span>

   ![擷取 Cosmos DB 的連接字串][COSMOSDB06]

## <a name="configure-the-sample-application"></a><span data-ttu-id="db116-135">設定範例應用程式</span><span class="sxs-lookup"><span data-stu-id="db116-135">Configure the sample application</span></span>

1. <span data-ttu-id="db116-136">開啟命令殼層，並使用 git 命令複製範例專案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="db116-136">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/spring-guides/gs-accessing-data-mongodb.git
   ```

1. <span data-ttu-id="db116-137">在範例專案的 &lt;專案根目錄&gt;/complete/src/main 中建立 resources 目錄，然後在 resources 目錄中建立 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="db116-137">Create a *resources* directory in the *&lt;project root&gt;/complete/src/main* directory of the sample project, and create an *application.properties* file in the *resources* directory.</span></span>

1. <span data-ttu-id="db116-138">在文字編輯器中開啟 application.properties 檔案、在檔案中新增下列幾行，然後使用本文稍早的適當值來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="db116-138">Open the *application.properties* file in a text editor, and add the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.data.mongodb.database=wingtiptoysmongodb
   spring.data.mongodb.uri=mongodb://wingtiptoysmongodb:AbCdEfGhIjKlMnOpQrStUvWxYz==@wingtiptoysmongodb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
   ```
   <span data-ttu-id="db116-139">其中：</span><span class="sxs-lookup"><span data-stu-id="db116-139">Where:</span></span>

   | <span data-ttu-id="db116-140">參數</span><span class="sxs-lookup"><span data-stu-id="db116-140">Parameter</span></span> | <span data-ttu-id="db116-141">說明</span><span class="sxs-lookup"><span data-stu-id="db116-141">Description</span></span> |
   |---|---|
   | `spring.data.mongodb.database` | <span data-ttu-id="db116-142">指定本文稍早所述 Cosmos DB 帳戶的名稱。</span><span class="sxs-lookup"><span data-stu-id="db116-142">Specifies the name of your Cosmos DB account from earlier in this article.</span></span> |
   | `spring.data.mongodb.uri` | <span data-ttu-id="db116-143">指定本文稍早所述的**主要連接字串**。</span><span class="sxs-lookup"><span data-stu-id="db116-143">Specifies the **Primary Connection String** from earlier in this article.</span></span> |

1. <span data-ttu-id="db116-144">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="db116-144">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="db116-145">封裝和測試應用程式範例</span><span class="sxs-lookup"><span data-stu-id="db116-145">Package and test the sample application</span></span> 

1. <span data-ttu-id="db116-146">使用 Maven 建置應用程式範例，然後設定 Maven 來略過測試；例如：</span><span class="sxs-lookup"><span data-stu-id="db116-146">Build the sample application with Maven, and configure Maven to skip tests; for example:</span></span>

   ```shell
   mvn clean package -DskipTests
   ```

1. <span data-ttu-id="db116-147">啟動應用程式範例；例如：</span><span class="sxs-lookup"><span data-stu-id="db116-147">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/gs-accessing-data-mongodb-0.1.0.jar
   ```
    
   <span data-ttu-id="db116-148">應用程式應該會傳回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="db116-148">Your application should return values like the following:</span></span>

   ```json
   Customers found with findAll():
   -------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   
   Customer found with findByFirstName('Alice'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customers found with findByLastName('Smith'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   ```

## <a name="summary"></a><span data-ttu-id="db116-149">總結</span><span class="sxs-lookup"><span data-stu-id="db116-149">Summary</span></span>

<span data-ttu-id="db116-150">在此教學課程中，您已建立使用 Spring Data 的範例 Java 應用程式，以使用 Azure Cosmos DB MongoDB API 來儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="db116-150">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information using the Azure Cosmos DB MongoDB API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db116-151">後續步驟</span><span class="sxs-lookup"><span data-stu-id="db116-151">Next steps</span></span>

<span data-ttu-id="db116-152">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="db116-152">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="db116-153">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="db116-153">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="db116-154">其他資源</span><span class="sxs-lookup"><span data-stu-id="db116-154">Additional Resources</span></span>

<span data-ttu-id="db116-155">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="db116-155">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<!-- URL List -->

[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[COSMOSDB01]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB06]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-06.png
