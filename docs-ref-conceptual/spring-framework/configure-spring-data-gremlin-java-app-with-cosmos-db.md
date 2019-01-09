---
title: 如何搭配 Azure Cosmos DB SQL API 使用 Spring Data Gremlin Starter
description: 了解如何設定搭配 Azure Cosmos DB SQL API 使用 Spring Boot Initializer 建立的應用程式。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.date: 12/19/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 70bed5696048af1de857f1064bf98e83ab96ca53
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991562"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="1001e-103">如何搭配 Azure Cosmos DB SQL API 使用 Spring Data Gremlin Starter</span><span class="sxs-lookup"><span data-stu-id="1001e-103">How to use the Spring Data Gremlin Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="1001e-104">概觀</span><span class="sxs-lookup"><span data-stu-id="1001e-104">Overview</span></span>

<span data-ttu-id="1001e-105">Spring Data Gremlin Starter 可為 Apache 中的 Gremlin 查詢語言提供 Spring Data 支援，讓開發人員可搭配使用任何與 Gremlin 相容的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="1001e-105">The Spring Data Gremlin Starter provides Spring Data support for the Gremlin query language from Apache, which developers can use with any Gremlin-compatible data store.</span></span>

<span data-ttu-id="1001e-106">本文將示範如何使用 Azure 入口網站建立可與 Gremlin API 搭配使用的 Azure Cosmos DB，接著使用 **[Spring Initializr]** 建立自訂的 Java 應用程式，然後將 Spring Data Gremlin Starter 功能新增到您的自訂應用程式，以使用 Gremlin 將資料儲存於您的 Azure Cosmos DB 以及從中擷取資料。</span><span class="sxs-lookup"><span data-stu-id="1001e-106">This article demonstrates creating an Azure Cosmos DB by using the Azure portal for use with Gremlin API, then using the **[Spring Initializr]** to create a custom java application, and then add the Spring Data Gremlin Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using Gremlin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1001e-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="1001e-107">Prerequisites</span></span>

<span data-ttu-id="1001e-108">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="1001e-108">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="1001e-109">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="1001e-109">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="1001e-110">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="1001e-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="1001e-111">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="1001e-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="1001e-112">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="1001e-112">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="1001e-113">您需要 Spring Boot 2.0 版或更新版本才能完成本文中的步驟。</span><span class="sxs-lookup"><span data-stu-id="1001e-113">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-cosmos-db-using-the-azure-portal"></a><span data-ttu-id="1001e-114">使用 Azure 入口網站建立 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1001e-114">Create an Azure Cosmos DB using the Azure portal</span></span>

### <a name="create-your-azure-cosmos-database-for-use-with-gremlin-api"></a><span data-ttu-id="1001e-115">建立 Azure Cosmos Database 以搭配 Gremlin API 使用</span><span class="sxs-lookup"><span data-stu-id="1001e-115">Create your Azure Cosmos Database for use with Gremlin API</span></span>

1. <span data-ttu-id="1001e-116">瀏覽至 Azure 入口網站 <https://portal.azure.com/> ，然後按一下 **+ 新增資源** 。</span><span class="sxs-lookup"><span data-stu-id="1001e-116">Browse to the Azure portal at <https://portal.azure.com/> and click **+Create a resource**.</span></span>

   ![建立資源][AZ01]

1. <span data-ttu-id="1001e-118">依序按一下 [資料庫] 和 [Azure Cosmos DB]。</span><span class="sxs-lookup"><span data-stu-id="1001e-118">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![建立 Azure Cosmos DB][AZ02]

