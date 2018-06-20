---
title: 如何搭配 Azure Cosmos DB SQL API 使用 Spring Boot Starter
description: 了解如何設定搭配 Azure Cosmos DB SQL API 使用 Spring Boot Initializer 建立的應用程式。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm;yungez;kevinzha
ms.date: 02/01/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 6cf999f3db397760709476dae1f5c0fd83503725
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823801"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="a0e05-103">如何搭配 Azure Cosmos DB SQL API 使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="a0e05-103">How to use the Spring Boot Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="a0e05-104">概觀</span><span class="sxs-lookup"><span data-stu-id="a0e05-104">Overview</span></span>

<span data-ttu-id="a0e05-105">Azure Cosmos DB 是一個橫跨全球的分散式資料庫服務，讓開發人員能夠利用各種不同的標準 API (例如 SQL、MongoDB、圖形和資料表 API) 來使用資料。</span><span class="sxs-lookup"><span data-stu-id="a0e05-105">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as SQL, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="a0e05-106">Microsoft 的 Spring Boot Starter 讓開發人員能夠使用 Spring Boot 應用程式，藉由使用 SQL API 來輕鬆地與 Azure Cosmos DB 整合。</span><span class="sxs-lookup"><span data-stu-id="a0e05-106">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using the SQL API.</span></span>

<span data-ttu-id="a0e05-107">本文將示範如何使用 Azure 入口網站建立 Azure Cosmos DB，接著使用 **[Spring Initializr]** 建立自訂的 Java 應用程式，然後將 Spring Boot Starter 功能新增到您的自訂應用程式，以使用 SQL API 將資料儲存於您的 Azure Cosmos DB 以及從中擷取資料。</span><span class="sxs-lookup"><span data-stu-id="a0e05-107">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **[Spring Initializr]** to create a custom java application, and then add the Spring Boot Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using the SQL API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0e05-108">先決條件</span><span class="sxs-lookup"><span data-stu-id="a0e05-108">Prerequisites</span></span>

<span data-ttu-id="a0e05-109">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="a0e05-109">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="a0e05-110">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="a0e05-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="a0e05-111">[Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="a0e05-111">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="a0e05-112">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="a0e05-112">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="a0e05-113">使用 Azure 入口網站建立 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a0e05-113">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="a0e05-114">瀏覽至 Azure 入口網站 <https://portal.azure.com/>，然後按一下 [+ 新增]。</span><span class="sxs-lookup"><span data-stu-id="a0e05-114">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure 入口網站][AZ01]

1. <span data-ttu-id="a0e05-116">依序按一下 [資料庫] 和 [Azure Cosmos DB]。</span><span class="sxs-lookup"><span data-stu-id="a0e05-116">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure 入口網站][AZ02]

1. <span data-ttu-id="a0e05-118">在 [Azure Cosmos DB] 頁面上，輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="a0e05-118">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="a0e05-119">輸入唯一的**識別碼**，您將使用此識別碼作為資料庫的 URI。</span><span class="sxs-lookup"><span data-stu-id="a0e05-119">Enter a unique **ID**, which you will use as the URI for your database.</span></span> <span data-ttu-id="a0e05-120">例如：*wingtiptoysdata.documents.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="a0e05-120">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="a0e05-121">針對 API 選擇 [SQL (Document DB)]。</span><span class="sxs-lookup"><span data-stu-id="a0e05-121">Choose **SQL (Document DB)** for the API.</span></span>
   * <span data-ttu-id="a0e05-122">選取您想要用於資料庫的**訂用帳戶**。</span><span class="sxs-lookup"><span data-stu-id="a0e05-122">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="a0e05-123">指定是否要為資料庫建立新的**資源群組**，或選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="a0e05-123">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="a0e05-124">指定資料庫的**位置**。</span><span class="sxs-lookup"><span data-stu-id="a0e05-124">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="a0e05-125">當您指定這些選項之後，按一下 [建立] 以建立您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0e05-125">When you have specified these options, click **Create** to create your database.</span></span>

   ![Azure 入口網站][AZ03]

