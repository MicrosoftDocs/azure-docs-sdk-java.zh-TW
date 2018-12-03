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
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="93945-103">如何搭配 Azure Cosmos DB SQL API 使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="93945-103">How to use the Spring Boot Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="93945-104">概觀</span><span class="sxs-lookup"><span data-stu-id="93945-104">Overview</span></span>

<span data-ttu-id="93945-105">Azure Cosmos DB 是一個橫跨全球的分散式資料庫服務，讓開發人員能夠利用各種不同的標準 API (例如 SQL、MongoDB、圖形和資料表 API) 來使用資料。</span><span class="sxs-lookup"><span data-stu-id="93945-105">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as SQL, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="93945-106">Microsoft 的 Spring Boot Starter 讓開發人員能夠使用 Spring Boot 應用程式，藉由使用 SQL API 來輕鬆地與 Azure Cosmos DB 整合。</span><span class="sxs-lookup"><span data-stu-id="93945-106">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using the SQL API.</span></span>

<span data-ttu-id="93945-107">本文將示範如何使用 Azure 入口網站建立 Azure Cosmos DB，接著使用 **[Spring Initializr]** 建立自訂的 Java 應用程式，然後將 Spring Boot Starter 功能新增到您的自訂應用程式，以使用 SQL API 將資料儲存於您的 Azure Cosmos DB 以及從中擷取資料。</span><span class="sxs-lookup"><span data-stu-id="93945-107">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **[Spring Initializr]** to create a custom java application, and then add the Spring Boot Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using the SQL API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93945-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="93945-108">Prerequisites</span></span>

<span data-ttu-id="93945-109">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="93945-109">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="93945-110">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="93945-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="93945-111">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="93945-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="93945-112">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="93945-112">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="93945-113">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="93945-113">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="93945-114">使用 Azure 入口網站建立 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="93945-114">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="93945-115">瀏覽至 Azure 入口網站 <https://portal.azure.com/> ，然後按一下 **+ 新增資源** 。</span><span class="sxs-lookup"><span data-stu-id="93945-115">Browse to the Azure portal at <https://portal.azure.com/> and click **+Create a resource**.</span></span>

   ![Azure 入口網站][AZ01]

1. <span data-ttu-id="93945-117">依序按一下 [資料庫] 和 [Azure Cosmos DB]。</span><span class="sxs-lookup"><span data-stu-id="93945-117">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure 入口網站][AZ02]

1. <span data-ttu-id="93945-119">在 [Azure Cosmos DB] 頁面上，輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="93945-119">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="93945-120">輸入唯一的**識別碼**，您將使用此識別碼作為資料庫的 URI。</span><span class="sxs-lookup"><span data-stu-id="93945-120">Enter a unique **ID**, which you will use as the URI for your database.</span></span> <span data-ttu-id="93945-121">例如：*wingtiptoysdata.documents.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="93945-121">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="93945-122">為 API 選擇 **SQL**。</span><span class="sxs-lookup"><span data-stu-id="93945-122">Choose **SQL** for the API.</span></span>
   * <span data-ttu-id="93945-123">選取您想要用於資料庫的**訂用帳戶**。</span><span class="sxs-lookup"><span data-stu-id="93945-123">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="93945-124">指定是否要為資料庫建立新的**資源群組**，或選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="93945-124">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="93945-125">指定資料庫的**位置**。</span><span class="sxs-lookup"><span data-stu-id="93945-125">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="93945-126">當您指定這些選項之後，按一下 [建立] 以建立您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="93945-126">When you have specified these options, click **Create** to create your database.</span></span>

   ![Azure 入口網站][AZ03]

1. <span data-ttu-id="93945-128">當您的資料庫建立之後，它會列在您的 Azure [儀表板] 中，以及 [所有資源] 和 [Azure Cosmos DB] 頁面下方。</span><span class="sxs-lookup"><span data-stu-id="93945-128">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="93945-129">您可以按一下這些任何位置上的資料庫，來開啟快取的屬性頁面。</span><span class="sxs-lookup"><span data-stu-id="93945-129">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Azure 入口網站][AZ04]