1. <span data-ttu-id="1001e-120">在 [Azure Cosmos DB] 頁面上，輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="1001e-120">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="1001e-121">輸入唯一的**識別碼**，您將使用此識別碼作為資料庫 Gremlin URI 的一部份。</span><span class="sxs-lookup"><span data-stu-id="1001e-121">Enter a unique **ID**, which you will use as part of the Gremlin URI for your database.</span></span> <span data-ttu-id="1001e-122">例如：如果您輸入 **wingtiptoysdata** 作為**識別碼**，則 Gremlin URI 會是 wingtiptoysdata.gremlin.cosmosdb.azure.com。</span><span class="sxs-lookup"><span data-stu-id="1001e-122">For example: if you entered **wingtiptoysdata** for the **ID**, the Gremlin URI would be *wingtiptoysdata.gremlin.cosmosdb.azure.com*.</span></span>
   * <span data-ttu-id="1001e-123">選擇 [Gremlin (圖表)] 作為 API。</span><span class="sxs-lookup"><span data-stu-id="1001e-123">Choose **Gremlin (Graph)** for the API.</span></span>
   * <span data-ttu-id="1001e-124">選取您想要用於資料庫的**訂用帳戶**。</span><span class="sxs-lookup"><span data-stu-id="1001e-124">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="1001e-125">指定是否要為資料庫建立新的**資源群組**，或選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="1001e-125">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="1001e-126">指定資料庫的**位置**。</span><span class="sxs-lookup"><span data-stu-id="1001e-126">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="1001e-127">當您指定這些選項之後，按一下 [建立] 以建立您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="1001e-127">When you have specified these options, click **Create** to create your database.</span></span>

   ![指定 Azure Cosmos DB 選項][AZ03]

1. <span data-ttu-id="1001e-129">當您的資料庫建立之後，它會列在您的 Azure [儀表板] 中，以及 [所有資源] 和 [Azure Cosmos DB] 頁面下方。</span><span class="sxs-lookup"><span data-stu-id="1001e-129">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="1001e-130">您可以按一下這些任何位置上的資料庫，來開啟快取的屬性頁面。</span><span class="sxs-lookup"><span data-stu-id="1001e-130">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![所有資源][AZ04]

1. <span data-ttu-id="1001e-132">當資料庫的屬性頁面顯示時，按一下 [存取金鑰]，並複製資料庫的 URI 和存取金鑰；您將會在 Spring Boot 應用程式中使用這些值。</span><span class="sxs-lookup"><span data-stu-id="1001e-132">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![存取金鑰][AZ05]

### <a name="add-a-graph-to-your-azure-cosmos-database"></a><span data-ttu-id="1001e-134">將圖表新增至您的 Azure Cosmos Database</span><span class="sxs-lookup"><span data-stu-id="1001e-134">Add a graph to your Azure Cosmos Database</span></span>

1. <span data-ttu-id="1001e-135">按一下 [資料總管]，然後按一下 [新增圖表]。</span><span class="sxs-lookup"><span data-stu-id="1001e-135">Click **Data Explorer**, and then click **New Graph**.</span></span>

   ![新增圖表][AZ06]

1. <span data-ttu-id="1001e-137">當 [新增圖表] 顯示時，輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="1001e-137">When the **Add Graph** is displayed, enter the following information:</span></span>

   * <span data-ttu-id="1001e-138">為資料庫指定唯一的**資料庫識別碼**。</span><span class="sxs-lookup"><span data-stu-id="1001e-138">Specify a unique **Database id** for your database.</span></span>
   * <span data-ttu-id="1001e-139">為圖表指定唯一的**圖表識別碼**。</span><span class="sxs-lookup"><span data-stu-id="1001e-139">Specify a unique **Graph id** for your graph.</span></span>
   * <span data-ttu-id="1001e-140">您可以選擇指定 [儲存體容量]，或是接受預設值。</span><span class="sxs-lookup"><span data-stu-id="1001e-140">You can choose to specify your **Storage capacity**, or you can accept the default.</span></span>
   * <span data-ttu-id="1001e-141">指定您的**輸送量**；在此範例中，您可以選擇 400 個要求單位 (RU)。</span><span class="sxs-lookup"><span data-stu-id="1001e-141">Specify your **Throughput**, and for this example you can choose 400 Request Units (RUs).</span></span>
   
   <span data-ttu-id="1001e-142">當您指定這些選項之後，按一下 [確認] 以建立您的圖表。</span><span class="sxs-lookup"><span data-stu-id="1001e-142">When you have specified these options, click **OK** to create your graph.</span></span>

   ![新增圖表][AZ07]

