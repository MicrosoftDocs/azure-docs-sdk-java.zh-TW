---
title: "如何搭配 Azure Cosmos DB DocumentDB API 使用 Spring Boot Starter"
description: "了解如何設定搭配 Azure Cosmos DB DocumentDB API 使用 Spring Boot Initializer 建立的應用程式。"
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Spring Starter, Cosmos DB, DocumentDB
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 10/11/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: 32b053d58a721a89e20f4be339ecbb2c5b7f6226
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2017
---
# <a name="how-to-use-the-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="785e3-104">如何搭配 Azure Cosmos DB DocumentDB API 使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="785e3-104">How to use the Spring Boot Starter with Azure Cosmos DB DocumentDB API</span></span>

## <a name="overview"></a><span data-ttu-id="785e3-105">概觀</span><span class="sxs-lookup"><span data-stu-id="785e3-105">Overview</span></span>

<span data-ttu-id="785e3-106">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="785e3-106">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="785e3-107">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="785e3-107">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="785e3-108">為了協助開發人員開始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="785e3-108">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="785e3-109">除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="785e3-109">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="785e3-110">Azure Cosmos DB 是一個橫跨全球的分散式資料庫服務，讓開發人員能夠利用各種不同的標準 API (例如 DocumentDB、MongoDB、圖形和資料表 API) 來使用資料。</span><span class="sxs-lookup"><span data-stu-id="785e3-110">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as DocumentDB, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="785e3-111">Microsoft 的 Spring Boot Starter 讓開發人員能夠使用 Spring Boot 應用程式，藉由使用 DocumentDB API 來輕鬆地與 Azure Cosmos DB 整合。</span><span class="sxs-lookup"><span data-stu-id="785e3-111">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using DocumentDB APIs.</span></span>

<span data-ttu-id="785e3-112">本文將示範如何使用 Azure 入口網站建立 Azure Cosmos DB，接著使用 **Spring Initializr** 建立自訂的 Java 應用程式，然後將 Spring Boot Starter 功能新增到您的自訂應用程式，以使用 DocumentDB API 將資料儲存於您的 Azure Cosmos DB 以及從中擷取資料。</span><span class="sxs-lookup"><span data-stu-id="785e3-112">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **Spring Initializr** to create a custom java application, and then add the Spring Boot Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using the DocumentDB API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="785e3-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="785e3-113">Prerequisites</span></span>

<span data-ttu-id="785e3-114">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="785e3-114">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="785e3-115">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="785e3-115">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="785e3-116">[Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="785e3-116">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="785e3-117">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="785e3-117">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="785e3-118">使用 Azure 入口網站建立 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="785e3-118">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="785e3-119">瀏覽至 <https://portal.azure.com/> 上的 Azure 入口網站，然後按一下 [+新增]。</span><span class="sxs-lookup"><span data-stu-id="785e3-119">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure 入口網站][AZ01]

1. <span data-ttu-id="785e3-121">依序按一下 [資料庫] 和 [Azure Cosmos DB]。</span><span class="sxs-lookup"><span data-stu-id="785e3-121">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure 入口網站][AZ02]

1. <span data-ttu-id="785e3-123">在 [Azure Cosmos DB] 頁面上，輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="785e3-123">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="785e3-124">輸入唯一的**識別碼**，您將使用此識別碼作為資料庫的 URI。</span><span class="sxs-lookup"><span data-stu-id="785e3-124">Enter a unique **ID**, which you will use as the URI for your database.</span></span> <span data-ttu-id="785e3-125">例如：*wingtiptoysdata.documents.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="785e3-125">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="785e3-126">針對 API 選擇 [SQL (Document DB)]。</span><span class="sxs-lookup"><span data-stu-id="785e3-126">Choose **SQL (Document DB)** for the API.</span></span>
   * <span data-ttu-id="785e3-127">選取您想要用於資料庫的**訂用帳戶**。</span><span class="sxs-lookup"><span data-stu-id="785e3-127">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="785e3-128">指定是否要為資料庫建立新的**資源群組**，或選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="785e3-128">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="785e3-129">指定資料庫的**位置**。</span><span class="sxs-lookup"><span data-stu-id="785e3-129">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="785e3-130">當您指定這些選項之後，按一下 [建立] 以建立您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="785e3-130">When you have specified these options, click **Create** to create your database.</span></span>

   ![Azure 入口網站][AZ03]

