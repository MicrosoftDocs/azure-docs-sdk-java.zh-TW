---
title: "使用 Java 來設定 webapp 部署 | Microsoft Docs"
description: "使用 Azure SDK for Java 來設定 Git 或 FTP Azure App Service 部署的 Java 程式碼範例"
author: rloutlaw
manager: douge
ms.assetid: 833e9c78-1e50-4c23-a611-f73a2f0c2983
ms.service: Azure
ms.devlang: java
ms.topic: article
ms.date: 03/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 910d1e43c9942d6402aeccb8757ba819b7453dab
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="configure-azure-app-service-deployment-sources-from-your-java-applications"></a>從 Java 應用程式設定 Azure App Service 部署來源

[此範例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)會將程式碼部署到位於單一 [Azure App Service](https://docs.microsoft.com/azure/app-service/) 方案的四個應用程式，且每個應用程式各自使用不同的部署來源。

## <a name="run-the-sample"></a>執行範例

建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。 然後，執行：

```
git clone https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps.git
cd app-service-java-configure-deployment-sources-for-web-apps
mvn clean compile exec:java
```

檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 進行驗證

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-app-service-app-running-apache-tomcat"></a>建立執行 Apache Tomcat 的 App Service 應用程式

```java
// create a new Standard app service plan and create a single Java 8/Tomcat 8 app in it
WebApp app1 = azure.webApps().define(app1Name)
             .withNewResourceGroup(rgName)
             .withNewAppServicePlan(planName)
             .withRegion(Region.US_WEST)
             .withPricingTier(AppServicePricingTier.STANDARD_S1)
             .withJavaVersion(JavaVersion.JAVA_8_NEWEST)
             .withWebContainer(WebContainer.TOMCAT_8_0_NEWEST)
             .create();
```

`withJavaVersion()` 和 `withWebContainer()` 會將 App Service 設定為使用 Tomcat 8 來提供 HTTP 要求。

## <a name="deploy-a-java-application-using-ftp"></a>使用 FTP 部署 Java 應用程式
```java
// pass the PublishingProfile that contains FTP information to a helper method 
uploadFileToFtp(app1.getPublishingProfile(), "helloworld.war", 
      ManageWebAppSourceControl.class.getResourceAsStream("/helloworld.war"));

// Use the FTP classes in the Apache Commons library to connect to Azure using 
// the information from the PublishingProfile
private static void uploadFileToFtp(PublishingProfile profile, String fileName, InputStream file) throws Exception {
        FTPClient ftpClient = new FTPClient();
        String[] ftpUrlSegments = profile.ftpUrl().split("/", 2);
        String server = ftpUrlSegments[0];
        // Tomcat will deploy WAR files uploaded to this directory.
        String path = "./site/wwwroot/webapps"; 

        // FTP the build WAR to Azure
        ftpClient.connect(server);
        ftpClient.login(profile.ftpUsername(), profile.ftpPassword());
        ftpClient.setFileType(FTP.BINARY_FILE_TYPE);
        ftpClient.changeWorkingDirectory(path);
        ftpClient.storeFile(fileName, file);
        ftpClient.disconnect();
}
```

此程式碼會將 WAR 檔案上傳到 `/site/wwwroot/webapps` 目錄。 Tomcat 預設會將放置在這個目錄的 WAR 檔案部署到 App Service。

## <a name="deploy-a-java-application-from-a-local-git-repo"></a>從本機 Git 存放庫部署 Java 應用程式

```java
// get the publishing profile from the App Service webapp
PublishingProfile profile = app2.getPublishingProfile();

// create a new Git repo in the sample directory under src/main/resources 
Git git = Git
    .init()
    .setDirectory(new File(ManageWebAppSourceControl.class.getResource("/azure-samples-appservice-helloworld/").getPath()))
    .call();
git.add().addFilepattern(".").call();
// add the files in the sample app to an initial commit
git.commit().setMessage("Initial commit").call(); 

// push the commit using the Azure Git remote URL and credentials in the publishing profile
PushCommand command = git.push();
command.setRemote(profile.gitUrl()); 
command.setCredentialsProvider(new UsernamePasswordCredentialsProvider(profile.gitUsername(), profile.gitPassword()));
command.setRefSpecs(new RefSpec("master:master")); 
command.setForce(true);
command.call();
```      

此程式碼會使用 [JGit](https://eclipse.org/jgit/) 程式庫在 `src/main/resources/azure-samples-appservice-helloworld` 資料夾中建立新的 Git 存放庫。 此範例接著會將資料夾內的所有檔案新增到初始認可，然後使用從 webapp 的 `PublishingProfile` 得到的 Git 部署資訊將認可推送到 Azure 中。 

>[!NOTE]
> 存放庫中的檔案配置必須與您想在 Azure App Service 的 `/site/wwwroot/` 目錄下部署檔案的方式完全相符。

## <a name="deploy-an-application-from-a-public-git-repo"></a>從公用 Git 存放庫部署應用程式

```java
// deploy a .NET sample app from a public GitHub repo into a new webapp
WebApp app3 = azure.webApps().define(app3Name)
                .withNewResourceGroup(rgName)
                .withExistingAppServicePlan(plan)
                .defineSourceControl()
                .withPublicGitRepository(
                   "https://github.com/Azure-Samples/app-service-web-dotnet-get-started")
                .withBranch("master")
                .attach()
                .create();
 ```

 App Service 執行階段會使用存放庫 `master` 分支上的最新程式碼自動建置和部署 .NET 專案。

## <a name="continuous-deployment-from-a-github-repo"></a>從 GitHub 存放庫進行持續部署

```java
// deploy the application whenever you push a new commit or merge a pull request into your master branch
WebApp app4 = azure.webApps()
                    .define(app4Name)
                    .withExistingResourceGroup(rgName)
                    .withExistingAppServicePlan(plan)
                    // Uncomment the following lines to turn on continuous deployment scenario
                    //.defineSourceControl()
                    //    .withContinuouslyIntegratedGitHubRepository("username", "reponame")
                    //    .withBranch("master")
                    //    .withGitHubAccessToken("YOUR GITHUB PERSONAL TOKEN")
                    //    .attach()
                    .create();
```  

`username` 和 `reponame` 值就是 GitHub 中使用的值。 [建立 GitHub 個人存取權杖](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)並使其具有存放庫的讀取權限，然後將此權杖傳遞給 `withGitHubAccessToken`。 


## <a name="sample-explanation"></a>範例的說明

此範例會使用於新建立的[標準](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) App Service 方案中執行的 Java 8 和 Tomcat 8，來建立第一個應用程式。 此程式碼接著會使用 `PublishingProfile` 物件中的資訊將 WAR 檔案上傳到 FTP，再由 Tomcat 進行部署。

第二個應用程式會使用與第一個應用程式相同的方案，並且也會設定為 Java 8/Tomcat 8 應用程式。 JGit 程式庫會在資料夾中建立新的 Git 存放庫，此資料夾位於與 App Service 相對應的目錄結構中，且包含已解壓縮的 Java Web 應用程式。 新的認可會將資料夾中的檔案新增至新的 Git 存放庫，而且 Git 會使用 webapp 的 `PublishingProfile` 所提供的遠端 URL 與使用者名稱/密碼將這個新的認可推送至 Azure。

第三個應用程式並非針對 Java 和 Tomcat 來設定。 相反地，我們會直接從來源部署公用 GitHub 存放庫中的 .NET 範例。

第四個應用程式會在您每次推送變更或合併提取要求時，將主要分支中的程式碼部署到 GitHub 存放庫的主要分支。

| 範例中使用的類別 | 注意事項
|-------|-------|
| [WebApp](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_app) | 透過 `azure.webApps().define()....create()` Fluent 鏈結所建立。 會建立 App Service Web 應用程式以及應用程式所需的任何資源。 大部分的方法都會查詢物件的組態詳細資料，但 `restart()` 等動詞命令方法則會變更 webapp 的狀態。
| [WebContainer](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_container) | 具有靜態公用欄位的類別，在定義執行 Java webcontainer 的 WebApp 時可作為 `withWebContainer()` 的參數。 Jetty 和 Tomcat 版本均有選項可供選擇。
| [PublishingProfile](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._publishing_profile) | 使用 `getPublishingProfile()` 方法透過 WebApp 物件來取得。 包含 FTP 和 Git 部署資訊，這些資訊包括部署使用者名稱和密碼 (不同於 Azure 帳戶或服務主體認證)。
| [AppServicePlan](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_plan) | `azure.appServices().appServicePlans().getByResourceGroup()` 所傳回。 有方法可以檢查容量、層級和方案中執行的 Web 應用程式數目。
| [AppServicePricingTier](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_pricing_tier) | 具有靜態公用欄位的類別，代表 App Service 層級。 用來在應用程式建立期間使用 `withPricingTier()` 定義內嵌的方案層級，或用來在透過 `azure.appServices().appServicePlans().define()` 定義方案時直接定義內嵌的方案層級
| [JavaVersion](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._java_version) | 具有靜態公用欄位的類別，代表 App Service 所支援的 Java 版本。 在建立新的 webapp 時，於 `define()...create()` 鏈結期間與 `withJavaVersion()` 搭配使用。

## <a name="next-steps"></a>後續步驟

[!INCLUDE [next-steps](includes/java-next-steps.md)]