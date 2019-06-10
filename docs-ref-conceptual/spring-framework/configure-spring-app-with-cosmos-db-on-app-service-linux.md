---
title: 如何在 Linux 上的 App Service 中使用 Spring 和 Cosmos DB
description: 本文將引導您完成在 Linux 上的 Azure App Service 中建置、設定、部署、疑難排解及調整 Java Web 應用程式的程序。
documentationcenter: java
author: bmitchell287
ms.author: brendm; joshuapa
ms.date: 4/24/2019
ms.devlang: java
ms.service: app-service, cosmos-db
ms.topic: article
ms.openlocfilehash: 5d209c0670d6f4265b1237e7b8cff45faa9bb5d8
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568631"
---
# <a name="how-to-use-spring-and-cosmos-db-with-app-service-on-linux"></a>如何在 Linux 上的 App Service 中使用 Spring 和 Cosmos DB

## <a name="overview"></a>概觀

本文將引導您完成在 Linux 上的 Azure App Service 中建置、設定、部署、疑難排解及調整 Java Web 應用程式的程序。

文中將示範如何使用下列元件：

- [Spring Boot Starter 與 Azure Cosmos DB SQL API](configure-spring-boot-starter-java-app-with-cosmos-db.md)
- [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction)
- [App Service Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro)

## <a name="prerequisites"></a>必要條件

若要遵循本文中的步驟，需要具備下列必要條件：