1. <span data-ttu-id="785e3-132">當您的資料庫建立之後，它會列在您的 Azure [儀表板] 中，以及 [所有資源] 和 [Azure Cosmos DB] 頁面下方。</span><span class="sxs-lookup"><span data-stu-id="785e3-132">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="785e3-133">您可以按一下這些任何位置上的資料庫，來開啟快取的屬性頁面。</span><span class="sxs-lookup"><span data-stu-id="785e3-133">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Azure 入口網站][AZ04]

1. <span data-ttu-id="785e3-135">當資料庫的屬性頁面顯示時，按一下 [存取金鑰]，並複製資料庫的 URI 和存取金鑰；您將會在 Spring Boot 應用程式中使用這些值。</span><span class="sxs-lookup"><span data-stu-id="785e3-135">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure 入口網站][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="785e3-137">使用 Spring Initializr 建立簡單的 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="785e3-137">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="785e3-138">瀏覽至 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="785e3-138">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="785e3-139">指定您想要使用 **Java** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="785e3-139">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the button to **Generate Project**.</span></span>

   ![Basic Spring Initializr 選項][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="785e3-141">Spring Initializr 會使用**群組**和**成品**名稱來建立封裝名稱；例如：*com.example.wintiptoys*。</span><span class="sxs-lookup"><span data-stu-id="785e3-141">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoys*.</span></span>
   >

1. <span data-ttu-id="785e3-142">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="785e3-142">When prompted, download the project to a path on your local computer.</span></span>

   ![下載自訂的 Spring Boot 專案][SI02]

1. <span data-ttu-id="785e3-144">當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="785e3-144">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![自訂的 Spring Boot 專案檔][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="785e3-146">設定 Spring Boot 應用程式以使用 Azure Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="785e3-146">Configure your Spring Boot app to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="785e3-147">在應用程式的目錄中尋找 *pom.xml* 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="785e3-147">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\pom.xml`

   <span data-ttu-id="785e3-148">-或-</span><span class="sxs-lookup"><span data-stu-id="785e3-148">-or-</span></span>

   `/users/example/home/wingtiptoys/pom.xml`

   ![尋找 pom.xml 檔案][PM01]

1. <span data-ttu-id="785e3-150">在文字編輯器中開啟 *pom.xml* 檔案，並將下列數行新增至 `<dependencies>` 的清單中：</span><span class="sxs-lookup"><span data-stu-id="785e3-150">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][PM02]

1. <span data-ttu-id="785e3-152">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="785e3-152">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="785e3-153">設定 Spring Boot 應用程式以使用您的 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="785e3-153">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="785e3-154">在應用程式的 [資源] 目錄中尋找 *application.properties* 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="785e3-154">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   <span data-ttu-id="785e3-155">-或-</span><span class="sxs-lookup"><span data-stu-id="785e3-155">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![尋找 application.properties 檔案][RE01]

1. <span data-ttu-id="785e3-157">在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用您資料庫的適當屬性來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="785e3-157">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![編輯 application.properties 檔案][RE02]

1. <span data-ttu-id="785e3-159">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="785e3-159">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="785e3-160">新增範例程式碼來實作基本的資料庫功能</span><span class="sxs-lookup"><span data-stu-id="785e3-160">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="785e3-161">在本節中，您會建立兩個 Java 類別以供儲存使用者資料，然後修改您的主要應用程式類別，以建立使用者類別的執行個體，並將其儲存至您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="785e3-161">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the user class and save it to your database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="785e3-162">定義用來儲存使用者資料的基本類別</span><span class="sxs-lookup"><span data-stu-id="785e3-162">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="785e3-163">在與主要應用程式 Java 檔案相同的目錄中，建立名為 *User.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="785e3-163">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="785e3-164">在文字編輯器中開啟 *User.java* 檔案，然後下列數行新增至檔案中，以定義泛型使用者類別來儲存和擷取您資料庫中的值：</span><span class="sxs-lookup"><span data-stu-id="785e3-164">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

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

1. <span data-ttu-id="785e3-165">儲存並關閉 *User.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="785e3-165">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="785e3-166">定義資料存放庫介面</span><span class="sxs-lookup"><span data-stu-id="785e3-166">Define a data repository interface</span></span>

1. <span data-ttu-id="785e3-167">在與主要應用程式 Java 檔案相同的目錄中，建立名為 *UserRepository.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="785e3-167">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="785e3-168">在文字編輯器中開啟 *UserRepository.java* 檔案，然後將下列數行新增至檔案中，以定義使用者存放庫介面來延伸預設的 DocumentDB 存放庫介面：</span><span class="sxs-lookup"><span data-stu-id="785e3-168">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. <span data-ttu-id="785e3-169">儲存並關閉 *UserRepository.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="785e3-169">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="785e3-170">修改主要應用程式類別</span><span class="sxs-lookup"><span data-stu-id="785e3-170">Modify the main application class</span></span>

1. <span data-ttu-id="785e3-171">在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="785e3-171">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   <span data-ttu-id="785e3-172">-或-</span><span class="sxs-lookup"><span data-stu-id="785e3-172">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![尋找應用程式 Java 檔案][JV01]

1. <span data-ttu-id="785e3-174">在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：</span><span class="sxs-lookup"><span data-stu-id="785e3-174">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

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

1. <span data-ttu-id="785e3-175">儲存並關閉主要應用程式 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="785e3-175">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="785e3-176">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="785e3-176">Build and test your app</span></span>

1. <span data-ttu-id="785e3-177">開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：</span><span class="sxs-lookup"><span data-stu-id="785e3-177">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoys`

   <span data-ttu-id="785e3-178">-或-</span><span class="sxs-lookup"><span data-stu-id="785e3-178">-or-</span></span>

   `cd /users/example/home/wingtiptoys`

1. <span data-ttu-id="785e3-179">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="785e3-179">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. <span data-ttu-id="785e3-180">您的應用程式將顯示數個執行階段訊息，而您應該會看到 `User: testFirstName testLastName` 這個訊息，表示值已成功儲存並可從您的資料庫擷取。</span><span class="sxs-lookup"><span data-stu-id="785e3-180">Your application will display several runtime messages, and you should see the message `User: testFirstName testLastName` displayed to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ![已成功從應用程式輸出][JV02]

1. <span data-ttu-id="785e3-182">選擇性：您可以使用 Azure 入口網站，從資料庫的屬性頁面中檢視 Azure Cosmos DB 的內容，方法是按一下 [文件總管]，然後從顯示的清單中選取要檢視內容的項目。</span><span class="sxs-lookup"><span data-stu-id="785e3-182">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Document Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![使用 [文件總管] 檢視資料][JV03]

## <a name="next-steps"></a><span data-ttu-id="785e3-184">後續步驟</span><span class="sxs-lookup"><span data-stu-id="785e3-184">Next steps</span></span>

<span data-ttu-id="785e3-185">如需使用 Azure Cosmos DB 和 Java 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="785e3-185">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="785e3-186">[Azure Cosmos DB 文件]。</span><span class="sxs-lookup"><span data-stu-id="785e3-186">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="785e3-187">[Azure Cosmos DB︰使用 Java 和 Azure 入口網站建置 DocumentDB API 應用程式][Build a DocumentDB API app with Java]</span><span class="sxs-lookup"><span data-stu-id="785e3-187">[Azure Cosmos DB: Build a DocumentDB API app with Java and the Azure portal][Build a DocumentDB API app with Java]</span></span>

<span data-ttu-id="785e3-188">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="785e3-188">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="785e3-189">適用於 Azure 的 Spring Boot DocumenDB Starter</span><span class="sxs-lookup"><span data-stu-id="785e3-189">Spring Boot DocumenDB Starter for Azure</span></span>](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [<span data-ttu-id="785e3-190">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="785e3-190">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="785e3-191">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="785e3-191">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="785e3-192">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱 [Azure Java 開發人員中心]和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="785e3-192">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure Cosmos DB 文件]: /azure/cosmos-db/
[Azure Java 開發人員中心]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
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
