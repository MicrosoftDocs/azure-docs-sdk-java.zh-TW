---
title: 如何搭配 Azure Cosmos DB SQL API 使用 Spring Boot Starter
description: 了解如何設定搭配 Azure Cosmos DB SQL API 使用 Spring Boot Initializer 建立的應用程式。
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
ms.workload: data-services
ms.openlocfilehash: f00afbdd09ce617f863ed758f4bdddcb40701e27
ms.sourcegitcommit: 5bbf64121a99019207ed8cca29280fc5183c7314
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2019
ms.locfileid: "66840841"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>如何搭配 Azure Cosmos DB SQL API 使用 Spring Boot Starter

## <a name="overview"></a>概觀

Azure Cosmos DB 是一個橫跨全球的分散式資料庫服務，讓開發人員能夠利用各種不同的標準 API (例如 SQL、MongoDB、圖形和資料表 API) 來使用資料。 Microsoft 的 Spring Boot Starter 讓開發人員能夠使用 Spring Boot 應用程式，藉由使用 SQL API 來輕鬆地與 Azure Cosmos DB 整合。

本文將示範如何使用 Azure 入口網站建立 Azure Cosmos DB，接著使用 **[Spring Initializr]** 建立自訂的 Spring Boot 應用程式，然後將 [適用於 Azure 的 Spring Boot Cosmos DB Starter] 新增到您的自訂應用程式，以使用 Spring Data 和 Cosmos DB SQL API 將資料儲存在您的 Azure Cosmos DB 並從中擷取資料。

## <a name="prerequisites"></a>必要條件

若要遵循本文中的步驟，需要具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>使用 Azure 入口網站建立 Azure Cosmos DB

1. 瀏覽至 Azure 入口網站 <https://portal.azure.com/> ，然後按一下 **+ 新增資源** 。

   ![Azure 入口網站][AZ01]

1. 依序按一下 [資料庫]  和 [Azure Cosmos DB]  。

   ![Azure 入口網站][AZ02]

1. 在 [Azure Cosmos DB]  頁面上，輸入下列資訊：

   * 選取您想要用於資料庫的**訂用帳戶**。
   * 指定是否要為資料庫建立新的**資源群組**，或選擇現有的資源群組。
   * 輸入唯一的**帳戶名稱**，您將使用此名稱作為資料庫的 URI。 例如：*wingtiptoysdata*。
   * 為 API 選擇**核心 (SQL)** 。
   * 指定資料庫的**位置**。

   您指定這些選項之後，按一下 [檢閱+建立]  以建立您的資料庫。

   ![Azure 入口網站][AZ03]

1. 當您的資料庫建立之後，它會列在您的 Azure [儀表板]  中，以及 [所有資源]  和 [Azure Cosmos DB]  頁面下方。 您可以按一下這些任何位置上的資料庫，來開啟快取的屬性頁面。

   ![Azure 入口網站][AZ04]

1. 當資料庫的屬性頁面顯示時，按一下 [金鑰]  ，並複製資料庫的 URI 和存取金鑰；您將會在 Spring Boot 應用程式中使用這些值。

   ![Azure 入口網站][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 建立簡單的 Spring Boot 應用程式

1. 瀏覽至 <https://start.spring.io/> 。

1. 指定您想要使用 **Java** 產生 **Maven 專案**指定您的 **Spring Boot** 版本、輸入應用程式的**群組**和**成品**名稱、在相依性中新增 **Azure 支援**，然後按一下按鈕以**產生專案**。

   ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：*com.example.wintiptoys*。
   >

1. 出現提示時，將專案下載至本機電腦上的路徑，並將檔案解壓縮。

   ![將自訂的 Spring Boot 專案解壓縮][SI02]

1. 當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。

   ![自訂的 Spring Boot 專案檔][SI03]

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a>設定 Spring Boot 應用程式以使用 Azure Spring Boot Starter

1. 在應用程式的目錄中尋找 *pom.xml* 檔案；例如：

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   -或-

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![尋找 pom.xml 檔案][PM01]

1. 在文字編輯器中開啟 *pom.xml* 檔案，並將下列數行新增至 `<dependencies>` 的清單中：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][PM02]

1. 請確認該 Spring Boot 版本為您在使用 Spring Initializr 建立應用程式時所選用的版本，例如：

   ```xml
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.1.5.RELEASE</version>
      <relativePath/>
   </parent>
   ```