1. <span data-ttu-id="a0e05-127">當您的資料庫建立之後，它會列在您的 Azure [儀表板] 中，以及 [所有資源] 和 [Azure Cosmos DB] 頁面下方。</span><span class="sxs-lookup"><span data-stu-id="a0e05-127">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="a0e05-128">您可以按一下這些任何位置上的資料庫，來開啟快取的屬性頁面。</span><span class="sxs-lookup"><span data-stu-id="a0e05-128">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Azure 入口網站][AZ04]

1. <span data-ttu-id="a0e05-130">當資料庫的屬性頁面顯示時，按一下 [存取金鑰]，並複製資料庫的 URI 和存取金鑰；您將會在 Spring Boot 應用程式中使用這些值。</span><span class="sxs-lookup"><span data-stu-id="a0e05-130">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure 入口網站][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="a0e05-132">使用 Spring Initializr 建立簡單的 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="a0e05-132">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="a0e05-133">瀏覽至 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="a0e05-133">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="a0e05-134">指定您想要使用 **Java** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="a0e05-134">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the button to **Generate Project**.</span></span>

   ![Basic Spring Initializr 選項][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="a0e05-136">Spring Initializr 會使用**群組**和**成品**名稱來建立封裝名稱；例如：*com.example.wintiptoys*。</span><span class="sxs-lookup"><span data-stu-id="a0e05-136">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoys*.</span></span>
   >

1. <span data-ttu-id="a0e05-137">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="a0e05-137">When prompted, download the project to a path on your local computer.</span></span>

   ![下載自訂的 Spring Boot 專案][SI02]

1. <span data-ttu-id="a0e05-139">當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0e05-139">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![自訂的 Spring Boot 專案檔][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="a0e05-141">設定 Spring Boot 應用程式以使用 Azure Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="a0e05-141">Configure your Spring Boot app to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="a0e05-142">在應用程式的目錄中尋找 *pom.xml* 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="a0e05-142">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\pom.xml`

   <span data-ttu-id="a0e05-143">-或-</span><span class="sxs-lookup"><span data-stu-id="a0e05-143">-or-</span></span>

   `/users/example/home/wingtiptoys/pom.xml`

   ![尋找 pom.xml 檔案][PM01]

1. <span data-ttu-id="a0e05-145">在文字編輯器中開啟 *pom.xml* 檔案，並將下列數行新增至 `<dependencies>` 的清單中：</span><span class="sxs-lookup"><span data-stu-id="a0e05-145">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][PM02]

1. <span data-ttu-id="a0e05-147">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="a0e05-147">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="a0e05-148">設定 Spring Boot 應用程式以使用您的 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a0e05-148">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="a0e05-149">在應用程式的 [資源] 目錄中尋找 *application.properties* 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="a0e05-149">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   <span data-ttu-id="a0e05-150">-或-</span><span class="sxs-lookup"><span data-stu-id="a0e05-150">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![尋找 application.properties 檔案][RE01]

1. <span data-ttu-id="a0e05-152">在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用您資料庫的適當屬性來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="a0e05-152">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![編輯 application.properties 檔案][RE02]

1. <span data-ttu-id="a0e05-154">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="a0e05-154">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="a0e05-155">新增範例程式碼來實作基本的資料庫功能</span><span class="sxs-lookup"><span data-stu-id="a0e05-155">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="a0e05-156">在本節中，您會建立兩個 Java 類別以供儲存使用者資料，然後修改您的主要應用程式類別，以建立使用者類別的執行個體，並將其儲存至您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0e05-156">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the user class and save it to your database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="a0e05-157">定義用來儲存使用者資料的基本類別</span><span class="sxs-lookup"><span data-stu-id="a0e05-157">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="a0e05-158">在與主要應用程式 Java 檔案相同的目錄中，建立名為 *User.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="a0e05-158">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="a0e05-159">在文字編輯器中開啟 *User.java* 檔案，然後下列數行新增至檔案中，以定義泛型使用者類別來儲存和擷取您資料庫中的值：</span><span class="sxs-lookup"><span data-stu-id="a0e05-159">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
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
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. <span data-ttu-id="a0e05-160">儲存並關閉 *User.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="a0e05-160">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="a0e05-161">定義資料存放庫介面</span><span class="sxs-lookup"><span data-stu-id="a0e05-161">Define a data repository interface</span></span>

1. <span data-ttu-id="a0e05-162">在與主要應用程式 Java 檔案相同的目錄中，建立名為 *UserRepository.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="a0e05-162">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="a0e05-163">在文字編輯器中開啟 *UserRepository.java* 檔案，然後將下列數行新增至檔案中，以定義使用者存放庫介面來延伸預設的 DocumentDB 存放庫介面：</span><span class="sxs-lookup"><span data-stu-id="a0e05-163">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. <span data-ttu-id="a0e05-164">儲存並關閉 *UserRepository.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="a0e05-164">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="a0e05-165">修改主要應用程式類別</span><span class="sxs-lookup"><span data-stu-id="a0e05-165">Modify the main application class</span></span>

1. <span data-ttu-id="a0e05-166">在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="a0e05-166">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   <span data-ttu-id="a0e05-167">-或-</span><span class="sxs-lookup"><span data-stu-id="a0e05-167">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![尋找應用程式 Java 檔案][JV01]

1. <span data-ttu-id="a0e05-169">在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：</span><span class="sxs-lookup"><span data-stu-id="a0e05-169">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. <span data-ttu-id="a0e05-170">儲存並關閉主要應用程式 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="a0e05-170">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="a0e05-171">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="a0e05-171">Build and test your app</span></span>

1. <span data-ttu-id="a0e05-172">開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：</span><span class="sxs-lookup"><span data-stu-id="a0e05-172">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoys`

   <span data-ttu-id="a0e05-173">-或-</span><span class="sxs-lookup"><span data-stu-id="a0e05-173">-or-</span></span>

   `cd /users/example/home/wingtiptoys`

1. <span data-ttu-id="a0e05-174">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="a0e05-174">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. <span data-ttu-id="a0e05-175">您的應用程式將顯示數個執行階段訊息，而您應該會看到 `User: testFirstName testLastName` 這個訊息，表示值已成功儲存並可從您的資料庫擷取。</span><span class="sxs-lookup"><span data-stu-id="a0e05-175">Your application will display several runtime messages, and you should see the message `User: testFirstName testLastName` displayed to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ![已成功從應用程式輸出][JV02]

1. <span data-ttu-id="a0e05-177">選擇性：您可以使用 Azure 入口網站，從資料庫的屬性頁面中檢視 Azure Cosmos DB 的內容，方法是按一下 [文件總管]，然後從顯示的清單中選取要檢視內容的項目。</span><span class="sxs-lookup"><span data-stu-id="a0e05-177">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Document Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![使用 [文件總管] 檢視資料][JV03]

## <a name="next-steps"></a><span data-ttu-id="a0e05-179">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a0e05-179">Next steps</span></span>

<span data-ttu-id="a0e05-180">如需使用 Azure Cosmos DB 和 Java 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="a0e05-180">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="a0e05-181">[Azure Cosmos DB 文件]。</span><span class="sxs-lookup"><span data-stu-id="a0e05-181">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="a0e05-182">[Azure Cosmos DB︰使用 Java 和 Azure 入口網站建立文件資料庫][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="a0e05-182">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="a0e05-183">[適用於 Azure Cosmos DB SQL API 的 Spring 資料]</span><span class="sxs-lookup"><span data-stu-id="a0e05-183">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="a0e05-184">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="a0e05-184">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* <span data-ttu-id="a0e05-185">[適用於 Azure 的 Spring Boot DocumenDB Starter]</span><span class="sxs-lookup"><span data-stu-id="a0e05-185">[Spring Boot DocumenDB Starter for Azure]</span></span>

* [<span data-ttu-id="a0e05-186">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a0e05-186">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="a0e05-187">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="a0e05-187">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="a0e05-188">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="a0e05-188">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="a0e05-189">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0e05-189">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="a0e05-190">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="a0e05-190">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="a0e05-191">為了協助開發人員開始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="a0e05-191">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="a0e05-192">除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0e05-192">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Cosmos DB 文件]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[適用於 Azure Cosmos DB SQL API 的 Spring 資料]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[適用於 Azure 的 Spring Boot DocumenDB Starter]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[Spring Boot DocumenDB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
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
