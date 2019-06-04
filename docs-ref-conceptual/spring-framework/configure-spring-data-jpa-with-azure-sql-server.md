---
title: 如何搭配使用 Spring Data JPA 和 Azure SQL Database
description: 了解如何搭配使用 Spring Data JPA 和 Azure SQL 資料庫。
services: sql-database
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 02b6eff059c8b7dff1c7473d0460ca44e76f6f2e
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2019
ms.locfileid: "64673961"
---
# <a name="how-to-use-spring-data-jpa-with-azure-sql-database"></a><span data-ttu-id="2a1de-103">如何搭配使用 Spring Data JPA 和 Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="2a1de-103">How to use Spring Data JPA with Azure SQL Database</span></span>

## <a name="overview"></a><span data-ttu-id="2a1de-104">概觀</span><span class="sxs-lookup"><span data-stu-id="2a1de-104">Overview</span></span>

<span data-ttu-id="2a1de-105">本文示範如何建立使用 [Spring Data] 的應用程式範例，以在 [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) 中使用 [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) 儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="2a1de-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) using [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a1de-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="2a1de-106">Prerequisites</span></span>

<span data-ttu-id="2a1de-107">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="2a1de-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="2a1de-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="2a1de-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="2a1de-109">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="2a1de-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="2a1de-110">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="2a1de-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="2a1de-111">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="2a1de-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="2a1de-112">[Curl](https://curl.haxx.se/) 或類似的 HTTP 公用程式，以便測試功能。</span><span class="sxs-lookup"><span data-stu-id="2a1de-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="2a1de-113">[Git](https://git-scm.com/downloads) 用戶端。</span><span class="sxs-lookup"><span data-stu-id="2a1de-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="2a1de-114">建立 Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="2a1de-114">Create an Azure SQL Database</span></span>

### <a name="create-a-sql-database-server-using-the-azure-portal"></a><span data-ttu-id="2a1de-115">使用 Azure 入口網站建立 SQL 資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="2a1de-115">Create a SQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="2a1de-116">您可以在[在 Azure 入口網站中建立 Azure SQL 資料庫](/azure/sql-database/sql-database-get-started-portal)中，閱讀更多關於如何建立 Azure SQL Database 的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="2a1de-116">You can read more detailed information about creating Azure SQL databases in [Create an Azure SQL database in the Azure portal](/azure/sql-database/sql-database-get-started-portal).</span></span>

1. <span data-ttu-id="2a1de-117">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="2a1de-117">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="2a1de-118">按一下 [+建立資源]  、[資料庫]  ，然後按一下 [SQL Database]  。</span><span class="sxs-lookup"><span data-stu-id="2a1de-118">Click **+Create a resource**, then **Databases**, and then click **SQL Database**.</span></span>

   ![建立 SQL 資料庫][SQL01]

1. <span data-ttu-id="2a1de-120">指定下列資訊：</span><span class="sxs-lookup"><span data-stu-id="2a1de-120">Specify the following information:</span></span>

   - <span data-ttu-id="2a1de-121">**資料庫名稱**：為 SQL Database 選擇唯一的名稱；您會在稍後將會指定的 SQL 伺服器中建立此名稱。</span><span class="sxs-lookup"><span data-stu-id="2a1de-121">**Database name**: Choose a unique name for your SQL database; this will be created in the SQL server that you will specify later.</span></span>
   - <span data-ttu-id="2a1de-122">訂用帳戶  ：指定您要使用的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="2a1de-122">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="2a1de-123">**資源群組**：指定是要建立新的資源群組，還是選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="2a1de-123">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="2a1de-124">**選取來源**：在此教學課程中，選取 `Blank database` 以建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="2a1de-124">**Select source**: For this tutorial, select `Blank database` to create a new database.</span></span>

   ![指定 SQL Database 屬性][SQL02]
   
1. <span data-ttu-id="2a1de-126">按一下 [伺服器]  、[建立新的伺服器]  ，然後指定下列資訊：</span><span class="sxs-lookup"><span data-stu-id="2a1de-126">Click **Server**, then **Create a new server**, and then specify the following information:</span></span>

   - <span data-ttu-id="2a1de-127">**伺服器名稱**：為 SQL 伺服器選擇唯一的名稱；此名稱將用來建立完整網域名稱，例如 wingtiptoyssql.database.windows.net  。</span><span class="sxs-lookup"><span data-stu-id="2a1de-127">**Server name**: Choose a unique name for your SQL server; this will be used to create a fully-qualified domain name like *wingtiptoyssql.database.windows.net*.</span></span>
   - <span data-ttu-id="2a1de-128">**伺服器管理員登入**：指定資料庫管理員的名稱。</span><span class="sxs-lookup"><span data-stu-id="2a1de-128">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="2a1de-129">**密碼**和**確認密碼**：指定資料庫管理員的密碼。</span><span class="sxs-lookup"><span data-stu-id="2a1de-129">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="2a1de-130">**位置**：指定與資料庫最為接近的地理區域。</span><span class="sxs-lookup"><span data-stu-id="2a1de-130">**Location**: Specify the closest geographic region for your database.</span></span>

   ![指定 SQL 伺服器][SQL03]

1. <span data-ttu-id="2a1de-132">上述所有資訊皆輸入完成時，按一下 [選取]  。</span><span class="sxs-lookup"><span data-stu-id="2a1de-132">When you have entered all of the above information, click **Select**.</span></span>

1. <span data-ttu-id="2a1de-133">在此教學課程中，指定價格最實惠的 [定價層]  ，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="2a1de-133">For this tutorial, specify the least-expensive **Pricing tier**, and then click **Create**.</span></span>

   ![建立 SQL Database][SQL04]

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a><span data-ttu-id="2a1de-135">使用 Azure 入口網站設定 SQL 伺服器的防火牆規則</span><span class="sxs-lookup"><span data-stu-id="2a1de-135">Configure a firewall rule for your SQL server using the Azure Portal</span></span>

1. <span data-ttu-id="2a1de-136">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="2a1de-136">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="2a1de-137">按一下 [所有資源]  ，然後按一下您剛才建立的 SQL 伺服器。</span><span class="sxs-lookup"><span data-stu-id="2a1de-137">Click **All Resources**, then click the SQL server you just created.</span></span>

   ![選取 SQL 伺服器][SQL05]

1. <span data-ttu-id="2a1de-139">在 [概觀]  區段中，按一下 [顯示防火牆設定] </span><span class="sxs-lookup"><span data-stu-id="2a1de-139">In the **Overview** section, click **Show firewall settings**</span></span>

   ![顯示防火牆設定][SQL06]

1. <span data-ttu-id="2a1de-141">在 [防火牆和虛擬網路]  區段中，藉由指定規則的唯一名稱來建立新的規則，然後輸入需要存取資料庫的 IP 位址範圍，再按一下 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="2a1de-141">In the **Firewalls and virtual networks** section, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![設定防火牆設定][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a><span data-ttu-id="2a1de-143">使用 Azure 入口網站擷取 SQL 伺服器的連接字串</span><span class="sxs-lookup"><span data-stu-id="2a1de-143">Retrieve the connection string for your SQL server using the Azure Portal</span></span>

1. <span data-ttu-id="2a1de-144">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="2a1de-144">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="2a1de-145">按一下 [所有資源]  ，然後按一下您剛才建立的 SQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="2a1de-145">Click **All Resources**, then click the SQL database you just created.</span></span>

   ![選取 SQL Database][SQL08]

1. <span data-ttu-id="2a1de-147">按一下 [連接字串]  ，然後按一下 [JDBC]  ，並複製 [JDBC] 文字欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="2a1de-147">Click **Connection strings**, then click **JDBC**, and copy the value in the JDBC text field.</span></span>

   ![擷取 JDBC 連接字串][SQL09]

## <a name="configure-the-sample-application"></a><span data-ttu-id="2a1de-149">設定範例應用程式</span><span class="sxs-lookup"><span data-stu-id="2a1de-149">Configure the sample application</span></span>

1. <span data-ttu-id="2a1de-150">開啟命令殼層，並使用 git 命令複製範例專案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="2a1de-150">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. <span data-ttu-id="2a1de-151">在範例專案的 [資源]  目錄中尋找 application.properties  檔案，如果該檔案不存在，則加以建立。</span><span class="sxs-lookup"><span data-stu-id="2a1de-151">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="2a1de-152">在文字編輯器中開啟 application.properties  檔案、在檔案中新增或設定下列幾行，然後使用本文稍早的適當值來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="2a1de-152">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   <span data-ttu-id="2a1de-153">其中：</span><span class="sxs-lookup"><span data-stu-id="2a1de-153">Where:</span></span>

   | <span data-ttu-id="2a1de-154">參數</span><span class="sxs-lookup"><span data-stu-id="2a1de-154">Parameter</span></span> | <span data-ttu-id="2a1de-155">說明</span><span class="sxs-lookup"><span data-stu-id="2a1de-155">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="2a1de-156">指定本文稍早所編輯的 SQL JDBC 字串版本。</span><span class="sxs-lookup"><span data-stu-id="2a1de-156">Specifies an edited version of your SQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="2a1de-157">指定本文稍早所述的 SQL 管理員名稱，並對其附加縮略的伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="2a1de-157">Specifies your SQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="2a1de-158">指定本文稍早所述的 SQL 管理員密碼。</span><span class="sxs-lookup"><span data-stu-id="2a1de-158">Specifies your SQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="2a1de-159">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2a1de-159">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="2a1de-160">封裝和測試應用程式範例</span><span class="sxs-lookup"><span data-stu-id="2a1de-160">Package and test the sample application</span></span> 

1. <span data-ttu-id="2a1de-161">使用 Maven 建置應用程式範例；例如：</span><span class="sxs-lookup"><span data-stu-id="2a1de-161">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P sql
   ```

1. <span data-ttu-id="2a1de-162">啟動應用程式範例；例如：</span><span class="sxs-lookup"><span data-stu-id="2a1de-162">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jpa-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="2a1de-163">從命令提示字元使用 `curl` 建立新的記錄，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="2a1de-163">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="2a1de-164">應用程式應該會傳回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="2a1de-164">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="2a1de-165">從命令提示字元使用 `curl` 擷取所有現有的記錄，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="2a1de-165">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="2a1de-166">應用程式應該會傳回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="2a1de-166">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="2a1de-167">總結</span><span class="sxs-lookup"><span data-stu-id="2a1de-167">Summary</span></span>

<span data-ttu-id="2a1de-168">在此教學課程中，您已建立使用 Spring Data 的範例 Java 應用程式，以在 Azure SQL 資料庫中使用 JPA 儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="2a1de-168">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure SQL database using JPA.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a1de-169">後續步驟</span><span class="sxs-lookup"><span data-stu-id="2a1de-169">Next steps</span></span>

<span data-ttu-id="2a1de-170">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="2a1de-170">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2a1de-171">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="2a1de-171">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="2a1de-172">其他資源</span><span class="sxs-lookup"><span data-stu-id="2a1de-172">Additional Resources</span></span>

<span data-ttu-id="2a1de-173">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="2a1de-173">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[SQL01]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-09.png
