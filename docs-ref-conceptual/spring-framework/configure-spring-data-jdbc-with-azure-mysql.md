---
title: 如何搭配使用 Spring Data JDBC 和 Azure MySQL
description: 了解如何搭配使用 Spring Data JDBC 和 Azure MySQL 資料庫。
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
ms.openlocfilehash: 5ec89194c72cef81c79d21382e41b33ebe91d3d0
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992124"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-mysql"></a>如何搭配使用 Spring Data JDBC 和 Azure MySQL

## <a name="overview"></a>概觀

本文示範如何建立使用 [Spring Data] 的應用程式範例，以在 Azure [MySQL](https://www.mysql.com/) 資料庫 中使用 [Java 資料庫連線 (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 儲存和擷取資訊。

## <a name="prerequisites"></a>必要條件

請務必具備下列必要條件，以便本文中說明的步驟：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。
* [Curl](https://curl.haxx.se/) 或類似的 HTTP 公用程式，以便測試功能。
* [mysql](https://dev.mysql.com/downloads/) 命令列公用程式。
* [Git](https://git-scm.com/downloads) 用戶端。

## <a name="create-a-mysql-database-for-azure"></a>建立 Azure MySQL 資料庫

### <a name="create-a-mysql-database-server-using-the-azure-portal"></a>使用 Azure 入口網站建立 MySQL 資料庫伺服器

> [!NOTE]
> 
> 您可以在[使用 Azure 入口網站建立適用於 MySQL 的 Azure 資料庫伺服器](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)中，閱讀更多關於如何建立 MySQL 資料庫的詳細資訊。

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [+建立資源]、[資料庫]，然後按一下 [適用於 MySQL 的 Azure 資料庫]。

   ![建立 MySQL 資料庫][MYSQL01]

1. 輸入以下資訊：

   - **伺服器名稱**：為 MySQL 伺服器選擇唯一的名稱；此名稱將用來建立完整網域名稱，例如 wingtiptoysmysql.mysql.database.azure.com。
   - 訂用帳戶：指定您要使用的 Azure 訂用帳戶。
   - **資源群組**：指定是要建立新的資源群組，還是選擇現有的資源群組。
   - **選取來源**：在此教學課程中，選取 `Blank` 以建立新的資料庫。
   - **伺服器管理員登入**：指定資料庫管理員的名稱。
   - **密碼**和**確認密碼**：指定資料庫管理員的密碼。
   - **位置**：指定與資料庫最為接近的地理區域。
   - **版本**：指定最新的資料庫版本。
   - **定價層**：在此教學課程中，指定價格最實惠的定價層。

   ![建立 MySQL 資料庫屬性][MYSQL02]

1. 上述所有資訊皆輸入完成時，按一下 [建立]。

### <a name="configure-a-firewall-rule-for-your-mysql-database-server-using-the-azure-portal"></a>使用 Azure 入口網站設定 MySQL 資料庫伺服器的防火牆規則

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [所有資源]，然後按一下您剛才建立的 MySQL 資料庫。

   ![選取 MySQL 資料庫][MYSQL03]

1. 按一下 [連線安全性]，然後在 [防火牆規則] 中，藉由指定規則的唯一名稱來建立新的規則，然後輸入需要存取資料庫的 IP 位址範圍，再按一下 [儲存]。

   ![設定連線安全性][MYSQL04]

### <a name="retrieve-the-connection-string-for-your-mysql-server-using-the-azure-portal"></a>使用 Azure 入口網站擷取 MySQL 伺服器的連接字串

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [所有資源]，然後按一下您剛才建立的 MySQL 資料庫。

   ![選取 MySQL 資料庫][MYSQL03]

1. 按一下 [連接字串]，然後複製 [JDBC] 文字欄位中的值。

   ![擷取 JDBC 連接字串][MYSQL05]

### <a name="create-mysql-database-using-the-mysql-command-line-utility"></a>使用 `mysql` 命令列公用程式建立 MySQL 資料庫

1. 開啟命令殼層，然後輸入 `mysql` 命令來連線至 MySQL 伺服器，如下列範例所示：

   ```shell
   mysql --host wingtiptoysmysql.mysql.database.azure.com --user wingtiptoysuser@wingtiptoysmysql -p
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `host` | 指定本文稍早所述的完整 MySQL 伺服器名稱。 |
   | `user` | 指定本文稍早所述的 MySQL 管理員和縮略的伺服器名稱。 |
   | `p` | 指定要等候系統提示您輸入密碼。 |

   MySQL 伺服器應該會回應如下列範例所示的顯示內容：

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

1. 輸入 `mysql` 命令來建立名為 mysqldb 的資料庫，如下列範例所示：

   ```SQL
   CREATE DATABASE mysqldb;
   ```

   MySQL 伺服器應該會回應如下列範例所示的顯示內容：

   ```shell
   Query OK, 1 row affected (0.30 sec)
   ```

1. 選擇性：您可以輸入 `mysql` 命令來確認資料庫是否已建立好，如下列範例所示：

   ```SQL
   SHOW DATABASES;
   ```

   MySQL 伺服器應該會回應如下列範例所示的顯示內容：

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

1. 輸入 `\q` 以結束 `mysql` 公用程式。

## <a name="configure-the-sample-application"></a>設定範例應用程式

1. 開啟命令殼層，並使用 git 命令複製範例專案，如下列範例所示：

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. 在範例專案的 [資源] 目錄中尋找 application.properties 檔案，如果該檔案不存在，則加以建立。

1. 在文字編輯器中開啟 application.properties 檔案、在檔案中新增或設定下列幾行，然後使用本文稍早的適當值來取代範例值：

   ```yaml
   spring.datasource.url=jdbc:mysql://wingtiptoysmysql.mysql.database.azure.com:3306/mysqldb?useSSL=true&requireSSL=false&serverTimezone=UTC
   spring.datasource.username=wingtiptoysuser@wingtiptoysmysql
   spring.datasource.password=********
    ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `spring.datasource.url` | 指定本文稍早所述的 MySQL JDBC 字串，並新增時區。 |
   | `spring.datasource.username` | 指定本文稍早所述的 MySQL 管理員名稱，並對其附加縮略的伺服器名稱。 |
   | `spring.datasource.password` | 指定本文稍早所述的 MySQL 管理員密碼。 |

1. 儲存並關閉 *application.properties* 檔案。

## <a name="package-and-test-the-sample-application"></a>封裝和測試應用程式範例 

1. 使用 Maven 建置應用程式範例；例如：

   ```shell
   mvn clean package -P mysql
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

在此教學課程中，您已建立使用 Spring Data 的範例 Java 應用程式，以在 Azure MySQL 資料庫中使用 JDBC 儲存和擷取資訊。

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

[MYSQL01]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png
[MYSQL02]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png
[MYSQL03]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-03.png
[MYSQL04]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-04.png
[MYSQL05]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-05.png
