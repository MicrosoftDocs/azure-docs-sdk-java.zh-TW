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
ms.date: 11/21/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 6675b3f76f19ec0bfdb28351681258b8c4792104
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339102"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>如何搭配 Azure Cosmos DB SQL API 使用 Spring Boot Starter

## <a name="overview"></a>概觀

Azure Cosmos DB 是一個橫跨全球的分散式資料庫服務，讓開發人員能夠利用各種不同的標準 API (例如 SQL、MongoDB、圖形和資料表 API) 來使用資料。 Microsoft 的 Spring Boot Starter 讓開發人員能夠使用 Spring Boot 應用程式，藉由使用 SQL API 來輕鬆地與 Azure Cosmos DB 整合。

本文將示範如何使用 Azure 入口網站建立 Azure Cosmos DB，接著使用 **[Spring Initializr]** 建立自訂的 Java 應用程式，然後將 Spring Boot Starter 功能新增到您的自訂應用程式，以使用 SQL API 將資料儲存於您的 Azure Cosmos DB 以及從中擷取資料。

## <a name="prerequisites"></a>必要條件

若要遵循本文中的步驟，需要具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>使用 Azure 入口網站建立 Azure Cosmos DB

1. 瀏覽至 Azure 入口網站 <https://portal.azure.com/> ，然後按一下 **+ 新增資源** 。

   ![Azure 入口網站][AZ01]

1. 依序按一下 [資料庫] 和 [Azure Cosmos DB]。

   ![Azure 入口網站][AZ02]

1. 在 [Azure Cosmos DB] 頁面上，輸入下列資訊：

   * 輸入唯一的**識別碼**，您將使用此識別碼作為資料庫的 URI。 例如：*wingtiptoysdata.documents.azure.com*。
   * 為 API 選擇 **SQL**。
   * 選取您想要用於資料庫的**訂用帳戶**。
   * 指定是否要為資料庫建立新的**資源群組**，或選擇現有的資源群組。
   * 指定資料庫的**位置**。
   
   當您指定這些選項之後，按一下 [建立] 以建立您的資料庫。

   ![Azure 入口網站][AZ03]

1. 當您的資料庫建立之後，它會列在您的 Azure [儀表板] 中，以及 [所有資源] 和 [Azure Cosmos DB] 頁面下方。 您可以按一下這些任何位置上的資料庫，來開啟快取的屬性頁面。

   ![Azure 入口網站][AZ04]

