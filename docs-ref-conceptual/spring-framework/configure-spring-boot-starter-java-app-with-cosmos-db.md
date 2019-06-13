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
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="4fee6-103">如何搭配 Azure Cosmos DB SQL API 使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="4fee6-103">How to use the Spring Boot Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="4fee6-104">概觀</span><span class="sxs-lookup"><span data-stu-id="4fee6-104">Overview</span></span>

<span data-ttu-id="4fee6-105">Azure Cosmos DB 是一個橫跨全球的分散式資料庫服務，讓開發人員能夠利用各種不同的標準 API (例如 SQL、MongoDB、圖形和資料表 API) 來使用資料。</span><span class="sxs-lookup"><span data-stu-id="4fee6-105">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as SQL, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="4fee6-106">Microsoft 的 Spring Boot Starter 讓開發人員能夠使用 Spring Boot 應用程式，藉由使用 SQL API 來輕鬆地與 Azure Cosmos DB 整合。</span><span class="sxs-lookup"><span data-stu-id="4fee6-106">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using the SQL API.</span></span>

<span data-ttu-id="4fee6-107">本文將示範如何使用 Azure 入口網站建立 Azure Cosmos DB，接著使用 **[Spring Initializr]** 建立自訂的 Spring Boot 應用程式，然後將 [適用於 Azure 的 Spring Boot Cosmos DB Starter] 新增到您的自訂應用程式，以使用 Spring Data 和 Cosmos DB SQL API 將資料儲存在您的 Azure Cosmos DB 並從中擷取資料。</span><span class="sxs-lookup"><span data-stu-id="4fee6-107">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **[Spring Initializr]** to create a custom Spring Boot application, and then add the [Spring Boot Cosmos DB Starter for Azure] to your custom application to store data in and retrieve data from your Azure Cosmos DB by using Spring Data and the Cosmos DB SQL API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4fee6-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="4fee6-108">Prerequisites</span></span>

<span data-ttu-id="4fee6-109">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="4fee6-109">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="4fee6-110">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="4fee6-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="4fee6-111">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="4fee6-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="4fee6-112">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="4fee6-112">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="4fee6-113">使用 Azure 入口網站建立 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4fee6-113">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="4fee6-114">瀏覽至 Azure 入口網站 <https://portal.azure.com/> ，然後按一下 **+ 新增資源** 。</span><span class="sxs-lookup"><span data-stu-id="4fee6-114">Browse to the Azure portal at <https://portal.azure.com/> and click **+Create a resource**.</span></span>

   ![Azure 入口網站][AZ01]

1. <span data-ttu-id="4fee6-116">依序按一下 [資料庫]  和 [Azure Cosmos DB]  。</span><span class="sxs-lookup"><span data-stu-id="4fee6-116">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure 入口網站][AZ02]

1. <span data-ttu-id="4fee6-118">在 [Azure Cosmos DB]  頁面上，輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="4fee6-118">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="4fee6-119">選取您想要用於資料庫的**訂用帳戶**。</span><span class="sxs-lookup"><span data-stu-id="4fee6-119">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="4fee6-120">指定是否要為資料庫建立新的**資源群組**，或選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="4fee6-120">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="4fee6-121">輸入唯一的**帳戶名稱**，您將使用此名稱作為資料庫的 URI。</span><span class="sxs-lookup"><span data-stu-id="4fee6-121">Enter a unique **Account Name**, which you will use as the URI for your database.</span></span> <span data-ttu-id="4fee6-122">例如：*wingtiptoysdata*。</span><span class="sxs-lookup"><span data-stu-id="4fee6-122">For example: *wingtiptoysdata*.</span></span>
   * <span data-ttu-id="4fee6-123">為 API 選擇**核心 (SQL)** 。</span><span class="sxs-lookup"><span data-stu-id="4fee6-123">Choose **Core (SQL)** for the API.</span></span>
   * <span data-ttu-id="4fee6-124">指定資料庫的**位置**。</span><span class="sxs-lookup"><span data-stu-id="4fee6-124">Specify the **Location** for your database.</span></span>

   <span data-ttu-id="4fee6-125">您指定這些選項之後，按一下 [檢閱+建立]  以建立您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4fee6-125">When you have specified these options, click **Review + create** to create your database.</span></span>

   ![Azure 入口網站][AZ03]

1. <span data-ttu-id="4fee6-127">當您的資料庫建立之後，它會列在您的 Azure [儀表板]  中，以及 [所有資源]  和 [Azure Cosmos DB]  頁面下方。</span><span class="sxs-lookup"><span data-stu-id="4fee6-127">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="4fee6-128">您可以按一下這些任何位置上的資料庫，來開啟快取的屬性頁面。</span><span class="sxs-lookup"><span data-stu-id="4fee6-128">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Azure 入口網站][AZ04]

