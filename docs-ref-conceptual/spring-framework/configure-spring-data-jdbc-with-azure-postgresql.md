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
# <a name="how-to-use-spring-data-jdbc-with-azure-postgresql"></a>如何搭配使用 Spring Data JDBC 和 Azure PostgreSQL

## <a name="overview"></a>概觀

本文示範如何建立使用 [Spring Data] 的應用程式範例，以在 Azure [PostgreSQL]https://www.postgresql.org/ 資料庫中使用 [Java 資料庫連線 (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 儲存和擷取資訊。

## <a name="prerequisites"></a>必要條件

請務必具備下列必要條件，以便本文中說明的步驟：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。
* [Curl](https://curl.haxx.se/) 或類似的 HTTP 公用程式，以便測試功能。
* [psql](https://www.postgresql.org/docs/current/app-psql.html) 命令列公用程式。
* [Git](https://git-scm.com/downloads) 用戶端。

## <a name="create-a-postgresql-database-for-azure"></a>建立 Azure PostgreSQL 資料庫

### <a name="create-a-postgresql-database-server-using-the-azure-portal"></a>使用 Azure 入口網站建立 PostgreSQL 資料庫伺服器

> [!NOTE]
> 
> 您可以在[使用 Azure 入口網站建立適用於 PostgreSQL 的 Azure 資料庫伺服器](/azure/postgresql/quickstart-create-server-database-portal)中，閱讀更多關於如何建立 PostgreSQL 資料庫的詳細資訊。

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [+建立資源]、[資料庫]，然後按一下 [適用於 PostgreSQL 的 Azure 資料庫]。

   ![建立 PostgreSQL 資料庫][POSTGRESQL01]

1. 輸入以下資訊：

   - **伺服器名稱**：為 PostgreSQL 伺服器選擇唯一的名稱；此名稱將用來建立完整網域名稱，例如 wingtiptoyspostgresql.postgres.database.azure.com。
   - 訂用帳戶：指定您要使用的 Azure 訂用帳戶。
   - **資源群組**：指定是要建立新的資源群組，還是選擇現有的資源群組。
   - **選取來源**：在此教學課程中，選取 `Blank` 以建立新的資料庫。
   - **伺服器管理員登入**：指定資料庫管理員的名稱。
   - **密碼**和**確認密碼**：指定資料庫管理員的密碼。
   - **位置**：指定與資料庫最為接近的地理區域。
   - **版本**：指定最新的資料庫版本。
   - **定價層**：在此教學課程中，指定價格最實惠的定價層。

   ![建立 PostgreSQL 資料庫屬性][POSTGRESQL02]

1. 上述所有資訊皆輸入完成時，按一下 [建立]。

### <a name="configure-a-firewall-rule-for-your-postgresql-database-server-using-the-azure-portal"></a>使用 Azure 入口網站設定 PostgreSQL 資料庫伺服器的防火牆規則

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [所有資源]，然後按一下您剛才建立的 PostgreSQL 資料庫。

   ![選取 PostgreSQL 資料庫][POSTGRESQL03]

1. 按一下 [連線安全性]，然後在 [防火牆規則] 中，藉由指定規則的唯一名稱來建立新的規則，然後輸入需要存取資料庫的 IP 位址範圍，再按一下 [儲存]。

   ![設定連線安全性][POSTGRESQL04]

### <a name="retrieve-the-connection-string-for-your-postgresql-server-using-the-azure-portal"></a>使用 Azure 入口網站擷取 PostgreSQL 伺服器的連接字串

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [所有資源]，然後按一下您剛才建立的 PostgreSQL 資料庫。

   ![選取 PostgreSQL 資料庫][POSTGRESQL03]

1. 按一下 [連接字串]，然後複製 [JDBC] 文字欄位中的值。

   ![擷取 JDBC 連接字串][POSTGRESQL05]

### <a name="create-postgresql-database-using-the-psql-command-line-utility"></a>使用 `psql` 命令列公用程式建立 PostgreSQL 資料庫

1. 開啟命令殼層，然後輸入 `psql` 命令來連線至 PostgreSQL 伺服器，如下列範例所示：

   ```shell
   psql --host=wingtiptoyspostgresql.postgres.database.azure.com --port=5432 --username=wingtiptoysuser@wingtiptoyspostgresql --dbname=postgres
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `host` | 指定本文稍早所述的完整 PostgreSQL 伺服器名稱。 |
   | `host` | 指定 PostgreSQL 伺服器連接埠，預設值為 `5432`。 |
   | `username` | 指定本文稍早所述的 PostgreSQL 管理員和縮略的伺服器名稱。 |
   | `dbname` | 指定您目前想要使用預設的 `postgres` 資料庫。 |

   PostgreSQL 伺服器應該會回應如下列範例所示的顯示內容：

   ```shell
   psql (9.3.24, server 10.5)
   SSL connection (cipher: ECDHE-RSA-AES256-SHA384, bits: 256)
   Type "help" for help.
   
   postgres=>
   ```

1. 輸入 `psql` 命令來建立名為 mypgsqldb 的資料庫，如下列範例所示：

   ```SQL
   CREATE DATABASE mypgsqldb;
   ```

   PostgreSQL 伺服器應該會回應如下列範例所示的顯示內容：

   ```shell
   CREATE DATABASE
   ```

1. 選擇性：您可以在 `psql` 輸入 `\l` 來確認資料庫是否已建立好；PostgreSQL 伺服器應該會有類似下列範例的回應：

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

1. 輸入 `\q` 以結束 `psql` 公用程式。

## <a name="configure-the-sample-application"></a>設定範例應用程式

1. 開啟命令殼層，並使用 git 命令複製範例專案，如下列範例所示：

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. 在範例專案的 [資源] 目錄中尋找 application.properties 檔案，如果該檔案不存在，則加以建立。

1. 在文字編輯器中開啟 application.properties 檔案、在檔案中新增或設定下列幾行，然後使用本文稍早的適當值來取代範例值：

   ```yaml
   spring.datasource.url=jdbc:postgresql://wingtiptoyspostgresql.postgres.database.azure.com:5432/mypgsqldb?ssl=true&sslmode=prefer
   spring.datasource.username=wingtiptoysuser@wingtiptoyspostgresql
   spring.datasource.password=********
    ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `spring.datasource.url` | 指定本文稍早所述的 PostgreSQL JDBC 字串。 |
   | `spring.datasource.username` | 指定本文稍早所述的 PostgreSQL 管理員名稱，並對其附加縮略的伺服器名稱。 |
   | `spring.datasource.password` | 指定本文稍早所述的 PostgreSQL 管理員密碼。 |

1. 儲存並關閉 *application.properties* 檔案。

## <a name="package-and-test-the-sample-application"></a>封裝和測試應用程式範例 

1. 使用 Maven 建置應用程式範例；例如：

   ```shell
   mvn clean package -P postgresql
   ```

1. 啟動應用程式範例；例如：

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 從命令提示字元使用 `curl` 建立新的記錄，如下列範例所示：

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   應用程式應該會傳回如下所示的值：

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. 從命令提示字元使用 `curl` 擷取所有現有的記錄，如下列範例所示：

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   應用程式應該會傳回如下所示的值：

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>總結

在此教學課程中，您已建立使用 Spring Data 的範例 Java 應用程式，以在 Azure PostgreSQL 資料庫中使用 JDBC 儲存和擷取資訊。

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他資源

如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。

<!-- URL List -->

[適用於 Java 開發人員的 Azure]: /java/azure/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