1. <span data-ttu-id="1001e-144">建好圖表之後，您可以使用 [資料總管] 來加以檢視。</span><span class="sxs-lookup"><span data-stu-id="1001e-144">After your graph has been created, you can use the **Data Explorer** to view it.</span></span>

   ![圖表屬性顯示][AZ08]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="1001e-146">使用 Spring Initializr 建立簡單的 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="1001e-146">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="1001e-147">瀏覽至 <https://start.spring.io/> 。</span><span class="sxs-lookup"><span data-stu-id="1001e-147">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="1001e-148">指定您想要使用 **Java** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱、指定您的 **Spring Boot** 版本 (等於或大於 2.0)，然後按一下 [產生專案]。</span><span class="sxs-lookup"><span data-stu-id="1001e-148">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, specify your **Spring Boot** version with a version that is equal to or greater than 2.0, and then click **Generate Project**.</span></span>

   ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="1001e-150">Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：\*com.example.wintiptoysdata。</span><span class="sxs-lookup"><span data-stu-id="1001e-150">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: \*com.example.wintiptoysdata.</span></span>
   >

1. <span data-ttu-id="1001e-151">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="1001e-151">When prompted, download the project to a path on your local computer.</span></span>

   ![下載自訂的 Spring Boot 專案][SI02]

1. <span data-ttu-id="1001e-153">當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1001e-153">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![自訂的 Spring Boot 專案檔][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a><span data-ttu-id="1001e-155">設定 Spring Boot 應用程式以使用 Spring Data Gremlin Starter</span><span class="sxs-lookup"><span data-stu-id="1001e-155">Configure your Spring Boot app to use the Spring Data Gremlin Starter</span></span>

1. <span data-ttu-id="1001e-156">在應用程式的目錄中尋找 *pom.xml* 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="1001e-156">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   <span data-ttu-id="1001e-157">-或-</span><span class="sxs-lookup"><span data-stu-id="1001e-157">-or-</span></span>

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![尋找 pom.xml 檔案][PM01]

1. <span data-ttu-id="1001e-159">在文字編輯器中開啟 pom.xml 檔案，並將 Spring Data Gremlin Starter 新增至 `<dependencies>` 的清單中：</span><span class="sxs-lookup"><span data-stu-id="1001e-159">Open the *pom.xml* file in a text editor, and add the Spring Data Gremlin Starter to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.spring.data.gremlin</groupId>
      <artifactId>spring-data-gremlin</artifactId>
      <version>2.0.0</version>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][PM02]

1. <span data-ttu-id="1001e-161">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-161">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="1001e-162">設定 Spring Boot 應用程式以使用您的 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1001e-162">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="1001e-163">找出應用程式的 [resources] 目錄，並建立名為 application.yml 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-163">Locate the *resources* directory of your app, and create a new file named *application.yml*.</span></span> <span data-ttu-id="1001e-164">例如︰</span><span class="sxs-lookup"><span data-stu-id="1001e-164">For example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.yml`

   <span data-ttu-id="1001e-165">-或-</span><span class="sxs-lookup"><span data-stu-id="1001e-165">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/resources/application.yml`

   ![建立 application.yml 檔案][RE01]

1. <span data-ttu-id="1001e-167">在文字編輯器中開啟 application.yml 檔案、將下列數行新增至檔案中，然後使用您資料庫的適當屬性來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="1001e-167">Open the *application.yml* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   gremlin:
      endpoint: wingtiptoys.gremlin.cosmosdb.azure.com
      port: 443
      username: /dbs/wingtiptoysdb/colls/wingtiptoysgraph
      password: 57686f6120447564652c20426f6220526f636b73==
      telemetryAllowed: false
   ```
   
   <span data-ttu-id="1001e-168">其中：</span><span class="sxs-lookup"><span data-stu-id="1001e-168">Where:</span></span>
   
   | <span data-ttu-id="1001e-169">欄位</span><span class="sxs-lookup"><span data-stu-id="1001e-169">Field</span></span> | <span data-ttu-id="1001e-170">說明</span><span class="sxs-lookup"><span data-stu-id="1001e-170">Description</span></span> |
   |---|---|
   | `endpoint` | <span data-ttu-id="1001e-171">指定您資料庫的 Gremlin URI，其衍生自稍早在本教學課程中建立 Azure Cosmos DB 時指定的**識別碼**。</span><span class="sxs-lookup"><span data-stu-id="1001e-171">Specifies the Gremlin URI for your database, which is derived from the unique **ID** that you specified when you created your Azure Cosmos DB earlier in this tutorial.</span></span> |
   | `port` | <span data-ttu-id="1001e-172">指定 TCP/IP 連接埠，其應該是用於 HTTPS 的 **443**。</span><span class="sxs-lookup"><span data-stu-id="1001e-172">Specifies the TCP/IP port, which should be **443** for HTTPS.</span></span> |
   | `username` | <span data-ttu-id="1001e-173">指定唯一**資料庫識別碼**和**圖表識別碼**，也就是您稍早在本教學課程中新增圖表時使用的識別碼；這必須使用下列語法來輸入："/dbs/**{Database id}**/colls/**{Graph id}**"。</span><span class="sxs-lookup"><span data-stu-id="1001e-173">Specifies the unique **Database id** and **Graph id** that you used when you added your graph earlier in this tutorial; this must be entered using the following syntax: "/dbs/**{Database id}**/colls/**{Graph id}**".</span></span> |
   | `password` | <span data-ttu-id="1001e-174">指定您稍早在本教學課程中複製的主要或次要**存取金鑰**。</span><span class="sxs-lookup"><span data-stu-id="1001e-174">Specifies either the primary or secondary **Access key** that you copied earlier in this tutorial.</span></span> |
   | `telemetryAllowed` | <span data-ttu-id="1001e-175">如果您想要啟用遙測，請指定 [true]；如果不想，請指定 [false]。</span><span class="sxs-lookup"><span data-stu-id="1001e-175">Specify **true** if you want to enable telemetry; otherwise, **false**.</span></span>