1. <span data-ttu-id="4fee6-130">當資料庫的屬性頁面顯示時，按一下 [金鑰]  ，並複製資料庫的 URI 和存取金鑰；您將會在 Spring Boot 應用程式中使用這些值。</span><span class="sxs-lookup"><span data-stu-id="4fee6-130">When the properties page for your database is displayed, click **Keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure 入口網站][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="4fee6-132">使用 Spring Initializr 建立簡單的 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="4fee6-132">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="4fee6-133">瀏覽至 <https://start.spring.io/> 。</span><span class="sxs-lookup"><span data-stu-id="4fee6-133">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="4fee6-134">指定您想要使用 **Java** 產生 **Maven 專案**指定您的 **Spring Boot** 版本、輸入應用程式的**群組**和**成品**名稱、在相依性中新增 **Azure 支援**，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="4fee6-134">Specify that you want to generate a **Maven Project** with **Java**, specify your **Spring Boot** version, enter the **Group** and **Artifact** names for your application, add **Azure Support** in the dependencies, and then click the button to **Generate Project**.</span></span>

   ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="4fee6-136">Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：*com.example.wintiptoys*。</span><span class="sxs-lookup"><span data-stu-id="4fee6-136">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoysdata*.</span></span>
   >

1. <span data-ttu-id="4fee6-137">出現提示時，將專案下載至本機電腦上的路徑，並將檔案解壓縮。</span><span class="sxs-lookup"><span data-stu-id="4fee6-137">When prompted, download the project to a path on your local computer and extract the files.</span></span>

   ![將自訂的 Spring Boot 專案解壓縮][SI02]

1. <span data-ttu-id="4fee6-139">當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4fee6-139">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![自訂的 Spring Boot 專案檔][SI03]

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="4fee6-141">設定 Spring Boot 應用程式以使用 Azure Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="4fee6-141">Configure your Spring Boot application to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="4fee6-142">在應用程式的目錄中尋找 *pom.xml* 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="4fee6-142">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   <span data-ttu-id="4fee6-143">-或-</span><span class="sxs-lookup"><span data-stu-id="4fee6-143">-or-</span></span>

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![尋找 pom.xml 檔案][PM01]

1. <span data-ttu-id="4fee6-145">在文字編輯器中開啟 *pom.xml* 檔案，並將下列數行新增至 `<dependencies>` 的清單中：</span><span class="sxs-lookup"><span data-stu-id="4fee6-145">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][PM02]

1. <span data-ttu-id="4fee6-147">請確認該 Spring Boot 版本為您在使用 Spring Initializr 建立應用程式時所選用的版本，例如：</span><span class="sxs-lookup"><span data-stu-id="4fee6-147">Verify that the Spring Boot version is the version that you chose when you created your application with the Spring Initializr; for example:</span></span>

   ```xml
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.1.5.RELEASE</version>
      <relativePath/>
   </parent>
   ```

