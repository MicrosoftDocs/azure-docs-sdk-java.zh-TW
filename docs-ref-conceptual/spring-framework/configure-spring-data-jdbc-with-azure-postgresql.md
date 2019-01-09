---
title: 如何搭配使用 Spring Data JDBC 和 Azure PostgreSQL
description: 了解如何搭配使用 Spring Data JDBC 和 Azure PostgreSQL 資料庫。
services: postgresql
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 371a8c686f7ad045443328d02a32a4e65af55981
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992128"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-postgresql"></a><span data-ttu-id="c9685-103">如何搭配使用 Spring Data JDBC 和 Azure PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="c9685-103">How to use Spring Data JDBC with Azure PostgreSQL</span></span>

## <a name="overview"></a><span data-ttu-id="c9685-104">概觀</span><span class="sxs-lookup"><span data-stu-id="c9685-104">Overview</span></span>

<span data-ttu-id="c9685-105">本文示範如何建立使用 [Spring Data] 的應用程式範例，以在 Azure [PostgreSQL]https://www.postgresql.org/ 資料庫中使用 [Java 資料庫連線 (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="c9685-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an Azure [PostgreSQL]https://www.postgresql.org/ database using [Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9685-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="c9685-106">Prerequisites</span></span>

<span data-ttu-id="c9685-107">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="c9685-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="c9685-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="c9685-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="c9685-109">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="c9685-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="c9685-110">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="c9685-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="c9685-111">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="c9685-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="c9685-112">[Curl](https://curl.haxx.se/) 或類似的 HTTP 公用程式，以便測試功能。</span><span class="sxs-lookup"><span data-stu-id="c9685-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="c9685-113">[psql](https://www.postgresql.org/docs/current/app-psql.html) 命令列公用程式。</span><span class="sxs-lookup"><span data-stu-id="c9685-113">The [psql](https://www.postgresql.org/docs/current/app-psql.html) command-line utility.</span></span>
* <span data-ttu-id="c9685-114">[Git](https://git-scm.com/downloads) 用戶端。</span><span class="sxs-lookup"><span data-stu-id="c9685-114">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-a-postgresql-database-for-azure"></a><span data-ttu-id="c9685-115">建立 Azure PostgreSQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="c9685-115">Create a PostgreSQL database for Azure</span></span>

### <a name="create-a-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="c9685-116">使用 Azure 入口網站建立 PostgreSQL 資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="c9685-116">Create a PostgreSQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="c9685-117">您可以在[使用 Azure 入口網站建立適用於 PostgreSQL 的 Azure 資料庫伺服器](/azure/postgresql/quickstart-create-server-database-portal)中，閱讀更多關於如何建立 PostgreSQL 資料庫的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="c9685-117">You can read more detailed information about creating PostgreSQL databases in [Create an Azure Database for PostgreSQL server by using the Azure portal](/azure/postgresql/quickstart-create-server-database-portal).</span></span>

1. <span data-ttu-id="c9685-118">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="c9685-118">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="c9685-119">按一下 [+建立資源]、[資料庫]，然後按一下 [適用於 PostgreSQL 的 Azure 資料庫]。</span><span class="sxs-lookup"><span data-stu-id="c9685-119">Click **+Create a resource**, then **Databases**, and then click **Azure Database for PostgreSQL**.</span></span>

   ![建立 PostgreSQL 資料庫][POSTGRESQL01]

1. <span data-ttu-id="c9685-121">輸入以下資訊：</span><span class="sxs-lookup"><span data-stu-id="c9685-121">Enter the following information:</span></span>

   - <span data-ttu-id="c9685-122">**伺服器名稱**：為 PostgreSQL 伺服器選擇唯一的名稱；此名稱將用來建立完整網域名稱，例如 wingtiptoyspostgresql.postgres.database.azure.com。</span><span class="sxs-lookup"><span data-stu-id="c9685-122">**Server name**: Choose a unique name for your PostgreSQL server; this will be used to create a fully-qualified domain name like *wingtiptoyspostgresql.postgres.database.azure.com*.</span></span>
   - <span data-ttu-id="c9685-123">訂用帳戶：指定您要使用的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="c9685-123">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="c9685-124">**資源群組**：指定是要建立新的資源群組，還是選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="c9685-124">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="c9685-125">**選取來源**：在此教學課程中，選取 `Blank` 以建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c9685-125">**Select source**: For this tutorial, select `Blank` to create a new database.</span></span>
   - <span data-ttu-id="c9685-126">**伺服器管理員登入**：指定資料庫管理員的名稱。</span><span class="sxs-lookup"><span data-stu-id="c9685-126">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="c9685-127">**密碼**和**確認密碼**：指定資料庫管理員的密碼。</span><span class="sxs-lookup"><span data-stu-id="c9685-127">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="c9685-128">**位置**：指定與資料庫最為接近的地理區域。</span><span class="sxs-lookup"><span data-stu-id="c9685-128">**Location**: Specify the closest geographic region for your database.</span></span>
   - <span data-ttu-id="c9685-129">**版本**：指定最新的資料庫版本。</span><span class="sxs-lookup"><span data-stu-id="c9685-129">**Version**: Specify the most-up-to-date database version.</span></span>
   - <span data-ttu-id="c9685-130">**定價層**：在此教學課程中，指定價格最實惠的定價層。</span><span class="sxs-lookup"><span data-stu-id="c9685-130">**Pricing tier**: For this tutorial, specify the least-expensive pricing tier.</span></span>

   ![建立 PostgreSQL 資料庫屬性][POSTGRESQL02]

1. <span data-ttu-id="c9685-132">上述所有資訊皆輸入完成時，按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="c9685-132">When you have entered all of the above information, click **Create**.</span></span>

### <a name="configure-a-firewall-rule-for-your-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="c9685-133">使用 Azure 入口網站設定 PostgreSQL 資料庫伺服器的防火牆規則</span><span class="sxs-lookup"><span data-stu-id="c9685-133">Configure a firewall rule for your PostgreSQL database server using the Azure Portal</span></span>

1. <span data-ttu-id="c9685-134">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="c9685-134">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="c9685-135">按一下 [所有資源]，然後按一下您剛才建立的 PostgreSQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="c9685-135">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![選取 PostgreSQL 資料庫][POSTGRESQL03]

1. <span data-ttu-id="c9685-137">按一下 [連線安全性]，然後在 [防火牆規則] 中，藉由指定規則的唯一名稱來建立新的規則，然後輸入需要存取資料庫的 IP 位址範圍，再按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="c9685-137">Click **Connection security**, and in the **Firewall rules**, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![設定連線安全性][POSTGRESQL04]

### <a name="retrieve-the-connection-string-for-your-postgresql-server-using-the-azure-portal"></a><span data-ttu-id="c9685-139">使用 Azure 入口網站擷取 PostgreSQL 伺服器的連接字串</span><span class="sxs-lookup"><span data-stu-id="c9685-139">Retrieve the connection string for your PostgreSQL server using the Azure Portal</span></span>

1. <span data-ttu-id="c9685-140">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="c9685-140">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="c9685-141">按一下 [所有資源]，然後按一下您剛才建立的 PostgreSQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="c9685-141">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![選取 PostgreSQL 資料庫][POSTGRESQL03]

1. <span data-ttu-id="c9685-143">按一下 [連接字串]，然後複製 [JDBC] 文字欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="c9685-143">Click **Connection strings**, and copy the value in the **JDBC** text field.</span></span>

   ![擷取 JDBC 連接字串][POSTGRESQL05]

### <a name="create-postgresql-database-using-the-psql-command-line-utility"></a><span data-ttu-id="c9685-145">使用 `psql` 命令列公用程式建立 PostgreSQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="c9685-145">Create PostgreSQL database using the `psql` command-line utility</span></span>

1. <span data-ttu-id="c9685-146">開啟命令殼層，然後輸入 `psql` 命令來連線至 PostgreSQL 伺服器，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="c9685-146">Open a command shell and connect to your PostgreSQL server by entering a `psql` command like the following example:</span></span>

   ```shell
   psql --host=wingtiptoyspostgresql.postgres.database.azure.com --port=5432 --username=wingtiptoysuser@wingtiptoyspostgresql --dbname=postgres
   ```
   <span data-ttu-id="c9685-147">其中：</span><span class="sxs-lookup"><span data-stu-id="c9685-147">Where:</span></span>

   | <span data-ttu-id="c9685-148">參數</span><span class="sxs-lookup"><span data-stu-id="c9685-148">Parameter</span></span> | <span data-ttu-id="c9685-149">說明</span><span class="sxs-lookup"><span data-stu-id="c9685-149">Description</span></span> |
   |---|---|
   | `host` | <span data-ttu-id="c9685-150">指定本文稍早所述的完整 PostgreSQL 伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="c9685-150">Specifies your fully qualified PostgreSQL server name from earlier in this article.</span></span> |
   | `host` | <span data-ttu-id="c9685-151">指定 PostgreSQL 伺服器連接埠，預設值為 `5432`。</span><span class="sxs-lookup"><span data-stu-id="c9685-151">Specifies the PostgreSQL server port, which is `5432` by default.</span></span> |
   | `username` | <span data-ttu-id="c9685-152">指定本文稍早所述的 PostgreSQL 管理員和縮略的伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="c9685-152">Specifies your PostgreSQL administrator and shortened server name from earlier in this article.</span></span> |
   | `dbname` | <span data-ttu-id="c9685-153">指定您目前想要使用預設的 `postgres` 資料庫。</span><span class="sxs-lookup"><span data-stu-id="c9685-153">Specifies that you want to use the default `postgres` database for now.</span></span> |

   <span data-ttu-id="c9685-154">PostgreSQL 伺服器應該會回應如下列範例所示的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="c9685-154">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   psql (9.3.24, server 10.5)
   SSL connection (cipher: ECDHE-RSA-AES256-SHA384, bits: 256)
   Type "help" for help.
   
   postgres=>
   ```

1. <span data-ttu-id="c9685-155">輸入 `psql` 命令來建立名為 mypgsqldb 的資料庫，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="c9685-155">Create a database named *mypgsqldb* by entering a `psql` command like the following example:</span></span>

   ```SQL
   CREATE DATABASE mypgsqldb;
   ```

   <span data-ttu-id="c9685-156">PostgreSQL 伺服器應該會回應如下列範例所示的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="c9685-156">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   CREATE DATABASE
   ```

1. <span data-ttu-id="c9685-157">選擇性：您可以在 `psql` 輸入 `\l` 來確認資料庫是否已建立好；PostgreSQL 伺服器應該會有類似下列範例的回應：</span><span class="sxs-lookup"><span data-stu-id="c9685-157">OPTIONAL: You can verify that your database was created by entering a `\l` at the `psql`; your PostgreSQL server should respond with something like the following example:</span></span>

   ```shell
                   List of databases
          Name        |      Owner      | Encoding
   -------------------+-----------------+----------
    azure_maintenance | azure_superuser | UTF8
    azure_sys         | azure_superuser | UTF8
    mypgsqldb         | wingtiptoysuser | UTF8
    postgres          | azure_superuser | UTF8
    template0         | azure_superuser | UTF8
    template1         | azure_superuser | UTF8
   (6 rows)
   ```

1. <span data-ttu-id="c9685-158">輸入 `\q` 以結束 `psql` 公用程式。</span><span class="sxs-lookup"><span data-stu-id="c9685-158">Enter `\q` to exit the `psql` utility.</span></span>

## <a name="configure-the-sample-application"></a><span data-ttu-id="c9685-159">設定範例應用程式</span><span class="sxs-lookup"><span data-stu-id="c9685-159">Configure the sample application</span></span>

1. <span data-ttu-id="c9685-160">開啟命令殼層，並使用 git 命令複製範例專案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="c9685-160">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. <span data-ttu-id="c9685-161">在範例專案的 [資源] 目錄中尋找 application.properties 檔案，如果該檔案不存在，則加以建立。</span><span class="sxs-lookup"><span data-stu-id="c9685-161">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="c9685-162">在文字編輯器中開啟 application.properties 檔案、在檔案中新增或設定下列幾行，然後使用本文稍早的適當值來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="c9685-162">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:postgresql://wingtiptoyspostgresql.postgres.database.azure.com:5432/mypgsqldb?ssl=true&sslmode=prefer
   spring.datasource.username=wingtiptoysuser@wingtiptoyspostgresql
   spring.datasource.password=********
    ```
   <span data-ttu-id="c9685-163">其中：</span><span class="sxs-lookup"><span data-stu-id="c9685-163">Where:</span></span>

   | <span data-ttu-id="c9685-164">參數</span><span class="sxs-lookup"><span data-stu-id="c9685-164">Parameter</span></span> | <span data-ttu-id="c9685-165">說明</span><span class="sxs-lookup"><span data-stu-id="c9685-165">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="c9685-166">指定本文稍早所述的 PostgreSQL JDBC 字串。</span><span class="sxs-lookup"><span data-stu-id="c9685-166">Specifies your PostgreSQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="c9685-167">指定本文稍早所述的 PostgreSQL 管理員名稱，並對其附加縮略的伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="c9685-167">Specifies your PostgreSQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="c9685-168">指定本文稍早所述的 PostgreSQL 管理員密碼。</span><span class="sxs-lookup"><span data-stu-id="c9685-168">Specifies your PostgreSQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="c9685-169">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c9685-169">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="c9685-170">封裝和測試應用程式範例</span><span class="sxs-lookup"><span data-stu-id="c9685-170">Package and test the sample application</span></span> 

1. <span data-ttu-id="c9685-171">使用 Maven 建置應用程式範例；例如：</span><span class="sxs-lookup"><span data-stu-id="c9685-171">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P postgresql
   ```

1. <span data-ttu-id="c9685-172">啟動應用程式範例；例如：</span><span class="sxs-lookup"><span data-stu-id="c9685-172">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="c9685-173">從命令提示字元使用 `curl` 建立新的記錄，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="c9685-173">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="c9685-174">應用程式應該會傳回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="c9685-174">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="c9685-175">從命令提示字元使用 `curl` 擷取所有現有的記錄，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="c9685-175">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="c9685-176">應用程式應該會傳回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="c9685-176">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="c9685-177">總結</span><span class="sxs-lookup"><span data-stu-id="c9685-177">Summary</span></span>

<span data-ttu-id="c9685-178">在此教學課程中，您已建立使用 Spring Data 的範例 Java 應用程式，以在 Azure PostgreSQL 資料庫中使用 JDBC 儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="c9685-178">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure PostgreSQL database using JDBC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9685-179">後續步驟</span><span class="sxs-lookup"><span data-stu-id="c9685-179">Next steps</span></span>

<span data-ttu-id="c9685-180">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="c9685-180">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c9685-181">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="c9685-181">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="c9685-182">其他資源</span><span class="sxs-lookup"><span data-stu-id="c9685-182">Additional Resources</span></span>

<span data-ttu-id="c9685-183">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="c9685-183">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[POSTGRESQL01]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-01.png
[POSTGRESQL02]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-02.png
[POSTGRESQL03]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-03.png
[POSTGRESQL04]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-04.png
[POSTGRESQL05]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-05.png
