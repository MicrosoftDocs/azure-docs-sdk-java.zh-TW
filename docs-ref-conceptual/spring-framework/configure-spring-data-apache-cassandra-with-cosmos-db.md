---
title: 如何搭配使用 Spring Data Apache Cassandra API 和 Azure Cosmos DB
description: 了解如何搭配使用 Spring Data Apache Cassandra API 和 Azure Cosmos DB。
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
ms.openlocfilehash: 1f3f7a55b49d64077df9e7d4d6acc3af4495b424
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992121"
---
# <a name="how-to-use-spring-data-apache-cassandra-api-with-azure-cosmos-db"></a><span data-ttu-id="b826f-103">如何搭配使用 Spring Data Apache Cassandra API 和 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b826f-103">How to use Spring Data Apache Cassandra API with Azure Cosmos DB</span></span>

## <a name="overview"></a><span data-ttu-id="b826f-104">概觀</span><span class="sxs-lookup"><span data-stu-id="b826f-104">Overview</span></span>

<span data-ttu-id="b826f-105">本文示範如何建立使用 [Spring Data] 的應用程式範例，以使用 [Azure Cosmos DB Cassandra API](/azure/cosmos-db/cassandra-introduction) 儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="b826f-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information using the [Azure Cosmos DB Cassandra API](/azure/cosmos-db/cassandra-introduction).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b826f-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="b826f-106">Prerequisites</span></span>

