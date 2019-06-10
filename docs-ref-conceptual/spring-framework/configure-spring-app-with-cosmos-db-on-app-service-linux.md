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
# <a name="how-to-use-spring-and-cosmos-db-with-app-service-on-linux"></a><span data-ttu-id="c156e-103">如何在 Linux 上的 App Service 中使用 Spring 和 Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c156e-103">How to use Spring and Cosmos DB with App Service on Linux</span></span>

## <a name="overview"></a><span data-ttu-id="c156e-104">概觀</span><span class="sxs-lookup"><span data-stu-id="c156e-104">Overview</span></span>

<span data-ttu-id="c156e-105">本文將引導您完成在 Linux 上的 Azure App Service 中建置、設定、部署、疑難排解及調整 Java Web 應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="c156e-105">This article will walk you through the process of building, configuring, deploying, troubleshooting, and scaling Java Web apps in Azure App Service on Linux.</span></span>

<span data-ttu-id="c156e-106">文中將示範如何使用下列元件：</span><span class="sxs-lookup"><span data-stu-id="c156e-106">It will demonstrate the usage of the following components:</span></span>

- [<span data-ttu-id="c156e-107">Spring Boot Starter 與 Azure Cosmos DB SQL API</span><span class="sxs-lookup"><span data-stu-id="c156e-107">Spring Boot Starter with the Azure Cosmos DB SQL API</span></span>](configure-spring-boot-starter-java-app-with-cosmos-db.md)
- [<span data-ttu-id="c156e-108">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c156e-108">Azure Cosmos DB</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction)
- [<span data-ttu-id="c156e-109">App Service Linux</span><span class="sxs-lookup"><span data-stu-id="c156e-109">App Service Linux</span></span>](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro)

## <a name="prerequisites"></a><span data-ttu-id="c156e-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="c156e-110">Prerequisites</span></span>

<span data-ttu-id="c156e-111">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="c156e-111">The following prerequisites are required in order to follow the steps in this article:</span></span>

- <span data-ttu-id="c156e-112">若要將 Java Web 應用程式部署到雲端，必須要有 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="c156e-112">In order to deploy a Java Web app to cloud, you need an Azure subscription.</span></span> <span data-ttu-id="c156e-113">如果您還沒有 Azure 訂用帳戶，您可以啟用 [MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或註冊[免費的 Azure 帳戶]((https://azure.microsoft.com/pricing/free-trial/))。</span><span class="sxs-lookup"><span data-stu-id="c156e-113">If you do not already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account]((https://azure.microsoft.com/pricing/free-trial/)).</span></span>
- [<span data-ttu-id="c156e-114">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c156e-114">Azure CLI 2.0</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- [<span data-ttu-id="c156e-115">Java 8 JDK</span><span class="sxs-lookup"><span data-stu-id="c156e-115">Java 8 JDK</span></span>](https://docs.microsoft.com/java/azure/jdk/java-jdk-install)
- [<span data-ttu-id="c156e-116">Maven 3</span><span class="sxs-lookup"><span data-stu-id="c156e-116">Maven 3</span></span>](http://maven.apache.org/)

## <a name="clone-the-sample-java-web-app-repository"></a><span data-ttu-id="c156e-117">複製範例 Java Web 應用程式存放庫</span><span class="sxs-lookup"><span data-stu-id="c156e-117">Clone the Sample Java Web App Repository</span></span>
<span data-ttu-id="c156e-118">在此練習中，您將使用 Spring Todo 應用程式，這是使用 [Spring Boot](https://spring.io/projects/spring-boot)、[適用於 Cosmos DB 的 Spring 資料](https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable)和 [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction) 建置的 Java 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c156e-118">For this exercise you'll be using the Spring Todo app, which is a Java application built using [Spring Boot](https://spring.io/projects/spring-boot), [Spring Data for Cosmos DB](https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable) and [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction).</span></span>
1. <span data-ttu-id="c156e-119">複製 Spring Todo 應用程式，並複製 **.prep** 資料夾的內容以初始化專案：</span><span class="sxs-lookup"><span data-stu-id="c156e-119">Clone the Spring Todo app and copy the contents of the **.prep** folder to initialize the project:</span></span>

    <span data-ttu-id="c156e-120">若為 Bash：</span><span class="sxs-lookup"><span data-stu-id="c156e-120">For bash:</span></span>
    ```bash
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    yes | cp -rf .prep/* .
    ```

    <span data-ttu-id="c156e-121">若為 Windows：</span><span class="sxs-lookup"><span data-stu-id="c156e-121">For Windows:</span></span>
    ```cmd
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    cd e2e-java-experience-in-app-service-linux-part-2
    xcopy .prep /f /s /e /y
    ```

2. <span data-ttu-id="c156e-122">將目錄切換至複製存放庫中的下列資料夾：</span><span class="sxs-lookup"><span data-stu-id="c156e-122">Change the directory to the following folder in the cloned repo:</span></span>

   ```bash
   cd initial\spring-todo-app
   ``` 
## <a name="create-an-azure-cosmos-db-from-azure-cli"></a><span data-ttu-id="c156e-123">從 Azure CLI 建立 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c156e-123">Create an Azure Cosmos DB from Azure CLI</span></span>

1. <span data-ttu-id="c156e-124">登入 Azure CLI，並設定您的訂用帳戶識別碼。</span><span class="sxs-lookup"><span data-stu-id="c156e-124">Login to your Azure CLI, and set your subscription id.</span></span>

    ```bash
    az login
    ```

2. <span data-ttu-id="c156e-125">視需要設定訂用帳戶識別碼。</span><span class="sxs-lookup"><span data-stu-id="c156e-125">Set the subscription id if needed.</span></span>
    ```bash
    az account set -s <your-subscription-id>
    ```

3. <span data-ttu-id="c156e-126">建立 Azure 資源群組，並記下資源群組名稱，以供後續使用。</span><span class="sxs-lookup"><span data-stu-id="c156e-126">Create an Azure resource group, and write down the resource group name for later use.</span></span>

    ```bash
    az group create -n <your-azure-group-name> \
    -l <your-resource-group-region>
    ```

4. <span data-ttu-id="c156e-127">建立 Cosmos DB，並將類型指定為 GlobalDocumentDB。</span><span class="sxs-lookup"><span data-stu-id="c156e-127">Create the Cosmos DB and specify the type as GlobalDocumentDB.</span></span>
<span data-ttu-id="c156e-128">Cosmos DB 的名稱只能使用小寫字母。</span><span class="sxs-lookup"><span data-stu-id="c156e-128">The name of the Cosmos DB must use only lower case letters.</span></span> <span data-ttu-id="c156e-129">請務必記下回應中的 `documentEndpoint` 欄位。</span><span class="sxs-lookup"><span data-stu-id="c156e-129">Make sure to note the `documentEndpoint` field in the response.</span></span> <span data-ttu-id="c156e-130">稍後您將需要此資訊。</span><span class="sxs-lookup"><span data-stu-id="c156e-130">You'll need this later.</span></span>

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
     ```

4. <span data-ttu-id="c156e-131">取得您的 Azure Cosmos DB 金鑰，並記錄 `primaryMasterKey` 值供後續使用。</span><span class="sxs-lookup"><span data-stu-id="c156e-131">Get your Azure Cosmos DB keys, record the `primaryMasterKey` value for later use.</span></span>

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="build-and-run-the-app-locally"></a><span data-ttu-id="c156e-132">於本機建置並執行應用程式</span><span class="sxs-lookup"><span data-stu-id="c156e-132">Build and Run the App Locally</span></span>

1. <span data-ttu-id="c156e-133">在您選擇的主控台內，使用您先前在本文中收集的 Azure 與 Cosmos DB 連線資訊，設定下列程式碼區段中顯示的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c156e-133">Within your console of choice configure the environment variables shown in the following code sections with the Azure and Cosmos DB connection information you gathered previously in this article.</span></span> <span data-ttu-id="c156e-134">您必須為 **WEBAPP_NAME** 提供唯一的名稱，並提供 **REGION** 變數的值。</span><span class="sxs-lookup"><span data-stu-id="c156e-134">You'll need to provide a unique name for **WEBAPP_NAME** and value for the **REGION** variables.</span></span>

<span data-ttu-id="c156e-135">針對 Linux (Bash)：</span><span class="sxs-lookup"><span data-stu-id="c156e-135">For Linux (Bash):</span></span>

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

<span data-ttu-id="c156e-136">針對 Windows (命令提示字元)：</span><span class="sxs-lookup"><span data-stu-id="c156e-136">For Windows (Command Prompt):</span></span>
```cmd
set COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
set COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
set COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
set RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
set WEBAPP_NAME=<put-your-Webapp-name-here>
set REGION=<put-your-REGION-here>
```

> [!NOTE]
> <span data-ttu-id="c156e-137">如果您想要使用指令碼佈建這些變數，.prep 目錄中有 Bash 的範本可供您複製並作為起點。</span><span class="sxs-lookup"><span data-stu-id="c156e-137">If you'd like to provision these variables with a script, there is a template for Bash in the .prep directory that you can copy and use as a starting point.</span></span>

2. <span data-ttu-id="c156e-138">將目錄切換至：</span><span class="sxs-lookup"><span data-stu-id="c156e-138">Change the directory to the following:</span></span>

    ```bash
    cd initial/spring-todo-app
    ``` 
 
3. <span data-ttu-id="c156e-139">使用下列命令執行 Spring Todo 應用程式：</span><span class="sxs-lookup"><span data-stu-id="c156e-139">Run the Spring Todo app locally with the following command:</span></span>

    ```bash
    mvn package spring-boot:run
    ```

4. <span data-ttu-id="c156e-140">應用程式啟動之後，您可以在此處存取 Spring Todo 應用程式以驗證部署：[http://localhost:8080/](http://localhost:8080/)。</span><span class="sxs-lookup"><span data-stu-id="c156e-140">Once the application has started,you can validate the deployment by accessing the Spring Todo app here: [http://localhost:8080/](http://localhost:8080/).</span></span>

 ![在本機執行的 Spring 應用程式][SCDB01]

## <a name="deploy-to-app-service-linux"></a><span data-ttu-id="c156e-142">部署到 App Service Linux</span><span class="sxs-lookup"><span data-stu-id="c156e-142">Deploy to App Service Linux</span></span>

1. <span data-ttu-id="c156e-143">開啟您先前複製到存放庫的 **initial/spring-todo-app** 目錄的 pom.xml 檔案。</span><span class="sxs-lookup"><span data-stu-id="c156e-143">Open the pom.xml file that you previously copied to the **initial/spring-todo-app** directory of the repository.</span></span> <span data-ttu-id="c156e-144">請確定其中包含[適用於 Azure App Service 的 Maven 外掛程式](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)，如下列 pom.xml 檔案中所示。</span><span class="sxs-lookup"><span data-stu-id="c156e-144">Ensure that the [Maven Plugin for Azure App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) is included as seen in the pom.xml file below.</span></span> <span data-ttu-id="c156e-145">如果版本未設為 **1.6.0**，請更新其值。</span><span class="sxs-lookup"><span data-stu-id="c156e-145">If the version is not set to **1.6.0** then update the value.</span></span>

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

2. <span data-ttu-id="c156e-146">部署到 App Service Linux 中的 Java SE</span><span class="sxs-lookup"><span data-stu-id="c156e-146">Deploy to Java SE in App Service Linux</span></span>

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

3. <span data-ttu-id="c156e-147">瀏覽至您在 App Service Linux 中的 Java SE 上執行的 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="c156e-147">Browse to your web app running on Java SE in App Service Linux:</span></span>

    ```bash
    https://<WEBAPP_NAME>.azurewebsites.net
    ```

![在 Linux 上的 App Service 中執行的 Spring 應用程式][SCDB02]

## <a name="troubleshoot-spring-todo-app-on-azure-by-viewing-logs"></a><span data-ttu-id="c156e-149">藉由檢視記錄對 Azure 上的 Spring Todo 應用程式進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="c156e-149">Troubleshoot Spring Todo App on Azure by Viewing Logs</span></span>

1. <span data-ttu-id="c156e-150">針對 Linux 中的 Azure App Service 已部署的 Java Web 應用程式設定記錄：</span><span class="sxs-lookup"><span data-stu-id="c156e-150">Configure logs for the deployed Java Web app in Azure App Service in Linux:</span></span>

    ```bash
    az webapp log config --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME} \
     --web-server-logging filesystem
    ```
2. <span data-ttu-id="c156e-151">從本機電腦開啟 Java Web 應用程式遠端記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="c156e-151">Open Java Web app remote log stream from a local machine:</span></span>

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

3. <span data-ttu-id="c156e-152">完成作業後，您可以根據 [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete) 中的程式碼檢查您的結果。</span><span class="sxs-lookup"><span data-stu-id="c156e-152">When you are finished, you can check your results against the code in [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete).</span></span>


## <a name="scale-out-the-spring-todo-app"></a><span data-ttu-id="c156e-153">相應放大 Spring Todo 應用程式</span><span class="sxs-lookup"><span data-stu-id="c156e-153">Scale out the Spring Todo App</span></span>

1. <span data-ttu-id="c156e-154">使用 Azure CLI 相應放大 Java Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="c156e-154">Scale out Java Web app using Azure CLI:</span></span>

    ```bash
    az appservice plan update --number-of-workers 2 \
      --name ${WEBAPP_PLAN_NAME} \
      --resource-group ${RESOURCEGROUP_NAME}
    ```

## <a name="next-steps"></a><span data-ttu-id="c156e-155">後續步驟</span><span class="sxs-lookup"><span data-stu-id="c156e-155">Next steps</span></span>

- [<span data-ttu-id="c156e-156">App Service Linux 中的 Java 開發人員指南</span><span class="sxs-lookup"><span data-stu-id="c156e-156">Java in App Service Linux dev guide</span></span>](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-java)
- <span data-ttu-id="c156e-157">[適用於 Java 開發人員的 Azure](https://docs.microsoft.com/en-us/java/azure/) 若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心的資訊。</span><span class="sxs-lookup"><span data-stu-id="c156e-157">[Azure for Java Developers](https://docs.microsoft.com/en-us/java/azure/) To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c156e-158">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="c156e-158">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="c156e-159">其他資源</span><span class="sxs-lookup"><span data-stu-id="c156e-159">Additional Resources</span></span>

<span data-ttu-id="c156e-160">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="c156e-160">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="c156e-161">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c156e-161">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="c156e-162">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="c156e-162">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="c156e-163">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="c156e-163">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="c156e-164">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="c156e-164">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="c156e-165">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="c156e-165">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="c156e-166">為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="c156e-166">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="c156e-167">除了從基本的 Spring Boot 專案清單中進行選擇， **[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c156e-167">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: https://azure.microsoft.com/services/devops/java/
[Working with Azure DevOps and Java]: https://azure.microsoft.com/services/devops/java/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SCDB01]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB01.png
[SCDB02]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB02.png
