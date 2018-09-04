---
title: 如何搭配 Azure Cosmos DB SQL API 使用 Spring Data Gremlin Starter
description: 了解如何設定搭配 Azure Cosmos DB SQL API 使用 Spring Boot Initializer 建立的應用程式。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 08/20/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 0976f4b0c13ce5c577458f2974f5dce123bf7e59
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2018
ms.locfileid: "43241130"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a>如何搭配 Azure Cosmos DB SQL API 使用 Spring Data Gremlin Starter

## <a name="overview"></a>概觀

Spring Data Gremlin Starter 可為 Apache 中的 Gremlin 查詢語言提供 Spring Data 支援，讓開發人員可搭配使用任何與 Gremlin 相容的資料存放區。

本文將示範如何使用 Azure 入口網站建立可與 Gremlin API 搭配使用的 Azure Cosmos DB，接著使用 **[Spring Initializr]** 建立自訂的 Java 應用程式，然後將 Spring Data Gremlin Starter 功能新增到您的自訂應用程式，以使用 Gremlin 將資料儲存於您的 Azure Cosmos DB 以及從中擷取資料。

## <a name="prerequisites"></a>必要條件

若要遵循本文中的步驟，需要具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* [Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。

> [!IMPORTANT]
>
> 您需要 Spring Boot 2.0 版或更新版本才能完成本文中的步驟。
>

## <a name="create-an-azure-cosmos-db-using-the-azure-portal"></a>使用 Azure 入口網站建立 Azure Cosmos DB

### <a name="create-your-azure-cosmos-database-for-use-with-gremlin-api"></a>建立 Azure Cosmos Database 以搭配 Gremlin API 使用

1. 瀏覽至 Azure 入口網站 <https://portal.azure.com/> ，然後按一下 **+ 新增資源** 。

   ![建立資源][AZ01]

1. 依序按一下 [資料庫] 和 [Azure Cosmos DB]。

   ![建立 Azure Cosmos DB][AZ02]

1. 在 [Azure Cosmos DB] 頁面上，輸入下列資訊：

   * 輸入唯一的**識別碼**，您將使用此識別碼作為資料庫 Gremlin URI 的一部份。 例如：如果您輸入 **wingtiptoysdata** 作為**識別碼**，則 Gremlin URI 會是 wingtiptoysdata.gremlin.cosmosdb.azure.com。
   * 選擇 [Gremlin (圖表)] 作為 API。
   * 選取您想要用於資料庫的**訂用帳戶**。
   * 指定是否要為資料庫建立新的**資源群組**，或選擇現有的資源群組。
   * 指定資料庫的**位置**。
   
   當您指定這些選項之後，按一下 [建立] 以建立您的資料庫。

   ![指定 Azure Cosmos DB 選項][AZ03]

1. 當您的資料庫建立之後，它會列在您的 Azure [儀表板] 中，以及 [所有資源] 和 [Azure Cosmos DB] 頁面下方。 您可以按一下這些任何位置上的資料庫，來開啟快取的屬性頁面。

   ![所有資源][AZ04]

1. 當資料庫的屬性頁面顯示時，按一下 [存取金鑰]，並複製資料庫的 URI 和存取金鑰；您將會在 Spring Boot 應用程式中使用這些值。

   ![存取金鑰][AZ05]

### <a name="add-a-graph-to-your-azure-cosmos-database"></a>將圖表新增至您的 Azure Cosmos Database

1. 按一下 [資料總管]，然後按一下 [新增圖表]。

   ![新增圖表][AZ06]

1. 當 [新增圖表] 顯示時，輸入下列資訊：

   * 為資料庫指定唯一的**資料庫識別碼**。
   * 為圖表指定唯一的**圖表識別碼**。
   * 您可以選擇指定 [儲存體容量]，或是接受預設值。
   * 指定您的 [輸送量]，在此範例中，您可以選擇 400 個要求單位 (RU)。
   
   當您指定這些選項之後，按一下 [確認] 以建立您的圖表。

   ![新增圖表][AZ07]

1. 建好圖表之後，您可以使用 [資料總管] 來加以檢視。

   ![圖表屬性顯示][AZ08]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 建立簡單的 Spring Boot 應用程式

1. 瀏覽至 <https://start.spring.io/> 。

1. 指定您想要使用 **Java** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱、指定您的 **Spring Boot** 版本 (等於或大於 2.0)，然後按一下 [產生專案]。

   ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：*com.example.wintiptoysdata。
   >

1. 出現提示時，將專案下載至本機電腦上的路徑。

   ![下載自訂的 Spring Boot 專案][SI02]

1. 當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。

   ![自訂的 Spring Boot 專案檔][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a>設定 Spring Boot 應用程式以使用 Spring Data Gremlin Starter

1. 在應用程式的目錄中尋找 *pom.xml* 檔案；例如：

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   -或-

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![尋找 pom.xml 檔案][PM01]

1. 在文字編輯器中開啟 pom.xml 檔案，並將 Spring Data Gremlin Starter 新增至 `<dependencies>` 的清單中：

   ```xml
   <dependency>
      <groupId>com.microsoft.spring.data.gremlin</groupId>
      <artifactId>spring-data-gremlin</artifactId>
      <version>2.0.0</version>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][PM02]

1. 儲存並關閉 *pom.xml* 檔案。

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>設定 Spring Boot 應用程式以使用您的 Azure Cosmos DB

1. 找出應用程式的 [resources] 目錄，並建立名為 application.yml 的新檔案。 例如︰

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.yml`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/resources/application.yml`

   ![建立 application.yml 檔案][RE01]

1.  在文字編輯器中開啟 application.yml 檔案、將下列數行新增至檔案中，然後使用您資料庫的適當屬性來取代範例值：

   ```yaml
   gremlin:
      endpoint: wingtiptoys.gremlin.cosmosdb.azure.com
      port: 443
      username: /dbs/wingtiptoysdb/colls/wingtiptoysgraph
      password: 57686f6120447564652c20426f6220526f636b73==
      telemetryAllowed: false
   ```
   其中：
   | 欄位 | 說明 |
   | ---|---|
   | `endpoint` | 指定您資料庫的 Gremlin URI，其衍生自稍早在本教學課程中建立 Azure Cosmos DB 時指定的**識別碼**。 |
   | `port` | 指定 TCP/IP 連接埠，其應該是用於 HTTPS 的 **443**。 |
   | `username` | 指定唯一**資料庫識別碼**和**圖表識別碼**，也就是您稍早在本教學課程中新增圖表時使用的識別碼；這必須使用下列語法來輸入："/dbs/**{Database id}**/colls/**{Graph id}**"。 |
   | `password` | 指定您稍早在本教學課程中複製的主要或次要**存取金鑰**。 |
   | `telemetryAllowed` | 如果您想要啟用遙測，請指定 [true]；如果不想，請指定 [false]。

1. 儲存並關閉 application.yml 檔案。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>新增範例程式碼來實作基本的資料庫功能

在本節中，您會建立必要的 Java 類別，以便將資料儲存在資料庫中。

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

1. 儲存並關閉主要應用程式 Java 檔案。

### <a name="define-a-basic-class-for-storing-configuration-information"></a>定義用來儲存組態資訊的基本類別

1. 在應用程式的封裝目錄下建立名為 config 的資料夾，例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\config`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/config`

1. 在 config 目錄中新建名為 UserRepositoryConfiguration.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：

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

1. 儲存並關閉 UserRepositoryConfiguration.java 檔案。

### <a name="define-a-set-of-classes-that-define-the-elements-of-your-graph-database"></a>定義一組類別，以定義您的圖表資料庫項目

1. 在應用程式的封裝目錄下建立名為 domain 的資料夾，例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\domain`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/domain`

1. 在 domain 目錄中新建名為 Person.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：

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

1. 儲存並關閉 Person.java 檔案。

1. 在 domain 目錄中新建名為 Relation.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：

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

1. 儲存並關閉 Relation.java 檔案。

1. 在 domain 目錄中新建名為 Network.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：

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

1. 儲存並關閉 Network.java 檔案。

### <a name="define-a-set-of-classes-that-define-the-repositories-for-your-graph-database"></a>定義一組類別，以定義圖表資料庫的存放庫

1. 在應用程式的封裝目錄下建立名為 repository 的資料夾，例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\repository`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/repository`

1. 在 repository 目錄中新建名為 NetworkRepository.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Network;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface NetworkRepository extends GremlinRepository<Network, String> {
   }
   ```

1. 儲存並關閉 NetworkRepository.java 檔案。

1. 在 repository 目錄中新建名為 PersonRepository.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Person;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface PersonRepository extends GremlinRepository<Person, String> {
   }
   ```

1. 儲存並關閉 PersonRepository.java 檔案。

1. 在 repository 目錄中新建名為 RelationRepository.java 的 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Relation;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface RelationRepository extends GremlinRepository<Relation, String> {
   }
   ```

1. 儲存並關閉 RelationRepository.java 檔案。

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

1. 您的應用程式會顯示數個執行階段訊息，如果沒有任何錯誤，可以使用 Azure 入口網站來檢視 Azure Cosmos DB 的內容。 若要這樣做，請在資料庫的屬性頁面上按一下 [資料總管]，然後按一下 [執行 Gremlin 查詢]，然後從結果清單中選取項目以檢視資料。

   ![使用 [文件總管] 檢視資料][JV03]

## <a name="next-steps"></a>後續步驟

若要深入了解適用於 Gremlin 和圖形 API 的 Azure 支援，請參閱下列文章：

* [Azure Cosmos DB：圖形 API 簡介](https://docs.microsoft.com/azure/cosmos-db/graph-introduction)

* [Azure Cosmos DB Gremlin 圖表支援](https://docs.microsoft.com/azure/cosmos-db/gremlin-support)

* [Azure Cosmos DB︰使用 Java 和 Azure 入口網站建立圖表資料庫](https://docs.microsoft.com/azure/cosmos-db/create-graph-java)

* [教學課程：使用 Gremlin 查詢 Azure Cosmos DB 圖形 API](https://docs.microsoft.com/azure/cosmos-db/tutorial-query-graph)

* [Spring Data Gremlin Starter]

如需使用 Azure Cosmos DB 和 Java 的詳細資訊，請參閱下列文章：

* [Azure Cosmos DB 文件]。

* [Azure Cosmos DB︰使用 Java 和 Azure 入口網站建立文件資料庫][Build a SQL API app with Java]

* [適用於 Azure Cosmos DB SQL API 的 Spring 資料]

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式](deploy-spring-boot-java-app-on-kubernetes.md)

如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。

**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。 [Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。 為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。 除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。


<!-- URL List -->

[Azure Cosmos DB 文件]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[適用於 Azure Cosmos DB SQL API 的 Spring 資料]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