1. <span data-ttu-id="1001e-176">儲存並關閉 application.yml 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-176">Save and close the *application.yml* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="1001e-177">新增範例程式碼來實作基本的資料庫功能</span><span class="sxs-lookup"><span data-stu-id="1001e-177">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="1001e-178">在本節中，您會建立必要的 Java 類別，以便將資料儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="1001e-178">In this section, you create the necessary Java classes for storing data in your database.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="1001e-179">修改主要應用程式類別</span><span class="sxs-lookup"><span data-stu-id="1001e-179">Modify the main application class</span></span>

1. <span data-ttu-id="1001e-180">在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="1001e-180">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   <span data-ttu-id="1001e-181">-或-</span><span class="sxs-lookup"><span data-stu-id="1001e-181">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![尋找應用程式 Java 檔案][JV01]

1. <span data-ttu-id="1001e-183">在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：</span><span class="sxs-lookup"><span data-stu-id="1001e-183">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoysdata;
   
   // These imports are required for the application.
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.example.wingtiptoysdata.domain.Network;
   import com.example.wingtiptoysdata.domain.Person;
   import com.example.wingtiptoysdata.domain.Relation;
   import com.example.wingtiptoysdata.repository.NetworkRepository;
   import com.example.wingtiptoysdata.repository.PersonRepository;
   import com.example.wingtiptoysdata.repository.RelationRepository;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import javax.annotation.PostConstruct;
   import javax.annotation.PreDestroy;
   
   @SpringBootApplication
   public class WingtiptoysdataApplication {
   
       // Define several person classes to store personal data.
       private final Person person1 = new Person("01", "Nellie Hughes", "23");
       private final Person person2 = new Person("02", "Delmar Alfred", "34");
       private final Person person3 = new Person("03", "Kelley Hunter", "45");
       private final Person person4 = new Person("04", "Roscoe Guerin", "56");
       private final Person person5 = new Person("05", "Gracie Chavez", "67");
   
       // Define relationship classes to define the relationships between some of the persons.
       private final Relation relation1 = new Relation("0102", "siblings", person1, person2);
       private final Relation relation2 = new Relation("0203", "coworkers", person2, person3);
       private final Relation relation3 = new Relation("0301", "parent", person3, person1);
       private final Relation relation4 = new Relation("0302", "parent", person3, person2);
       private final Relation relation5 = new Relation("0501", "grandparent", person5, person1);
       private final Relation relation6 = new Relation("0502", "grandparent", person5, person2);
   
       // Define the network.
       private final Network network = new Network();
   
       // Autowire the repositories and factory.
       @Autowired
       private PersonRepository personRepo;
       @Autowired
       private RelationRepository relationRepo;
       @Autowired
       private NetworkRepository networkRepo;
       @Autowired
       private GremlinFactory factory;
   
       // Run the Spring application and exit.
    public static void main(String[] args) {
           SpringApplication.run(WingtiptoysdataApplication.class, args);
           System.exit(0);
    }
   
       // Perform post-construct operations.    
       @PostConstruct
       public void setup() {
           // Delete any existing data from the database.
           this.networkRepo.deleteAll();
   
           // Add the relationship classes as edges.
           this.network.getEdges().add(this.relation1);
           this.network.getEdges().add(this.relation2);
           this.network.getEdges().add(this.relation3);
           this.network.getEdges().add(this.relation4);
           this.network.getEdges().add(this.relation5);
           this.network.getEdges().add(this.relation6);
   
           // Add the person classes as vertices.
           this.network.getVertexes().add(this.person1);
           this.network.getVertexes().add(this.person2);
           this.network.getVertexes().add(this.person3);
           this.network.getVertexes().add(this.person4);
           this.network.getVertexes().add(this.person5);
   
           // Save the network.
           this.networkRepo.save(this.network);
       }
   }
   ```

1. <span data-ttu-id="1001e-184">儲存並關閉主要應用程式 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-184">Save and close the main application Java file.</span></span>

### <a name="define-a-basic-class-for-storing-configuration-information"></a><span data-ttu-id="1001e-185">定義用來儲存組態資訊的基本類別</span><span class="sxs-lookup"><span data-stu-id="1001e-185">Define a basic class for storing configuration information</span></span>

1. <span data-ttu-id="1001e-186">在應用程式的封裝目錄下建立名為 config 的資料夾，例如：</span><span class="sxs-lookup"><span data-stu-id="1001e-186">Create a folder named *config* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\config`

   <span data-ttu-id="1001e-187">-或-</span><span class="sxs-lookup"><span data-stu-id="1001e-187">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/config`

1. <span data-ttu-id="1001e-188">在 config 目錄中新建名為 UserRepositoryConfiguration.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：</span><span class="sxs-lookup"><span data-stu-id="1001e-188">Create a new Java file named *UserRepositoryConfiguration.java* in the *config* directory, then open the file in a text editor, and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.config;

   import com.microsoft.spring.data.gremlin.common.GremlinConfiguration;
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.microsoft.spring.data.gremlin.config.AbstractGremlinConfiguration;
   import com.microsoft.spring.data.gremlin.repository.config.EnableGremlinRepositories;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.context.properties.EnableConfigurationProperties;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.PropertySource;
   
   @Configuration
   @EnableGremlinRepositories(basePackages = "com.example.wingtiptoysdata.repository")
   @EnableConfigurationProperties(GremlinConfiguration.class)
   @PropertySource("classpath:application.yml")
   public class UserRepositoryConfiguration extends AbstractGremlinConfiguration {
   
       @Autowired
       private GremlinConfiguration config;
   
       @Override
       public GremlinConfiguration getGremlinConfiguration() {
           return this.config;
       }
   }
   ```

