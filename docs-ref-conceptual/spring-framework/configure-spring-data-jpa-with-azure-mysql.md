---
title: 如何搭配使用 Spring Data JPA 和 Azure MySQL
description: 了解如何搭配使用 Spring Data JPA 和 Azure MySQL 資料庫。
services: mysql
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 95dfc292d8f6d7e03d396afd7da4cfff0bd05b1b
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992164"
---
# <a name="how-to-use-spring-data-jpa-with-azure-mysql"></a><span data-ttu-id="0ebea-103">如何搭配使用 Spring Data JPA 和 Azure MySQL</span><span class="sxs-lookup"><span data-stu-id="0ebea-103">How to use Spring Data JPA with Azure MySQL</span></span>

## <a name="overview"></a><span data-ttu-id="0ebea-104">概觀</span><span class="sxs-lookup"><span data-stu-id="0ebea-104">Overview</span></span>

<span data-ttu-id="0ebea-105">本文示範如何建立使用 [Spring Data] 的應用程式範例，以在 Azure [MySQL](https://www.mysql.com/) 資料庫中使用 [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) 儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="0ebea-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an Azure [MySQL](https://www.mysql.com/) database using [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ebea-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="0ebea-106">Prerequisites</span></span>

<span data-ttu-id="0ebea-107">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="0ebea-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="0ebea-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="0ebea-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="0ebea-109">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="0ebea-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="0ebea-110">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="0ebea-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="0ebea-111">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="0ebea-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="0ebea-112">[Curl](https://curl.haxx.se/) 或類似的 HTTP 公用程式，以便測試功能。</span><span class="sxs-lookup"><span data-stu-id="0ebea-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="0ebea-113">[mysql](https://dev.mysql.com/downloads/) 命令列公用程式。</span><span class="sxs-lookup"><span data-stu-id="0ebea-113">The [mysql](https://dev.mysql.com/downloads/) command-line utility.</span></span>
* <span data-ttu-id="0ebea-114">[Git](https://git-scm.com/downloads) 用戶端。</span><span class="sxs-lookup"><span data-stu-id="0ebea-114">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-a-mysql-database-for-azure"></a><span data-ttu-id="0ebea-115">建立 Azure MySQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="0ebea-115">Create a MySQL database for Azure</span></span>

### <a name="create-a-mysql-database-server-using-the-azure-portal"></a><span data-ttu-id="0ebea-116">使用 Azure 入口網站建立 MySQL 資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="0ebea-116">Create a MySQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="0ebea-117">您可以在[使用 Azure 入口網站建立適用於 MySQL 的 Azure 資料庫伺服器](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)中，閱讀更多關於如何建立 MySQL 資料庫的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="0ebea-117">You can read more detailed information about creating MySQL databases in [Create an Azure Database for MySQL server by using the Azure portal](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).</span></span>

1. <span data-ttu-id="0ebea-118">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="0ebea-118">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="0ebea-119">按一下 [+建立資源]、[資料庫]，然後按一下 [適用於 MySQL 的 Azure 資料庫]。</span><span class="sxs-lookup"><span data-stu-id="0ebea-119">Click **+Create a resource**, then **Databases**, and then click **Azure Database for MySQL**.</span></span>

   ![建立 MySQL 資料庫][MYSQL01]

1. <span data-ttu-id="0ebea-121">輸入以下資訊：</span><span class="sxs-lookup"><span data-stu-id="0ebea-121">Enter the following information:</span></span>

   - <span data-ttu-id="0ebea-122">**伺服器名稱**：為 MySQL 伺服器選擇唯一的名稱；此名稱將用來建立完整網域名稱，例如 wingtiptoysmysql.mysql.database.azure.com。</span><span class="sxs-lookup"><span data-stu-id="0ebea-122">**Server name**: Choose a unique name for your MySQL server; this will be used to create a fully-qualified domain name like *wingtiptoysmysql.mysql.database.azure.com*.</span></span>
   - <span data-ttu-id="0ebea-123">訂用帳戶：指定您要使用的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="0ebea-123">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="0ebea-124">**資源群組**：指定是要建立新的資源群組，還是選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="0ebea-124">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="0ebea-125">**選取來源**：在此教學課程中，選取 `Blank` 以建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="0ebea-125">**Select source**: For this tutorial, select `Blank` to create a new database.</span></span>
   - <span data-ttu-id="0ebea-126">**伺服器管理員登入**：指定資料庫管理員的名稱。</span><span class="sxs-lookup"><span data-stu-id="0ebea-126">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="0ebea-127">**密碼**和**確認密碼**：指定資料庫管理員的密碼。</span><span class="sxs-lookup"><span data-stu-id="0ebea-127">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="0ebea-128">**位置**：指定與資料庫最為接近的地理區域。</span><span class="sxs-lookup"><span data-stu-id="0ebea-128">**Location**: Specify the closest geographic region for your database.</span></span>
   - <span data-ttu-id="0ebea-129">**版本**：指定最新的資料庫版本。</span><span class="sxs-lookup"><span data-stu-id="0ebea-129">**Version**: Specify the most-up-to-date database version.</span></span>
   - <span data-ttu-id="0ebea-130">**定價層**：在此教學課程中，指定價格最實惠的定價層。</span><span class="sxs-lookup"><span data-stu-id="0ebea-130">**Pricing tier**: For this tutorial, specify the least-expensive pricing tier.</span></span>

   ![建立 MySQL 資料庫屬性][MYSQL02]

1. <span data-ttu-id="0ebea-132">上述所有資訊皆輸入完成時，按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="0ebea-132">When you have entered all of the above information, click **Create**.</span></span>

### <a name="configure-a-firewall-rule-for-your-mysql-database-server-using-the-azure-portal"></a><span data-ttu-id="0ebea-133">使用 Azure 入口網站設定 MySQL 資料庫伺服器的防火牆規則</span><span class="sxs-lookup"><span data-stu-id="0ebea-133">Configure a firewall rule for your MySQL database server using the Azure Portal</span></span>

1. <span data-ttu-id="0ebea-134">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="0ebea-134">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="0ebea-135">按一下 [所有資源]，然後按一下您剛才建立的 MySQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="0ebea-135">Click **All Resources**, then click the MySQL database you just created.</span></span>

   ![選取 MySQL 資料庫][MYSQL03]

1. <span data-ttu-id="0ebea-137">按一下 [連線安全性]，然後在 [防火牆規則] 中，藉由指定規則的唯一名稱來建立新的規則，然後輸入需要存取資料庫的 IP 位址範圍，再按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="0ebea-137">Click **Connection security**, and in the **Firewall rules**, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![設定連線安全性][MYSQL04]

### <a name="retrieve-the-connection-string-for-your-mysql-server-using-the-azure-portal"></a><span data-ttu-id="0ebea-139">使用 Azure 入口網站擷取 MySQL 伺服器的連接字串</span><span class="sxs-lookup"><span data-stu-id="0ebea-139">Retrieve the connection string for your MySQL server using the Azure Portal</span></span>

1. <span data-ttu-id="0ebea-140">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="0ebea-140">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="0ebea-141">按一下 [所有資源]，然後按一下您剛才建立的 MySQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="0ebea-141">Click **All Resources**, then click the MySQL database you just created.</span></span>

   ![選取 MySQL 資料庫][MYSQL03]

1. <span data-ttu-id="0ebea-143">按一下 [連接字串]，然後複製 [JDBC] 文字欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="0ebea-143">Click **Connection strings**, and copy the value in the **JDBC** text field.</span></span>

   ![擷取 JDBC 連接字串][MYSQL05]

### <a name="create-mysql-database-using-the-mysql-command-line-utility"></a><span data-ttu-id="0ebea-145">使用 `mysql` 命令列公用程式建立 MySQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="0ebea-145">Create MySQL database using the `mysql` command-line utility</span></span>

1. <span data-ttu-id="0ebea-146">開啟命令殼層，然後輸入 `mysql` 命令來連線至 MySQL 伺服器，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0ebea-146">Open a command shell and connect to your MySQL server by entering a `mysql` command like the following example:</span></span>

   ```shell
   mysql --host wingtiptoysmysql.mysql.database.azure.com --user wingtiptoysuser@wingtiptoysmysql -p
   ```
   <span data-ttu-id="0ebea-147">其中：</span><span class="sxs-lookup"><span data-stu-id="0ebea-147">Where:</span></span>

   | <span data-ttu-id="0ebea-148">參數</span><span class="sxs-lookup"><span data-stu-id="0ebea-148">Parameter</span></span> | <span data-ttu-id="0ebea-149">說明</span><span class="sxs-lookup"><span data-stu-id="0ebea-149">Description</span></span> |
   |---|---|
   | `host` | <span data-ttu-id="0ebea-150">指定本文稍早所述的完整 MySQL 伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="0ebea-150">Specifies your fully qualified MySQL server name from earlier in this article.</span></span> |
   | `user` | <span data-ttu-id="0ebea-151">指定本文稍早所述的 MySQL 管理員和縮略的伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="0ebea-151">Specifies your MySQL administrator and shortened server name from earlier in this article.</span></span> |
   | `p` | <span data-ttu-id="0ebea-152">指定要等候系統提示您輸入密碼。</span><span class="sxs-lookup"><span data-stu-id="0ebea-152">Specifies to wait until prompted for a password.</span></span> |


   <span data-ttu-id="0ebea-153">MySQL 伺服器應該會回應如下列範例所示的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="0ebea-153">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 64552
   Server version: 5.6.39.0 MySQL Community Server (GPL)
   
   Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```

1. <span data-ttu-id="0ebea-154">輸入 `mysql` 命令來建立名為 mysqldb 的資料庫，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0ebea-154">Create a database named *mysqldb* by entering a `mysql` command like the following example:</span></span>

   ```SQL
   CREATE DATABASE mysqldb;
   ```

   <span data-ttu-id="0ebea-155">MySQL 伺服器應該會回應如下列範例所示的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="0ebea-155">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   Query OK, 1 row affected (0.30 sec)
   ```

1. <span data-ttu-id="0ebea-156">選擇性：您可以輸入 `mysql` 命令來確認資料庫是否已建立好，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0ebea-156">OPTIONAL: You can verify that your database was created by entering a `mysql` command like the following example:</span></span>

   ```SQL
   SHOW DATABASES;
   ```

   <span data-ttu-id="0ebea-157">MySQL 伺服器應該會回應如下列範例所示的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="0ebea-157">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | mysqldb            |
   | performance_schema |
   | sys                |
   +--------------------+
   ```

1. <span data-ttu-id="0ebea-158">輸入 `\q` 以結束 `mysql` 公用程式。</span><span class="sxs-lookup"><span data-stu-id="0ebea-158">Enter `\q` to exit the `mysql` utility.</span></span>

## <a name="configure-the-sample-application"></a><span data-ttu-id="0ebea-159">設定範例應用程式</span><span class="sxs-lookup"><span data-stu-id="0ebea-159">Configure the sample application</span></span>

1. <span data-ttu-id="0ebea-160">開啟命令殼層，並使用 git 命令複製範例專案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0ebea-160">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jpa-on-azure.git
   ```

1. <span data-ttu-id="0ebea-161">在範例專案的 [資源] 目錄中尋找 application.properties 檔案，如果該檔案不存在，則加以建立。</span><span class="sxs-lookup"><span data-stu-id="0ebea-161">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="0ebea-162">在文字編輯器中開啟 application.properties 檔案、在檔案中新增或設定下列幾行，然後使用本文稍早的適當值來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="0ebea-162">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
   spring.datasource.url=jdbc:mysql://wingtiptoysmysql.mysql.database.azure.com:3306/mysqldb?useSSL=true&requireSSL=false
   spring.datasource.username=wingtiptoysuser@wingtiptoysmysql
   spring.datasource.password=********
    ```
   <span data-ttu-id="0ebea-163">其中：</span><span class="sxs-lookup"><span data-stu-id="0ebea-163">Where:</span></span>

   | <span data-ttu-id="0ebea-164">參數</span><span class="sxs-lookup"><span data-stu-id="0ebea-164">Parameter</span></span> | <span data-ttu-id="0ebea-165">說明</span><span class="sxs-lookup"><span data-stu-id="0ebea-165">Description</span></span> |
   |---|---|
   | `spring.jpa.database-platform` | <span data-ttu-id="0ebea-166">指定 JPA 資料庫平台。</span><span class="sxs-lookup"><span data-stu-id="0ebea-166">Specifies the JPA database platform.</span></span> |
   | `spring.datasource.url` | <span data-ttu-id="0ebea-167">指定本文稍早所述的 MySQL JDBC 字串。</span><span class="sxs-lookup"><span data-stu-id="0ebea-167">Specifies your MySQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="0ebea-168">指定本文稍早所述的 MySQL 管理員名稱，並對其附加縮略的伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="0ebea-168">Specifies your MySQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="0ebea-169">指定本文稍早所述的 MySQL 管理員密碼。</span><span class="sxs-lookup"><span data-stu-id="0ebea-169">Specifies your MySQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="0ebea-170">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0ebea-170">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="0ebea-171">封裝和測試應用程式範例</span><span class="sxs-lookup"><span data-stu-id="0ebea-171">Package and test the sample application</span></span> 

1. <span data-ttu-id="0ebea-172">使用 Maven 建置應用程式範例；例如：</span><span class="sxs-lookup"><span data-stu-id="0ebea-172">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P mysql
   ```

1. <span data-ttu-id="0ebea-173">啟動應用程式範例；例如：</span><span class="sxs-lookup"><span data-stu-id="0ebea-173">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jpa-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="0ebea-174">從命令提示字元使用 `curl` 建立新的記錄，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0ebea-174">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="0ebea-175">應用程式應該會傳回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="0ebea-175">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="0ebea-176">從命令提示字元使用 `curl` 擷取所有現有的記錄，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0ebea-176">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="0ebea-177">應用程式應該會傳回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="0ebea-177">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="0ebea-178">總結</span><span class="sxs-lookup"><span data-stu-id="0ebea-178">Summary</span></span>

<span data-ttu-id="0ebea-179">在此教學課程中，您已建立使用 Spring Data 的範例 Java 應用程式，以在 Azure MySQL 資料庫中使用 JPA 儲存和擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="0ebea-179">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure MySQL database using JPA.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ebea-180">後續步驟</span><span class="sxs-lookup"><span data-stu-id="0ebea-180">Next steps</span></span>

<span data-ttu-id="0ebea-181">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="0ebea-181">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0ebea-182">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="0ebea-182">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="0ebea-183">其他資源</span><span class="sxs-lookup"><span data-stu-id="0ebea-183">Additional Resources</span></span>

<span data-ttu-id="0ebea-184">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="0ebea-184">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[MYSQL01]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-01.png
[MYSQL02]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-02.png
[MYSQL03]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-03.png
[MYSQL04]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-04.png
[MYSQL05]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-05.png