1. 請確認您使用的是最新的 [Azure Spring Boot Starter](https://github.com/microsoft/azure-spring-boot) 版本，例如：

   ```xml
   <azure.version>2.1.6</azure.version>
   ```

1. 儲存並關閉 *pom.xml* 檔案。

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>設定 Spring Boot 應用程式以使用您的 Azure Cosmos DB

1. 在應用程式的 [資源]  目錄中尋找 *application.properties* 檔案；例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![尋找 application.properties 檔案][RE01]

1. 在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用您資料庫的適當屬性來取代範例值：

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.cosmosdb.database=wingtiptoysdata
   ```

   ![編輯 application.properties 檔案][RE02]

1. 儲存並關閉 *application.properties* 檔案。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>新增範例程式碼來實作基本的資料庫功能

在本節中，您會建立兩個 Java 類別以供儲存使用者資料，然後修改您的主要應用程式類別，以建立*使用者*類別的執行個體，並將其儲存至您的資料庫。

### <a name="define-a-base-class-for-storing-user-data"></a>定義用來儲存使用者資料的基底類別

1. 在與主要應用程式 Java 檔案相同的目錄中，建立名為 *User.java* 的新檔案。

1. 在文字編輯器中開啟 *User.java* 檔案，然後下列數行新增至檔案中，以定義泛型使用者類別來儲存和擷取您資料庫中的值：

   ```java
   package com.example.wingtiptoysdata;

   // Define a generic User class.
   public class User {
      private String id;
      private String firstName;
      private String lastName;

      public User() {
      }

      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }

      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s %s", id, firstName, lastName);
      }
   }
   ```

1. 儲存並關閉 *User.java* 檔案。

### <a name="define-a-data-repository-interface"></a>定義資料存放庫介面

1. 在與主要應用程式 Java 檔案相同的目錄中，建立名為 *UserRepository.java* 的新檔案。

1. 在文字編輯器中開啟 *UserRepository.java* 檔案，然後將下列數行新增至檔案中，以定義使用者存放庫介面來延伸預設的 DocumentDB 存放庫介面：

   ```java
   package com.example.wingtiptoysdata;

   import com.microsoft.azure.spring.data.cosmosdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> { }
   ```

1. 儲存並關閉 *UserRepository.java* 檔案。

### <a name="modify-the-main-application-class"></a>修改主要應用程式類別

1. 在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![尋找應用程式 Java 檔案][JV01]

1. 在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：

   ```java
    package com.example.wingtiptoysdata;

    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    import java.util.Optional;
    import java.util.UUID;

    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {

        private final UserRepository repository;

        public WingtiptoysdataApplication(UserRepository repository) {
            this.repository = repository;
        }

        public static void main(String[] args) {
            // Execute the command line runner.
            SpringApplication.run(WingtiptoysdataApplication.class, args);
            System.exit(0);
        }

        public void run(String... args) throws Exception {
            // Create a unique identifier.
            String uuid = UUID.randomUUID().toString();

            // Create a new User class.
            final User testUser = new User(uuid, "John", "Doe");

            // For this example, remove all of the existing records.
            repository.deleteAll();

            // Save the User class to the Azure database.
            repository.save(testUser);

            // Retrieve the database record for the User class you just saved by ID.
            Optional<User> result = repository.findById(testUser.getId());

            // Display the results of the database record retrieval.
            System.out.println("\nSaved user is: " + result + "\n")
        }
    }
   ```

1. 儲存並關閉主要應用程式 Java 檔案。

## <a name="build-and-test-your-app"></a>建置及測試您的應用程式

1. 開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：

   `cd C:\SpringBoot\wingtiptoysdata`

   -或-

   `cd /users/example/home/wingtiptoysdata`

1. 使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：

   ```shell
   mvnw clean spring-boot:run
   ```

1. 您的應用程式將顯示數個執行階段訊息，並且會顯示如下範例所示的訊息，表示值已成功儲存並可從您的資料庫擷取。

   ```shell
   Saved user is: Optional[User: 24093cb5-55fe-4d2c-b459-cb8bafdd39fe John Doe]
   ```

   ![已成功從應用程式輸出][JV02]

1. 選擇性：您可以使用 Azure 入口網站，從資料庫的屬性頁面中檢視 Azure Cosmos DB 的內容，方法是按一下 [資料總管]  ，然後從顯示的清單中選取要檢視內容的項目。

   ![使用 [文件總管] 檢視資料][JV03]

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他資源

如需使用 Azure Cosmos DB 和 Java 的詳細資訊，請參閱下列文章：

* [Azure Cosmos DB 文件]。

* [Azure Cosmos DB：使用 Java 和 Azure 入口網站建立文件資料庫][Build a SQL API app with Java]

* [適用於 Azure Cosmos DB SQL API 的 Spring 資料]

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [適用於 Azure 的 Spring Boot Cosmos DB Starter]

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式](deploy-spring-boot-java-app-on-kubernetes.md)

如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。

**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。 [Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。 為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。 除了從基本的 Spring Boot 專案清單中進行選擇， **[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。

<!-- URL List -->

[Azure Cosmos DB 文件]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[適用於 Azure Cosmos DB SQL API 的 Spring 資料]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[適用於 Azure 的 Spring Boot Cosmos DB Starter]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: https://azure.microsoft.com/services/devops/java/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV03.png