1. <span data-ttu-id="1001e-189">儲存並關閉 UserRepositoryConfiguration.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-189">Save and close the *UserRepositoryConfiguration.java* file.</span></span>

### <a name="define-a-set-of-classes-that-define-the-elements-of-your-graph-database"></a><span data-ttu-id="1001e-190">定義一組類別，以定義您的圖表資料庫項目</span><span class="sxs-lookup"><span data-stu-id="1001e-190">Define a set of classes that define the elements of your graph database</span></span>

1. <span data-ttu-id="1001e-191">在應用程式的封裝目錄下建立名為 domain 的資料夾，例如：</span><span class="sxs-lookup"><span data-stu-id="1001e-191">Create a folder named *domain* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\domain`

   <span data-ttu-id="1001e-192">-或-</span><span class="sxs-lookup"><span data-stu-id="1001e-192">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/domain`

1. <span data-ttu-id="1001e-193">在 domain 目錄中新建名為 Person.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：</span><span class="sxs-lookup"><span data-stu-id="1001e-193">Create a new Java file named *Person.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Vertex;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Vertex
   @AllArgsConstructor
   @NoArgsConstructor
   public class Person {
   
       @Id
       private String id;
   
       private String name;
   
       private String age;
   }
   ```

1. <span data-ttu-id="1001e-194">儲存並關閉 Person.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-194">Save and close the *Person.java* file.</span></span>

1. <span data-ttu-id="1001e-195">在 domain 目錄中新建名為 Relation.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：</span><span class="sxs-lookup"><span data-stu-id="1001e-195">Create a new Java file named *Relation.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Edge;
   import com.microsoft.spring.data.gremlin.annotation.EdgeFrom;
   import com.microsoft.spring.data.gremlin.annotation.EdgeTo;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Edge
   @AllArgsConstructor
   @NoArgsConstructor
   public class Relation {
   
       @Id
       private String id;
   
       private String name;
   
       @EdgeFrom
       private Person personFrom;
   
       @EdgeTo
       private Person personTo;
   }
   ```

