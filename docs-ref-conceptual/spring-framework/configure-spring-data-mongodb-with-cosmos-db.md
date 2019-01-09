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
# <a name="how-to-use-spring-data-mongodb-api-with-azure-cosmos-db"></a>如何搭配使用 Spring Data MongoDB API 和 Azure Cosmos DB

## <a name="overview"></a>概觀

本文示範如何建立使用 [Spring Data] 的應用程式範例，以使用 [Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction) 儲存和擷取資訊。

## <a name="prerequisites"></a>必要條件

請務必具備下列必要條件，以便本文中說明的步驟：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。
* [Git](https://git-scm.com/downloads) 用戶端。

## <a name="create-an-azure-cosmos-db-account"></a>建立 Azure Cosmos DB 帳戶

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a>使用 Azure 入口網站建立 Cosmos DB 帳戶

> [!NOTE]
> 
> 您可以在 [Azure Cosmos DB 文件](/azure/cosmos-db/)中閱讀更多有關如何建立 Azure Cosmos DB 帳戶的詳細資訊。

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [+建立資源]、[資料庫]，然後按一下 [Azure Cosmos DB]。

   ![建立 Azure Cosmos DB 帳戶][COSMOSDB01]

1. 指定下列資訊：

   - 訂用帳戶：指定您要使用的 Azure 訂用帳戶。
   - **資源群組**：指定是要建立新的資源群組，還是選擇現有的資源群組。
   - **帳戶名稱**：為 Cosmos DB 帳戶選擇唯一的名稱；此名稱將用來建立完整網域名稱，例如 wingtiptoysmongodb.documents.azure.com。
   - **API**：在此教學課程中指定 `Azure Cosmos DB for MongoDB API`。
   - **位置**：指定與資料庫最為接近的地理區域。

   ![指定 Cosmos DB 帳戶的設定][COSMOSDB02]
   
1. 上述所有資訊皆輸入完成時，按一下 [檢閱 + 建立]。

1. 如果 [檢閱] 頁面上的所有資訊皆正確，按一下 [建立]。

   ![檢閱 Cosmos DB 帳戶的設定][COSMOSDB03]

### <a name="retrieve-the-connection-string-for-your-azure-cosmos-db-account"></a>擷取 Azure Cosmos DB 帳戶的連接字串

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [所有資源]，然後按一下您剛才建立的 Azure Cosmos DB 帳戶。

   ![選取 Azure Cosmos DB 帳戶][COSMOSDB04]

1. 按一下 [連接字串]，然後複製 [主要連接字串] 欄位的值；您之後會使用該值來設定應用程式。

   ![擷取 Cosmos DB 的連接字串][COSMOSDB06]

## <a name="configure-the-sample-application"></a>設定範例應用程式

1. 開啟命令殼層，並使用 git 命令複製範例專案，如下列範例所示：

   ```shell
   git clone https://github.com/spring-guides/gs-accessing-data-mongodb.git
   ```

1. 在範例專案的 &lt;專案根目錄&gt;/complete/src/main 中建立 resources 目錄，然後在 resources 目錄中建立 application.properties 檔案。

1. 在文字編輯器中開啟 application.properties 檔案、在檔案中新增下列幾行，然後使用本文稍早的適當值來取代範例值：

   ```yaml
   spring.data.mongodb.database=wingtiptoysmongodb
   spring.data.mongodb.uri=mongodb://wingtiptoysmongodb:AbCdEfGhIjKlMnOpQrStUvWxYz==@wingtiptoysmongodb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `spring.data.mongodb.database` | 指定本文稍早所述 Cosmos DB 帳戶的名稱。 |
   | `spring.data.mongodb.uri` | 指定本文稍早所述的**主要連接字串**。 |

1. 儲存並關閉 *application.properties* 檔案。

## <a name="package-and-test-the-sample-application"></a>封裝和測試應用程式範例 

1. 使用 Maven 建置應用程式範例，然後設定 Maven 來略過測試；例如：

   ```shell
   mvn clean package -DskipTests
   ```

1. 啟動應用程式範例；例如：

   ```shell
   java -jar target/gs-accessing-data-mongodb-0.1.0.jar
   ```
    
   應用程式應該會傳回如下所示的值：

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

## <a name="summary"></a>總結

在此教學課程中，您已建立使用 Spring Data 的範例 Java 應用程式，以使用 Azure Cosmos DB MongoDB API 來儲存和擷取資訊。

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

[COSMOSDB01]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB06]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-06.png