- 若要將 Java Web 應用程式部署到雲端，必須要有 Azure 訂用帳戶。 如果您還沒有 Azure 訂用帳戶，您可以啟用 [MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或註冊[免費的 Azure 帳戶]((https://azure.microsoft.com/pricing/free-trial/))。
- [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Java 8 JDK](https://docs.microsoft.com/java/azure/jdk/java-jdk-install)
- [Maven 3](http://maven.apache.org/)

## <a name="clone-the-sample-java-web-app-repository"></a>複製範例 Java Web 應用程式存放庫
在此練習中，您將使用 Spring Todo 應用程式，這是使用 [Spring Boot](https://spring.io/projects/spring-boot)、[適用於 Cosmos DB 的 Spring 資料](https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable)和 [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction) 建置的 Java 應用程式。
1. 複製 Spring Todo 應用程式，並複製 **.prep** 資料夾的內容以初始化專案：

    若為 Bash：
    ```bash
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    yes | cp -rf .prep/* .
    ```

    若為 Windows：
    ```cmd
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    cd e2e-java-experience-in-app-service-linux-part-2
    xcopy .prep /f /s /e /y
    ```

2. 將目錄切換至複製存放庫中的下列資料夾：

   ```bash
   cd initial\spring-todo-app
   ``` 
## <a name="create-an-azure-cosmos-db-from-azure-cli"></a>從 Azure CLI 建立 Azure Cosmos DB

1. 登入 Azure CLI，並設定您的訂用帳戶識別碼。

    ```bash
    az login
    ```

2. 視需要設定訂用帳戶識別碼。
    ```bash
    az account set -s <your-subscription-id>
    ```

3. 建立 Azure 資源群組，並記下資源群組名稱，以供後續使用。

    ```bash
    az group create -n <your-azure-group-name> \
    -l <your-resource-group-region>
    ```

4. 建立 Cosmos DB，並將類型指定為 GlobalDocumentDB。
Cosmos DB 的名稱只能使用小寫字母。 請務必記下回應中的 `documentEndpoint` 欄位。 稍後您將需要此資訊。

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
     ```

4. 取得您的 Azure Cosmos DB 金鑰，並記錄 `primaryMasterKey` 值供後續使用。

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="build-and-run-the-app-locally"></a>於本機建置並執行應用程式

1. 在您選擇的主控台內，使用您先前在本文中收集的 Azure 與 Cosmos DB 連線資訊，設定下列程式碼區段中顯示的環境變數。 您必須為 **WEBAPP_NAME** 提供唯一的名稱，並提供 **REGION** 變數的值。

針對 Linux (Bash)：

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

針對 Windows (命令提示字元)：
```cmd
set COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
set COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
set COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
set RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
set WEBAPP_NAME=<put-your-Webapp-name-here>
set REGION=<put-your-REGION-here>
```

> [!NOTE]
> 如果您想要使用指令碼佈建這些變數，.prep 目錄中有 Bash 的範本可供您複製並作為起點。

2. 將目錄切換至：

    ```bash
    cd initial/spring-todo-app
    ``` 
 
3. 使用下列命令執行 Spring Todo 應用程式：

    ```bash
    mvn package spring-boot:run
    ```

4. 應用程式啟動之後，您可以在此處存取 Spring Todo 應用程式以驗證部署：[http://localhost:8080/](http://localhost:8080/)。

 ![在本機執行的 Spring 應用程式][SCDB01]

## <a name="deploy-to-app-service-linux"></a>部署到 App Service Linux

1. 開啟您先前複製到存放庫的 **initial/spring-todo-app** 目錄的 pom.xml 檔案。 請確定其中包含[適用於 Azure App Service 的 Maven 外掛程式](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)，如下列 pom.xml 檔案中所示。 如果版本未設為 **1.6.0**，請更新其值。

```xml
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.6.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

2. 部署到 App Service Linux 中的 Java SE

    ```bash
    mvn azure-webapp:deploy
    ```

```bash
// Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.6.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesnt exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlan11111111-1111-1111'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

3. 瀏覽至您在 App Service Linux 中的 Java SE 上執行的 Web 應用程式：

    ```bash
    https://<WEBAPP_NAME>.azurewebsites.net
    ```

![在 Linux 上的 App Service 中執行的 Spring 應用程式][SCDB02]

## <a name="troubleshoot-spring-todo-app-on-azure-by-viewing-logs"></a>藉由檢視記錄對 Azure 上的 Spring Todo 應用程式進行疑難排解

1. 針對 Linux 中的 Azure App Service 已部署的 Java Web 應用程式設定記錄：

    ```bash
    az webapp log config --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME} \
     --web-server-logging filesystem
    ```
2. 從本機電腦開啟 Java Web 應用程式遠端記錄資料流：

    ```bash
    az webapp log tail --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME}
     ```

```bash
bash-3.2$ az webapp log tail --name ${WEBAPP_NAME}  --resource-group ${RESOURCEGROUP_NAME}
2018-10-28T22:50:17  Welcome, you are now connected to log-streaming service.
2018-10-28T22:44:56.265890407Z   _____                               
2018-10-28T22:44:56.265930308Z   /  _  \ __________ _________   ____  
2018-10-28T22:44:56.265936008Z  /  /_\  \___   /  |  \_  __ \_/ __ \ 
2018-10-28T22:44:56.265940308Z /    |    \/    /|  |  /|  | \/\  ___/ 
2018-10-28T22:44:56.265944408Z \____|__  /_____ \____/ |__|    \___  >
2018-10-28T22:44:56.265948508Z         \/      \/                  \/ 
2018-10-28T22:44:56.265952508Z A P P   S E R V I C E   O N   L I N U X
2018-10-28T22:44:56.265956408Z Documentation: http://aka.ms/webapp-linux
2018-10-28T22:44:56.266260910Z Setup openrc ...
2018-10-28T22:44:57.396926506Z Service `hwdrivers needs non existent service dev
2018-10-28T22:44:57.397294409Z  * Caching service dependencies ... [ ok ]
2018-10-28T22:44:57.474152273Z Starting ssh service...
...
...
2018-10-28T22:46:13.432160734Z [INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
2018-10-28T22:46:13.744859424Z [INFO] TomcatWebServer - Tomcat started on port(s): 80 (http) with context path ''
2018-10-28T22:46:13.783230205Z [INFO] TodoApplication - Started TodoApplication in 57.209 seconds (JVM running for 70.815)
2018-10-28T22:46:14.887366993Z 2018-10-28 22:46:14.887  INFO 198 --- [p-nio-80-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-10-28T22:46:14.887637695Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization started
2018-10-28T22:46:14.998479907Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization completed in 111 ms

2018-10-28T22:49:20.572059062Z Sun Oct 28 22:49:20 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:25.850543080Z Sun Oct 28 22:49:25 GMT 2018 DELETE ======= /api/todolist/{4f41ab03-1b12-4131-a920-fe5dfec106ca} ======= 
2018-10-28T22:49:26.047126614Z Sun Oct 28 22:49:26 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:30.201740227Z Sun Oct 28 22:49:30 GMT 2018 POST ======= /api/todolist ======= Milk
2018-10-28T22:49:30.413468872Z Sun Oct 28 22:49:30 GMT 2018 GET ======= /api/todolist =======


```

3. 完成作業後，您可以根據 [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete) 中的程式碼檢查您的結果。


## <a name="scale-out-the-spring-todo-app"></a>相應放大 Spring Todo 應用程式

1. 使用 Azure CLI 相應放大 Java Web 應用程式：

    ```bash
    az appservice plan update --number-of-workers 2 \
      --name ${WEBAPP_PLAN_NAME} \
      --resource-group ${RESOURCEGROUP_NAME}
    ```

## <a name="next-steps"></a>後續步驟

- [App Service Linux 中的 Java 開發人員指南](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-java)
- [適用於 Java 開發人員的 Azure](https://docs.microsoft.com/en-us/java/azure/) 若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他資源

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式](deploy-spring-boot-java-app-on-kubernetes.md)

如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。

**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。 [Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。 為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。 除了從基本的 Spring Boot 專案清單中進行選擇， **[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。

<!-- URL List -->

[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: https://azure.microsoft.com/services/devops/java/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SCDB01]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB01.png
[SCDB02]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB02.png