1. <span data-ttu-id="1001e-196">儲存並關閉 Relation.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-196">Save and close the *Relation.java* file.</span></span>

1. <span data-ttu-id="1001e-197">在 domain 目錄中新建名為 Network.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：</span><span class="sxs-lookup"><span data-stu-id="1001e-197">Create a new Java file named *Network.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.EdgeSet;
   import com.microsoft.spring.data.gremlin.annotation.Graph;
   import com.microsoft.spring.data.gremlin.annotation.VertexSet;
   import lombok.Getter;
   import org.springframework.data.annotation.Id;
   
   import java.util.ArrayList;
   import java.util.List;
   
   @Graph
   public class Network {
   
       @Id
       private String id;
   
       public Network() {
           this.edges = new ArrayList<Object>();
           this.vertexes = new ArrayList<Object>();
       }
   
       @EdgeSet
       @Getter
       private List<Object> edges;
   
       @VertexSet
       @Getter
       private List<Object> vertexes;
   }
   ```

1. <span data-ttu-id="1001e-198">儲存並關閉 Network.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-198">Save and close the *Network.java* file.</span></span>

### <a name="define-a-set-of-classes-that-define-the-repositories-for-your-graph-database"></a><span data-ttu-id="1001e-199">定義一組類別，以定義圖表資料庫的存放庫</span><span class="sxs-lookup"><span data-stu-id="1001e-199">Define a set of classes that define the repositories for your graph database</span></span>

1. <span data-ttu-id="1001e-200">在應用程式的封裝目錄下建立名為 repository 的資料夾，例如：</span><span class="sxs-lookup"><span data-stu-id="1001e-200">Create a folder named *repository* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\repository`

   <span data-ttu-id="1001e-201">-或-</span><span class="sxs-lookup"><span data-stu-id="1001e-201">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/repository`

1. <span data-ttu-id="1001e-202">在 repository 目錄中新建名為 NetworkRepository.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：</span><span class="sxs-lookup"><span data-stu-id="1001e-202">Create a new Java file named *NetworkRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Network;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface NetworkRepository extends GremlinRepository<Network, String> {
   }
   ```

1. <span data-ttu-id="1001e-203">儲存並關閉 NetworkRepository.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-203">Save and close the *NetworkRepository.java* file.</span></span>

1. <span data-ttu-id="1001e-204">在 repository 目錄中新建名為 PersonRepository.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：</span><span class="sxs-lookup"><span data-stu-id="1001e-204">Create a new Java file named *PersonRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Person;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface PersonRepository extends GremlinRepository<Person, String> {
   }
   ```

1. <span data-ttu-id="1001e-205">儲存並關閉 PersonRepository.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-205">Save and close the *PersonRepository.java* file.</span></span>

1. <span data-ttu-id="1001e-206">在 repository 目錄中新建名為 RelationRepository.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：</span><span class="sxs-lookup"><span data-stu-id="1001e-206">Create a new Java file named *RelationRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Relation;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface RelationRepository extends GremlinRepository<Relation, String> {
   }
   ```

1. <span data-ttu-id="1001e-207">儲存並關閉 RelationRepository.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="1001e-207">Save and close the *RelationRepository.java* file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="1001e-208">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="1001e-208">Build and test your app</span></span>