<span data-ttu-id="b826f-107">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="b826f-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="b826f-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="b826f-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="b826f-109">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="b826f-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="b826f-110">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="b826f-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="b826f-111">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="b826f-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="b826f-112">[Curl](https://curl.haxx.se/) 或類似的 HTTP 公用程式，以便測試功能。</span><span class="sxs-lookup"><span data-stu-id="b826f-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="b826f-113">[Git](https://git-scm.com/downloads) 用戶端。</span><span class="sxs-lookup"><span data-stu-id="b826f-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="b826f-114">建立 Azure Cosmos DB 帳戶</span><span class="sxs-lookup"><span data-stu-id="b826f-114">Create an Azure Cosmos DB account</span></span>

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a><span data-ttu-id="b826f-115">使用 Azure 入口網站建立 Cosmos DB 帳戶</span><span class="sxs-lookup"><span data-stu-id="b826f-115">Create a Cosmos DB account using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="b826f-116">您可以在 [Azure Cosmos DB 文件](/azure/cosmos-db/)中閱讀更多有關如何建立 Azure Cosmos DB 帳戶的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b826f-116">You can read more detailed information about creating Azure Cosmos DB accounts in [Azure Cosmos DB Documentation](/azure/cosmos-db/).</span></span>

1. <span data-ttu-id="b826f-117">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="b826f-117">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="b826f-118">按一下 [+建立資源]、[資料庫]，然後按一下 [Azure Cosmos DB]。</span><span class="sxs-lookup"><span data-stu-id="b826f-118">Click **+Create a resource**, then **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![建立 Azure Cosmos DB 帳戶][COSMOSDB01]

1. <span data-ttu-id="b826f-120">指定下列資訊：</span><span class="sxs-lookup"><span data-stu-id="b826f-120">Specify the following information:</span></span>

   - <span data-ttu-id="b826f-121">訂用帳戶：指定您要使用的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="b826f-121">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="b826f-122">**資源群組**：指定是要建立新的資源群組，還是選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="b826f-122">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="b826f-123">**帳戶名稱**：為 Cosmos DB 帳戶選擇唯一的名稱；此名稱將用來建立完整網域名稱，例如 wingtiptoyscassandra.documents.azure.com。</span><span class="sxs-lookup"><span data-stu-id="b826f-123">**Account name**: Choose a unique name for your Cosmos DB account; this will be used to create a fully-qualified domain name like *wingtiptoyscassandra.documents.azure.com*.</span></span>
   - <span data-ttu-id="b826f-124">**API**：在此教學課程中指定 `Cassandra`。</span><span class="sxs-lookup"><span data-stu-id="b826f-124">**API**: Specify `Cassandra` for this tutorial.</span></span>
   - <span data-ttu-id="b826f-125">**位置**：指定與資料庫最為接近的地理區域。</span><span class="sxs-lookup"><span data-stu-id="b826f-125">**Location**: Specify the closest geographic region for your database.</span></span>

   ![指定 Cosmos DB 帳戶的設定][COSMOSDB02]
   
1. <span data-ttu-id="b826f-127">上述所有資訊皆輸入完成時，按一下 [檢閱 + 建立]。</span><span class="sxs-lookup"><span data-stu-id="b826f-127">When you have entered all of the above information, click **Review + create**.</span></span>

1. <span data-ttu-id="b826f-128">如果 [檢閱] 頁面上的所有資訊皆正確，按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="b826f-128">If everything looks correct on the review page, click **Create**.</span></span>

   ![檢閱 Cosmos DB 帳戶的設定][COSMOSDB03]

### <a name="add-a-keyspace-to-your-azure-cosmos-db-account"></a><span data-ttu-id="b826f-130">在 Azure Cosmos DB 帳戶中新增 Keyspace</span><span class="sxs-lookup"><span data-stu-id="b826f-130">Add a keyspace to your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="b826f-131">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="b826f-131">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="b826f-132">按一下 [所有資源]，然後按一下您剛才建立的 Azure Cosmos DB 帳戶。</span><span class="sxs-lookup"><span data-stu-id="b826f-132">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![選取 Azure Cosmos DB 帳戶][COSMOSDB04]

1. <span data-ttu-id="b826f-134">按一下 [資料總管]，然後按一下 [新增 Keyspace]。</span><span class="sxs-lookup"><span data-stu-id="b826f-134">Click **Data Explorer**, then click **New Keyspace**.</span></span> <span data-ttu-id="b826f-135">輸入唯一的識別碼來作為 [Keyspace 識別碼]，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b826f-135">Enter a unique identifier for your **Keyspace id**, then click **OK**.</span></span>

   ![建立 Cosmos DB Keyspace][COSMOSDB05]

### <a name="retrieve-the-connection-settings-for-your-azure-cosmos-db-account"></a><span data-ttu-id="b826f-137">擷取 Azure Cosmos DB 帳戶的連線設定</span><span class="sxs-lookup"><span data-stu-id="b826f-137">Retrieve the connection settings for your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="b826f-138">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="b826f-138">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="b826f-139">按一下 [所有資源]，然後按一下您剛才建立的 Azure Cosmos DB 帳戶。</span><span class="sxs-lookup"><span data-stu-id="b826f-139">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![選取 Azure Cosmos DB 帳戶][COSMOSDB04]

1. <span data-ttu-id="b826f-141">按一下 [連接字串]，然後複製 [連絡點]、[連接埠]、[使用者名稱] 和 [主要密碼] 欄位的值；您之後會使用這些值來設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="b826f-141">Click **Connection strings**, and copy the values for the **Contact Point**, **Port**, **Username**, and **Primary Password** fields; you will use those values to configure your application later.</span></span>

   ![擷取 Cosmos DB 的連線設定][COSMOSDB05]

## <a name="configure-the-sample-application"></a><span data-ttu-id="b826f-143">設定範例應用程式</span><span class="sxs-lookup"><span data-stu-id="b826f-143">Configure the sample application</span></span>

1. <span data-ttu-id="b826f-144">開啟命令殼層，並使用 git 命令複製範例專案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="b826f-144">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-cassandra-on-azure.git
   ```

1. <span data-ttu-id="b826f-145">在範例專案的 [資源] 目錄中尋找 application.properties 檔案，如果該檔案不存在，則加以建立。</span><span class="sxs-lookup"><span data-stu-id="b826f-145">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="b826f-146">在文字編輯器中開啟 application.properties 檔案、在檔案中新增或設定下列幾行，然後使用本文稍早的適當值來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="b826f-146">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.data.cassandra.contact-points=wingtiptoyscassandra.cassandra.cosmosdb.azure.com
   spring.data.cassandra.port=10350
   spring.data.cassandra.username=wingtiptoyscassandra
   spring.data.cassandra.password=********
   ```
   <span data-ttu-id="b826f-147">其中：</span><span class="sxs-lookup"><span data-stu-id="b826f-147">Where:</span></span>

   | <span data-ttu-id="b826f-148">參數</span><span class="sxs-lookup"><span data-stu-id="b826f-148">Parameter</span></span> | <span data-ttu-id="b826f-149">說明</span><span class="sxs-lookup"><span data-stu-id="b826f-149">Description</span></span> |
   |---|---|
   | `spring.data.cassandra.contact-points` | <span data-ttu-id="b826f-150">指定本文稍早所述的**連絡點**。</span><span class="sxs-lookup"><span data-stu-id="b826f-150">Specifies the **Contact Point** from earlier in this article.</span></span> |
   | `spring.data.cassandra.port` | <span data-ttu-id="b826f-151">指定本文稍早所述的**連接埠**。</span><span class="sxs-lookup"><span data-stu-id="b826f-151">Specifies the **Port** from earlier in this article.</span></span> |
   | `spring.data.cassandra.username` | <span data-ttu-id="b826f-152">指定本文稍早所述的**使用者名稱**。</span><span class="sxs-lookup"><span data-stu-id="b826f-152">Specifies your **Username** from earlier in this article.</span></span> |
   | `spring.data.cassandra.password` | <span data-ttu-id="b826f-153">指定本文稍早所述的**主要密碼**。</span><span class="sxs-lookup"><span data-stu-id="b826f-153">Specifies your **Primary Password** from earlier in this article.</span></span> |

1. <span data-ttu-id="b826f-154">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b826f-154">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="b826f-155">封裝和測試應用程式範例</span><span class="sxs-lookup"><span data-stu-id="b826f-155">Package and test the sample application</span></span> 

1. <span data-ttu-id="b826f-156">使用 Maven 建置應用程式範例；例如：</span><span class="sxs-lookup"><span data-stu-id="b826f-156">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="b826f-157">啟動應用程式範例；例如：</span><span class="sxs-lookup"><span data-stu-id="b826f-157">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-cassandra-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="b826f-158">從命令提示字元使用 `curl` 建立新的記錄，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="b826f-158">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="b826f-159">應用程式應該會傳回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="b826f-159">Your application should return values like the following:</span></span>

   ```shell
   Added Pet{id=60fa8cb0-0423-11e9-9a70-39311962166b, name='dog', species='canine'}.

   Added Pet{id=72c1c9e0-0423-11e9-9a70-39311962166b, name='cat', species='feline'}.
   ```

1. <span data-ttu-id="b826f-160">從命令提示字元使用 `curl` 擷取所有現有的記錄，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="b826f-160">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="b826f-161">應用程式應該會傳回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="b826f-161">Your application should return values like the following:</span></span>

   ```json
   [{"id":"60fa8cb0-0423-11e9-9a70-39311962166b","name":"dog","species":"canine"},{"id":"72c1c9e0-0423-11e9-9a70-39311962166b","name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="b826f-162">總結</span><span class="sxs-lookup"><span data-stu-id="b826f-162">Summary</span></span>

<span data-ttu-id="b826f-163">在此教學課程中，您已建立使用 Spring Data 的範例 Java 應用程式，以使用 Azure Cosmos DB Cassandra API 來儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="b826f-163">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information using the Azure Cosmos DB Cassandra API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b826f-164">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b826f-164">Next steps</span></span>

<span data-ttu-id="b826f-165">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="b826f-165">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b826f-166">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="b826f-166">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="b826f-167">其他資源</span><span class="sxs-lookup"><span data-stu-id="b826f-167">Additional Resources</span></span>

<span data-ttu-id="b826f-168">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="b826f-168">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[COSMOSDB01]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB05]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-05.png