1. <span data-ttu-id="93945-131">當資料庫的屬性頁面顯示時，按一下 [存取金鑰]，並複製資料庫的 URI 和存取金鑰；您將會在 Spring Boot 應用程式中使用這些值。</span><span class="sxs-lookup"><span data-stu-id="93945-131">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure 入口網站][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="93945-133">使用 Spring Initializr 建立簡單的 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="93945-133">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="93945-134">瀏覽至 <https://start.spring.io/> 。</span><span class="sxs-lookup"><span data-stu-id="93945-134">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="93945-135">指定您想要使用 **Java** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱、指定您的 **Spring Boot** 版本，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="93945-135">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, specify your **Spring Boot** version, and then click the button to **Generate Project**.</span></span>

   > [!IMPORTANT]
   >
   > <span data-ttu-id="93945-136">Spring Boot 版本 2.0.n 中的 API 包含數個重大變更，本文會使用這些變更來完成一些步驟。</span><span class="sxs-lookup"><span data-stu-id="93945-136">There were several breaking changes to the APIs in Spring Boot version 2.0.n, which will be used to complete the steps in this article.</span></span> <span data-ttu-id="93945-137">您仍然可以使用其中一個 Spring Boot 1.5.n 版本來完成本教學課程中的步驟，必要時本文中會標明不同版本的相異之處。</span><span class="sxs-lookup"><span data-stu-id="93945-137">You can still use one of the Spring Boot 1.5.n versions to complete the steps in this tutorial, and the differences will be highlighted when necessary.</span></span>
   >

   ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="93945-139">Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：*com.example.wintiptoys*。</span><span class="sxs-lookup"><span data-stu-id="93945-139">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoysdata*.</span></span>
   >

1. <span data-ttu-id="93945-140">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="93945-140">When prompted, download the project to a path on your local computer.</span></span>

   ![下載自訂的 Spring Boot 專案][SI02]

1. <span data-ttu-id="93945-142">當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="93945-142">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![自訂的 Spring Boot 專案檔][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="93945-144">設定 Spring Boot 應用程式以使用 Azure Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="93945-144">Configure your Spring Boot app to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="93945-145">在應用程式的目錄中尋找 *pom.xml* 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="93945-145">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   <span data-ttu-id="93945-146">-或-</span><span class="sxs-lookup"><span data-stu-id="93945-146">-or-</span></span>

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![尋找 pom.xml 檔案][PM01]

1. <span data-ttu-id="93945-148">在文字編輯器中開啟 *pom.xml* 檔案，並將下列數行新增至 `<dependencies>` 的清單中：</span><span class="sxs-lookup"><span data-stu-id="93945-148">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

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
   > <span data-ttu-id="93945-150">如果您是使用其中一個 Spring Boot 1.5.n 版本完成本教學課程，則需要指定 Azure Cosmos DB Starter 的較舊版本，例如：</span><span class="sxs-lookup"><span data-stu-id="93945-150">If you are using one of Spring Boot 1.5.n versions to complete this tutorial, you will need to specify the older version of the Azure Cosmos DB starter; for example:</span></span>
   >
   > ```xml
   > <dependency>
   >   <groupId>com.microsoft.azure</groupId>
   >   <artifactId>azure-documentdb-spring-boot-starter</artifactId>
   >   <version>0.1.4</version>
   > </dependency>
   > ```

1. <span data-ttu-id="93945-151">請確認該 Spring Boot 版本為您在使用 Spring Initializr 建立應用程式時所選用的版本，例如：</span><span class="sxs-lookup"><span data-stu-id="93945-151">Verify that the Spring Boot version is the version that you chose when you created your application with the Spring Initializr; for example:</span></span>

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
   > <span data-ttu-id="93945-152">如果您使用的是其中一個 Spring Boot 1.5.n 版本完成本教學課程，則需要確認正確的版本，例如：`<version>1.5.14.RELEASE</version>`。</span><span class="sxs-lookup"><span data-stu-id="93945-152">If you are using one of Spring Boot 1.5.n versions to complete this tutorial, you will need to verify the correct version; for example: `<version>1.5.14.RELEASE</version>`.</span></span>
   >

1. <span data-ttu-id="93945-153">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="93945-153">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="93945-154">設定 Spring Boot 應用程式以使用您的 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="93945-154">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="93945-155">在應用程式的 [資源] 目錄中尋找 *application.properties* 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="93945-155">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   <span data-ttu-id="93945-156">-或-</span><span class="sxs-lookup"><span data-stu-id="93945-156">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![尋找 application.properties 檔案][RE01]

1. <span data-ttu-id="93945-158">在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用您資料庫的適當屬性來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="93945-158">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![編輯 application.properties 檔案][RE02]

1. <span data-ttu-id="93945-160">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="93945-160">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="93945-161">新增範例程式碼來實作基本的資料庫功能</span><span class="sxs-lookup"><span data-stu-id="93945-161">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="93945-162">在本節中，您會建立兩個 Java 類別以供儲存使用者資料，然後修改您的主要應用程式類別，以建立使用者類別的執行個體，並將其儲存至您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="93945-162">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the user class and save it to your database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="93945-163">定義用來儲存使用者資料的基本類別</span><span class="sxs-lookup"><span data-stu-id="93945-163">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="93945-164">在與主要應用程式 Java 檔案相同的目錄中，建立名為 *User.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="93945-164">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="93945-165">在文字編輯器中開啟 *User.java* 檔案，然後下列數行新增至檔案中，以定義泛型使用者類別來儲存和擷取您資料庫中的值：</span><span class="sxs-lookup"><span data-stu-id="93945-165">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

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

1. <span data-ttu-id="93945-166">儲存並關閉 *User.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="93945-166">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="93945-167">定義資料存放庫介面</span><span class="sxs-lookup"><span data-stu-id="93945-167">Define a data repository interface</span></span>

1. <span data-ttu-id="93945-168">在與主要應用程式 Java 檔案相同的目錄中，建立名為 *UserRepository.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="93945-168">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="93945-169">在文字編輯器中開啟 *UserRepository.java* 檔案，然後將下列數行新增至檔案中，以定義使用者存放庫介面來延伸預設的 DocumentDB 存放庫介面：</span><span class="sxs-lookup"><span data-stu-id="93945-169">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoysdata;
   
   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> { } 
   ```

1. <span data-ttu-id="93945-170">儲存並關閉 *UserRepository.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="93945-170">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="93945-171">修改主要應用程式類別</span><span class="sxs-lookup"><span data-stu-id="93945-171">Modify the main application class</span></span>

1. <span data-ttu-id="93945-172">在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="93945-172">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   <span data-ttu-id="93945-173">-或-</span><span class="sxs-lookup"><span data-stu-id="93945-173">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![尋找應用程式 Java 檔案][JV01]

1. <span data-ttu-id="93945-175">在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：</span><span class="sxs-lookup"><span data-stu-id="93945-175">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

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
   > <span data-ttu-id="93945-176">如果您使用的是其中一個 Spring Boot 1.5.n 版本完成本教學課程，則需要以 `final User result = repository.findOne(testUser.getId());` 來取代 `final User result = repository.findById(testUser.getId()).get();` 語法。</span><span class="sxs-lookup"><span data-stu-id="93945-176">If you are using one of Spring Boot 1.5.n versions to complete this tutorial, you will need to replace the `final User result = repository.findById(testUser.getId()).get();` syntax with `final User result = repository.findOne(testUser.getId());`.</span></span>
   >

1. <span data-ttu-id="93945-177">儲存並關閉主要應用程式 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="93945-177">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="93945-178">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="93945-178">Build and test your app</span></span>

1. <span data-ttu-id="93945-179">開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：</span><span class="sxs-lookup"><span data-stu-id="93945-179">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoysdata`

   <span data-ttu-id="93945-180">-或-</span><span class="sxs-lookup"><span data-stu-id="93945-180">-or-</span></span>

   `cd /users/example/home/wingtiptoysdata`

1. <span data-ttu-id="93945-181">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="93945-181">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="93945-182">您的應用程式將顯示數個執行階段訊息，並且會顯示如下範例所示的訊息，表示值已成功儲存並可從您的資料庫擷取。</span><span class="sxs-lookup"><span data-stu-id="93945-182">Your application will display several runtime messages, and it will display a message like the following examples to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ```
   User: 20170724025215132 Gena Soto
   ```

   ![已成功從應用程式輸出][JV02]

1. <span data-ttu-id="93945-184">選擇性：您可以使用 Azure 入口網站，從資料庫的屬性頁面中檢視 Azure Cosmos DB 的內容，方法是按一下 [資料總管]，然後從顯示的清單中選取要檢視內容的項目。</span><span class="sxs-lookup"><span data-stu-id="93945-184">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Data Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![使用 [文件總管] 檢視資料][JV03]

## <a name="next-steps"></a><span data-ttu-id="93945-186">後續步驟</span><span class="sxs-lookup"><span data-stu-id="93945-186">Next steps</span></span>

<span data-ttu-id="93945-187">如需使用 Azure Cosmos DB 和 Java 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="93945-187">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="93945-188">[Azure Cosmos DB 文件]。</span><span class="sxs-lookup"><span data-stu-id="93945-188">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="93945-189">[Azure Cosmos DB︰使用 Java 和 Azure 入口網站建立文件資料庫][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="93945-189">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="93945-190">[適用於 Azure Cosmos DB SQL API 的 Spring 資料]</span><span class="sxs-lookup"><span data-stu-id="93945-190">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="93945-191">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="93945-191">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* <span data-ttu-id="93945-192">[適用於 Azure 的 Spring Boot DocumenDB Starter]</span><span class="sxs-lookup"><span data-stu-id="93945-192">[Spring Boot Document DB Starter for Azure]</span></span>

* [<span data-ttu-id="93945-193">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="93945-193">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="93945-194">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="93945-194">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="93945-195">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="93945-195">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="93945-196">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="93945-196">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="93945-197">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="93945-197">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="93945-198">為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="93945-198">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="93945-199">除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="93945-199">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Cosmos DB 文件]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[適用於 Azure Cosmos DB SQL API 的 Spring 資料]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[適用於 Azure 的 Spring Boot DocumenDB Starter]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[適用於 Visual Studio Team Services 的 Java 工具]: https://azure.microsoft.com/services/devops/java/
[Java Tools for Visual Studio Team Services]: https://azure.microsoft.com/services/devops/java/
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