1. <span data-ttu-id="1001e-209">開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：</span><span class="sxs-lookup"><span data-stu-id="1001e-209">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoysdata`

   <span data-ttu-id="1001e-210">-或-</span><span class="sxs-lookup"><span data-stu-id="1001e-210">-or-</span></span>

   `cd /users/example/home/wingtiptoysdata`

1. <span data-ttu-id="1001e-211">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="1001e-211">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="1001e-212">您的應用程式會顯示數個執行階段訊息，如果沒有任何錯誤，可以使用 Azure 入口網站來檢視 Azure Cosmos DB 的內容。</span><span class="sxs-lookup"><span data-stu-id="1001e-212">Your application will display several runtime messages, and if there were no errors, you can use the Azure portal to view the contents of your Azure Cosmos DB.</span></span> <span data-ttu-id="1001e-213">若要這樣做，請在資料庫的屬性頁面上按一下 [資料總管]，然後按一下 [執行 Gremlin 查詢]，然後從結果清單中選取項目以檢視資料。</span><span class="sxs-lookup"><span data-stu-id="1001e-213">To do so, click **Data Explorer** on the properties page for your database, then click **Execute Gremlin Query**, and then select an item from the list of results to view the data.</span></span>

   ![使用 [文件總管] 檢視資料][JV03]

## <a name="next-steps"></a><span data-ttu-id="1001e-215">後續步驟</span><span class="sxs-lookup"><span data-stu-id="1001e-215">Next steps</span></span>

<span data-ttu-id="1001e-216">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="1001e-216">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1001e-217">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="1001e-217">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="1001e-218">其他資源</span><span class="sxs-lookup"><span data-stu-id="1001e-218">Additional Resources</span></span>

<span data-ttu-id="1001e-219">若要深入了解適用於 Gremlin 和圖形 API 的 Azure 支援，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="1001e-219">For more information about Azure support for Gremlin and Graph API, see the following articles:</span></span>

* [<span data-ttu-id="1001e-220">Azure Cosmos DB：圖形 API 簡介</span><span class="sxs-lookup"><span data-stu-id="1001e-220">Introduction to Azure Cosmos DB: Graph API</span></span>](/azure/cosmos-db/graph-introduction)

* [<span data-ttu-id="1001e-221">Azure Cosmos DB Gremlin 圖表支援</span><span class="sxs-lookup"><span data-stu-id="1001e-221">Azure Cosmos DB Gremlin graph support</span></span>](/azure/cosmos-db/gremlin-support)

* [<span data-ttu-id="1001e-222">Azure Cosmos DB：使用 Java 和 Azure 入口網站建立圖形資料庫</span><span class="sxs-lookup"><span data-stu-id="1001e-222">Azure Cosmos DB: Create a graph database using Java and the Azure portal</span></span>](/azure/cosmos-db/create-graph-java)

* [<span data-ttu-id="1001e-223">教學課程：使用 Gremlin 查詢 Azure Cosmos DB 圖形 API</span><span class="sxs-lookup"><span data-stu-id="1001e-223">Tutorial: Query Azure Cosmos DB Graph API by using Gremlin</span></span>](/azure/cosmos-db/tutorial-query-graph)

* <span data-ttu-id="1001e-224">[Spring Data Gremlin Starter]</span><span class="sxs-lookup"><span data-stu-id="1001e-224">[Spring Data Gremlin Starter]</span></span>

<span data-ttu-id="1001e-225">如需使用 Azure Cosmos DB 和 Java 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="1001e-225">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="1001e-226">[Azure Cosmos DB 文件]。</span><span class="sxs-lookup"><span data-stu-id="1001e-226">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="1001e-227">[Azure Cosmos DB：使用 Java 和 Azure 入口網站建立文件資料庫][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="1001e-227">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="1001e-228">[適用於 Azure Cosmos DB SQL API 的 Spring 資料]</span><span class="sxs-lookup"><span data-stu-id="1001e-228">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="1001e-229">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="1001e-229">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="1001e-230">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1001e-230">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="1001e-231">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="1001e-231">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="1001e-232">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="1001e-232">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="1001e-233">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="1001e-233">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="1001e-234">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="1001e-234">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="1001e-235">為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="1001e-235">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="1001e-236">除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1001e-236">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Cosmos DB 文件]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[適用於 Azure Cosmos DB SQL API 的 Spring 資料]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ05.png
[AZ06]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ06.png
[AZ07]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ07.png
[AZ08]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ08.png

[SI01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV03.png