1. 當資料庫的屬性頁面顯示時，按一下 [存取金鑰]，並複製資料庫的 URI 和存取金鑰；您將會在 Spring Boot 應用程式中使用這些值。

   ![Azure 入口網站][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 建立簡單的 Spring Boot 應用程式

1. 瀏覽至 <https://start.spring.io/> 。

1. 指定您想要使用 **Java** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱、指定您的 **Spring Boot** 版本，然後按一下按鈕以**產生專案**。

   > [!IMPORTANT]
   >
   > Spring Boot 版本 2.0.n 中的 API 包含數個重大變更，本文會使用這些變更來完成一些步驟。 您仍然可以使用其中一個 Spring Boot 1.5.n 版本來完成本教學課程中的步驟，必要時本文中會標明不同版本的相異之處。
   >

   ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：*com.example.wintiptoys*。
   >

1. 出現提示時，將專案下載至本機電腦上的路徑。

   ![下載自訂的 Spring Boot 專案][SI02]

1. 當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。

   ![自訂的 Spring Boot 專案檔][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a>設定 Spring Boot 應用程式以使用 Azure Spring Boot Starter

1. 在應用程式的目錄中尋找 *pom.xml* 檔案；例如：

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   -或-

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![尋找 pom.xml 檔案][PM01]

1. 在文字編輯器中開啟 *pom.xml* 檔案，並將下列數行新增至 `<dependencies>` 的清單中：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>2.0.4</version>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][PM02]

   > [!IMPORTANT]
   >
   > 如果您是使用其中一個 Spring Boot 1.5.n 版本完成本教學課程，則需要指定 Azure Cosmos DB Starter 的較舊版本，例如：
   >
   > ```xml
   > <dependency>
   >   <groupId>com.microsoft.azure</groupId>
   >   <artifactId>azure-documentdb-spring-boot-starter</artifactId>
   >   <version>0.1.4</version>
   > </dependency>
   > ```

1. 請確認該 Spring Boot 版本為您在使用 Spring Initializr 建立應用程式時所選用的版本，例如：

   ```xml
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.0.1.RELEASE</version>
      <relativePath/>
   </parent>
   ```

   > [!NOTE]
   >
   > 如果您使用的是其中一個 Spring Boot 1.5.n 版本完成本教學課程，則需要確認正確的版本，例如：`<version>1.5.14.RELEASE</version>`。
   >

1. 儲存並關閉 *pom.xml* 檔案。

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>設定 Spring Boot 應用程式以使用您的 Azure Cosmos DB

1. 在應用程式的 [資源] 目錄中尋找 *application.properties* 檔案；例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![尋找 application.properties 檔案][RE01]

1. 在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用您資料庫的適當屬性來取代範例值：

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![編輯 application.properties 檔案][RE02]

1. 儲存並關閉 *application.properties* 檔案。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>新增範例程式碼來實作基本的資料庫功能

在本節中，您會建立兩個 Java 類別以供儲存使用者資料，然後修改您的主要應用程式類別，以建立使用者類別的執行個體，並將其儲存至您的資料庫。

### <a name="define-a-basic-class-for-storing-user-data"></a>定義用來儲存使用者資料的基本類別

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
   
   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
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

   // These imports are required for the application.
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   // These imports are only used to create an ID for this example.
   import java.util.Date;
   import java.text.SimpleDateFormat;

   @SpringBootApplication
   public class wingtiptoysdataApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;

      public static void main(String[] args) {
         // Execute the command line runner.
         SpringApplication.run(wingtiptoysdataApplication.class, args);
         System.exit(0);
      }

      public void run(String... args) throws Exception {
         // Create a simple date/time ID.
         SimpleDateFormat userId = new SimpleDateFormat("yyyyMMddHHmmssSSS");
         Date currentDate = new Date();

         // Create a new User class.
         final User testUser = new User(userId.format(currentDate), "Gena", "Soto");

         // For this example, remove all of the existing records.
         repository.deleteAll();

         // Save the User class to the Azure database.
         repository.save(testUser);
      
         // Retrieve the database record for the User class you just saved by ID.
         // final User result = repository.findOne(testUser.getId());
         final User result = repository.findById(testUser.getId()).get();

         // Display the results of the database record retrieval.
         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

   > [!IMPORTANT]
   >
   > 如果您使用的是其中一個 Spring Boot 1.5.n 版本完成本教學課程，則需要以 `final User result = repository.findOne(testUser.getId());` 來取代 `final User result = repository.findById(testUser.getId()).get();` 語法。
   >

1. 儲存並關閉主要應用程式 Java 檔案。

## <a name="build-and-test-your-app"></a>建置及測試您的應用程式

1. 開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：

   `cd C:\SpringBoot\wingtiptoysdata`

   -或-

   `cd /users/example/home/wingtiptoysdata`

1. 使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 您的應用程式將顯示數個執行階段訊息，並且會顯示如下範例所示的訊息，表示值已成功儲存並可從您的資料庫擷取。

   ```
   User: 20170724025215132 Gena Soto
   ```

   ![已成功從應用程式輸出][JV02]

1. 選擇性：您可以使用 Azure 入口網站，從資料庫的屬性頁面中檢視 Azure Cosmos DB 的內容，方法是按一下 [資料總管]，然後從顯示的清單中選取要檢視內容的項目。

   ![使用 [文件總管] 檢視資料][JV03]

## <a name="next-steps"></a>後續步驟

如需使用 Azure Cosmos DB 和 Java 的詳細資訊，請參閱下列文章：

* [Azure Cosmos DB 文件]。

* [Azure Cosmos DB︰使用 Java 和 Azure 入口網站建立文件資料庫][Build a SQL API app with Java]

* [適用於 Azure Cosmos DB SQL API 的 Spring 資料]

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [適用於 Azure 的 Spring Boot DocumenDB Starter]

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式](deploy-spring-boot-java-app-on-kubernetes.md)

如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。

**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。 [Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。 為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。 除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。

<!-- URL List -->

[Azure Cosmos DB 文件]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[適用於 Azure Cosmos DB SQL API 的 Spring 資料]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[適用於 Azure 的 Spring Boot DocumenDB Starter]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[適用於 Visual Studio Team Services 的 Java 工具]: https://azure.microsoft.com/services/devops/java/
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