1. <span data-ttu-id="4fee6-148">請確認您使用的是最新的 [Azure Spring Boot Starter](https://github.com/microsoft/azure-spring-boot) 版本，例如：</span><span class="sxs-lookup"><span data-stu-id="4fee6-148">Verify that you use the most recent [Azure Spring Boot starters](https://github.com/microsoft/azure-spring-boot) version, for example:</span></span>

   ```xml
   <azure.version>2.1.6</azure.version>
   ```

1. <span data-ttu-id="4fee6-149">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="4fee6-149">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a><span data-ttu-id="4fee6-150">設定 Spring Boot 應用程式以使用您的 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4fee6-150">Configure your Spring Boot application to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="4fee6-151">在應用程式的 [資源]  目錄中尋找 *application.properties* 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="4fee6-151">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   <span data-ttu-id="4fee6-152">-或-</span><span class="sxs-lookup"><span data-stu-id="4fee6-152">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![尋找 application.properties 檔案][RE01]

1. <span data-ttu-id="4fee6-154">在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用您資料庫的適當屬性來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="4fee6-154">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.cosmosdb.database=wingtiptoysdata
   ```

   ![編輯 application.properties 檔案][RE02]

1. <span data-ttu-id="4fee6-156">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="4fee6-156">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="4fee6-157">新增範例程式碼來實作基本的資料庫功能</span><span class="sxs-lookup"><span data-stu-id="4fee6-157">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="4fee6-158">在本節中，您會建立兩個 Java 類別以供儲存使用者資料，然後修改您的主要應用程式類別，以建立*使用者*類別的執行個體，並將其儲存至您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4fee6-158">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the *User* class and save it to your database.</span></span>

### <a name="define-a-base-class-for-storing-user-data"></a><span data-ttu-id="4fee6-159">定義用來儲存使用者資料的基底類別</span><span class="sxs-lookup"><span data-stu-id="4fee6-159">Define a base class for storing user data</span></span>

1. <span data-ttu-id="4fee6-160">在與主要應用程式 Java 檔案相同的目錄中，建立名為 *User.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="4fee6-160">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="4fee6-161">在文字編輯器中開啟 *User.java* 檔案，然後下列數行新增至檔案中，以定義泛型使用者類別來儲存和擷取您資料庫中的值：</span><span class="sxs-lookup"><span data-stu-id="4fee6-161">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

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

1. <span data-ttu-id="4fee6-162">儲存並關閉 *User.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="4fee6-162">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="4fee6-163">定義資料存放庫介面</span><span class="sxs-lookup"><span data-stu-id="4fee6-163">Define a data repository interface</span></span>

1. <span data-ttu-id="4fee6-164">在與主要應用程式 Java 檔案相同的目錄中，建立名為 *UserRepository.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="4fee6-164">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="4fee6-165">在文字編輯器中開啟 *UserRepository.java* 檔案，然後將下列數行新增至檔案中，以定義使用者存放庫介面來延伸預設的 DocumentDB 存放庫介面：</span><span class="sxs-lookup"><span data-stu-id="4fee6-165">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoysdata;

   import com.microsoft.azure.spring.data.cosmosdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> { }
   ```

1. <span data-ttu-id="4fee6-166">儲存並關閉 *UserRepository.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="4fee6-166">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="4fee6-167">修改主要應用程式類別</span><span class="sxs-lookup"><span data-stu-id="4fee6-167">Modify the main application class</span></span>

1. <span data-ttu-id="4fee6-168">在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="4fee6-168">Locate the main application Java file in the package directory of your application, for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   <span data-ttu-id="4fee6-169">-或-</span><span class="sxs-lookup"><span data-stu-id="4fee6-169">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![尋找應用程式 Java 檔案][JV01]

1. <span data-ttu-id="4fee6-171">在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：</span><span class="sxs-lookup"><span data-stu-id="4fee6-171">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

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

1. <span data-ttu-id="4fee6-172">儲存並關閉主要應用程式 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="4fee6-172">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="4fee6-173">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="4fee6-173">Build and test your app</span></span>

1. <span data-ttu-id="4fee6-174">開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：</span><span class="sxs-lookup"><span data-stu-id="4fee6-174">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoysdata`

   <span data-ttu-id="4fee6-175">-或-</span><span class="sxs-lookup"><span data-stu-id="4fee6-175">-or-</span></span>

   `cd /users/example/home/wingtiptoysdata`

1. <span data-ttu-id="4fee6-176">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="4fee6-176">Build your Spring Boot application with Maven and run it, for example:</span></span>

   ```shell
   mvnw clean spring-boot:run
   ```

1. <span data-ttu-id="4fee6-177">您的應用程式將顯示數個執行階段訊息，並且會顯示如下範例所示的訊息，表示值已成功儲存並可從您的資料庫擷取。</span><span class="sxs-lookup"><span data-stu-id="4fee6-177">Your application will display several runtime messages, and it will display a message like the following examples to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ```shell
   Saved user is: Optional[User: 24093cb5-55fe-4d2c-b459-cb8bafdd39fe John Doe]
   ```

   ![已成功從應用程式輸出][JV02]

1. <span data-ttu-id="4fee6-179">選擇性：您可以使用 Azure 入口網站，從資料庫的屬性頁面中檢視 Azure Cosmos DB 的內容，方法是按一下 [資料總管]  ，然後從顯示的清單中選取要檢視內容的項目。</span><span class="sxs-lookup"><span data-stu-id="4fee6-179">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Data Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![使用 [文件總管] 檢視資料][JV03]

## <a name="next-steps"></a><span data-ttu-id="4fee6-181">後續步驟</span><span class="sxs-lookup"><span data-stu-id="4fee6-181">Next steps</span></span>

<span data-ttu-id="4fee6-182">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="4fee6-182">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4fee6-183">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="4fee6-183">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="4fee6-184">其他資源</span><span class="sxs-lookup"><span data-stu-id="4fee6-184">Additional Resources</span></span>

<span data-ttu-id="4fee6-185">如需使用 Azure Cosmos DB 和 Java 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="4fee6-185">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="4fee6-186">[Azure Cosmos DB 文件]。</span><span class="sxs-lookup"><span data-stu-id="4fee6-186">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="4fee6-187">[Azure Cosmos DB：使用 Java 和 Azure 入口網站建立文件資料庫][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="4fee6-187">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="4fee6-188">[適用於 Azure Cosmos DB SQL API 的 Spring 資料]</span><span class="sxs-lookup"><span data-stu-id="4fee6-188">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="4fee6-189">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="4fee6-189">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* <span data-ttu-id="4fee6-190">[適用於 Azure 的 Spring Boot Cosmos DB Starter]</span><span class="sxs-lookup"><span data-stu-id="4fee6-190">[Spring Boot Cosmos DB Starter for Azure]</span></span>

* [<span data-ttu-id="4fee6-191">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fee6-191">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="4fee6-192">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="4fee6-192">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="4fee6-193">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="4fee6-193">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="4fee6-194">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="4fee6-194">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="4fee6-195">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="4fee6-195">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="4fee6-196">為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="4fee6-196">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="4fee6-197">除了從基本的 Spring Boot 專案清單中進行選擇， **[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4fee6-197">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Cosmos DB 文件]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[適用於 Azure Cosmos DB SQL API 的 Spring 資料]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[適用於 Azure 的 Spring Boot Cosmos DB Starter]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[Spring Boot Cosmos DB Starter for Azure]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: https://azure.microsoft.com/services/devops/java/
[Working with Azure DevOps and Java]: https://azure.microsoft.com/services/devops/java/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
