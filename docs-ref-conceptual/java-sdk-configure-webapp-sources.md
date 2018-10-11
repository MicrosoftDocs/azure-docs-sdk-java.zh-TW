---
title: 使用 Java 來設定 webapp 部署 | Microsoft Docs
description: 使用 Azure SDK for Java 來設定 Git 或 FTP Azure App Service 部署的 Java 程式碼範例
author: rloutlaw
manager: douge
ms.assetid: 833e9c78-1e50-4c23-a611-f73a2f0c2983
ms.service: Azure
ms.devlang: java
ms.topic: article
ms.date: 03/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 910d1e43c9942d6402aeccb8757ba819b7453dab
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893139"
---
# <a name="configure-azure-app-service-deployment-sources-from-your-java-applications"></a><span data-ttu-id="7de6d-103">從 Java 應用程式設定 Azure App Service 部署來源</span><span class="sxs-lookup"><span data-stu-id="7de6d-103">Configure Azure App Service deployment sources from your Java applications</span></span>

<span data-ttu-id="7de6d-104">[此範例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)會將程式碼部署到位於單一 [Azure App Service](https://docs.microsoft.com/azure/app-service/) 方案的四個應用程式，且每個應用程式各自使用不同的部署來源。</span><span class="sxs-lookup"><span data-stu-id="7de6d-104">[This sample](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) deploys code to four applications in a single [Azure App Service](https://docs.microsoft.com/azure/app-service/) plan, each using a different deployment source.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="7de6d-105">執行範例</span><span class="sxs-lookup"><span data-stu-id="7de6d-105">Run the sample</span></span>

<span data-ttu-id="7de6d-106">建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="7de6d-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="7de6d-107">然後，執行：</span><span class="sxs-lookup"><span data-stu-id="7de6d-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps.git
cd app-service-java-configure-deployment-sources-for-web-apps
mvn clean compile exec:java
```

<span data-ttu-id="7de6d-108">檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java)。</span><span class="sxs-lookup"><span data-stu-id="7de6d-108">View the [complete sample code on GitHub](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="7de6d-109">使用 Azure 進行驗證</span><span class="sxs-lookup"><span data-stu-id="7de6d-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-app-service-app-running-apache-tomcat"></a><span data-ttu-id="7de6d-110">建立執行 Apache Tomcat 的 App Service 應用程式</span><span class="sxs-lookup"><span data-stu-id="7de6d-110">Create a App Service app running Apache Tomcat</span></span>

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

<span data-ttu-id="7de6d-111">`withJavaVersion()` 和 `withWebContainer()` 會將 App Service 設定為使用 Tomcat 8 來提供 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="7de6d-111">`withJavaVersion()` and `withWebContainer()` configure App Service to serve HTTP requests using Tomcat 8.</span></span>

## <a name="deploy-a-java-application-using-ftp"></a><span data-ttu-id="7de6d-112">使用 FTP 部署 Java 應用程式</span><span class="sxs-lookup"><span data-stu-id="7de6d-112">Deploy a Java application using FTP</span></span>
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

<span data-ttu-id="7de6d-113">此程式碼會將 WAR 檔案上傳到 `/site/wwwroot/webapps` 目錄。</span><span class="sxs-lookup"><span data-stu-id="7de6d-113">This code uploads a WAR file to the `/site/wwwroot/webapps` directory.</span></span> <span data-ttu-id="7de6d-114">Tomcat 預設會將放置在這個目錄的 WAR 檔案部署到 App Service。</span><span class="sxs-lookup"><span data-stu-id="7de6d-114">Tomcat deploys WAR files placed in this directory by default in App Service.</span></span>

## <a name="deploy-a-java-application-from-a-local-git-repo"></a><span data-ttu-id="7de6d-115">從本機 Git 存放庫部署 Java 應用程式</span><span class="sxs-lookup"><span data-stu-id="7de6d-115">Deploy a Java application from a local Git repo</span></span>

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

<span data-ttu-id="7de6d-116">此程式碼會使用 [JGit](https://eclipse.org/jgit/) 程式庫在 `src/main/resources/azure-samples-appservice-helloworld` 資料夾中建立新的 Git 存放庫。</span><span class="sxs-lookup"><span data-stu-id="7de6d-116">This code uses the [JGit](https://eclipse.org/jgit/) libraries to create a new Git repo in the `src/main/resources/azure-samples-appservice-helloworld` folder.</span></span> <span data-ttu-id="7de6d-117">此範例接著會將資料夾內的所有檔案新增到初始認可，然後使用從 webapp 的 `PublishingProfile` 得到的 Git 部署資訊將認可推送到 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="7de6d-117">The sample then adds all files in the folder to an initial commit and pushes the commit to Azure using Git deployment information from the webapp's `PublishingProfile`.</span></span> 

>[!NOTE]
> <span data-ttu-id="7de6d-118">存放庫中的檔案配置必須與您想在 Azure App Service 的 `/site/wwwroot/` 目錄下部署檔案的方式完全相符。</span><span class="sxs-lookup"><span data-stu-id="7de6d-118">The layout of the files in the repo must match exactly how you want the files deployed under the `/site/wwwroot/` directory in Azure App Service.</span></span>

## <a name="deploy-an-application-from-a-public-git-repo"></a><span data-ttu-id="7de6d-119">從公用 Git 存放庫部署應用程式</span><span class="sxs-lookup"><span data-stu-id="7de6d-119">Deploy an application from a public Git repo</span></span>

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

 <span data-ttu-id="7de6d-120">App Service 執行階段會使用存放庫 `master` 分支上的最新程式碼自動建置和部署 .NET 專案。</span><span class="sxs-lookup"><span data-stu-id="7de6d-120">The App Service runtime automatically builds and deploys the .NET project using the latest code on the `master` branch of the repo.</span></span>

## <a name="continuous-deployment-from-a-github-repo"></a><span data-ttu-id="7de6d-121">從 GitHub 存放庫進行持續部署</span><span class="sxs-lookup"><span data-stu-id="7de6d-121">Continuous deployment from a GitHub repo</span></span>

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

<span data-ttu-id="7de6d-122">`username` 和 `reponame` 值就是 GitHub 中使用的值。</span><span class="sxs-lookup"><span data-stu-id="7de6d-122">The `username` and `reponame` values are the ones used in GitHub.</span></span> <span data-ttu-id="7de6d-123">[建立 GitHub 個人存取權杖](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)並使其具有存放庫的讀取權限，然後將此權杖傳遞給 `withGitHubAccessToken`。</span><span class="sxs-lookup"><span data-stu-id="7de6d-123">[Create a GitHub personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) with repo read permissions and pass it to `withGitHubAccessToken`.</span></span> 


## <a name="sample-explanation"></a><span data-ttu-id="7de6d-124">範例的說明</span><span class="sxs-lookup"><span data-stu-id="7de6d-124">Sample explanation</span></span>

<span data-ttu-id="7de6d-125">此範例會使用於新建立的[標準](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) App Service 方案中執行的 Java 8 和 Tomcat 8，來建立第一個應用程式。</span><span class="sxs-lookup"><span data-stu-id="7de6d-125">The sample creates the first application using Java 8 and Tomcat 8 running in a newly created [Standard](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) App Service plan.</span></span> <span data-ttu-id="7de6d-126">此程式碼接著會使用 `PublishingProfile` 物件中的資訊將 WAR 檔案上傳到 FTP，再由 Tomcat 進行部署。</span><span class="sxs-lookup"><span data-stu-id="7de6d-126">The code then FTPs a WAR file using the information in the `PublishingProfile` object and Tomcat deploys it.</span></span>

<span data-ttu-id="7de6d-127">第二個應用程式會使用與第一個應用程式相同的方案，並且也會設定為 Java 8/Tomcat 8 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7de6d-127">The second application uses in the same plan as the first and is also configured as a Java 8/Tomcat 8 application.</span></span> <span data-ttu-id="7de6d-128">JGit 程式庫會在資料夾中建立新的 Git 存放庫，此資料夾位於與 App Service 相對應的目錄結構中，且包含已解壓縮的 Java Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7de6d-128">The JGit libraries create a new Git repository in a folder that contains an unpacked Java web application in a directory structure that maps to App Service.</span></span> <span data-ttu-id="7de6d-129">新的認可會將資料夾中的檔案新增至新的 Git 存放庫，而且 Git 會使用 webapp 的 `PublishingProfile` 所提供的遠端 URL 與使用者名稱/密碼將這個新的認可推送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="7de6d-129">A new commit adds the files in the folder to the new Git repo, and Git pushes the commit to Azure with a remote URL and username/password provided by the webapp's `PublishingProfile`.</span></span>

<span data-ttu-id="7de6d-130">第三個應用程式並非針對 Java 和 Tomcat 來設定。</span><span class="sxs-lookup"><span data-stu-id="7de6d-130">The third application is not configured for Java and Tomcat.</span></span> <span data-ttu-id="7de6d-131">相反地，我們會直接從來源部署公用 GitHub 存放庫中的 .NET 範例。</span><span class="sxs-lookup"><span data-stu-id="7de6d-131">Instead, a .NET sample in a public GitHub repo is deployed directly from source.</span></span>

<span data-ttu-id="7de6d-132">第四個應用程式會在您每次推送變更或合併提取要求時，將主要分支中的程式碼部署到 GitHub 存放庫的主要分支。</span><span class="sxs-lookup"><span data-stu-id="7de6d-132">The fourth application deploys the code in your master branch every time you push changes or merge a pull request into the GitHub repo's master branch.</span></span>

| <span data-ttu-id="7de6d-133">範例中使用的類別</span><span class="sxs-lookup"><span data-stu-id="7de6d-133">Class used in sample</span></span> | <span data-ttu-id="7de6d-134">注意</span><span class="sxs-lookup"><span data-stu-id="7de6d-134">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="7de6d-135">WebApp</span><span class="sxs-lookup"><span data-stu-id="7de6d-135">WebApp</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_app) | <span data-ttu-id="7de6d-136">透過 `azure.webApps().define()....create()` Fluent 鏈結所建立。</span><span class="sxs-lookup"><span data-stu-id="7de6d-136">Created from the `azure.webApps().define()....create()` fluent chain.</span></span> <span data-ttu-id="7de6d-137">會建立 App Service Web 應用程式以及應用程式所需的任何資源。</span><span class="sxs-lookup"><span data-stu-id="7de6d-137">Creates a App Service web app and any resources needed for the app.</span></span> <span data-ttu-id="7de6d-138">大部分的方法都會查詢物件的組態詳細資料，但 `restart()` 等動詞命令方法則會變更 webapp 的狀態。</span><span class="sxs-lookup"><span data-stu-id="7de6d-138">Most methods query the object for configuration details, but verb methods like `restart()` change the state of the webapp.</span></span>
| [<span data-ttu-id="7de6d-139">WebContainer</span><span class="sxs-lookup"><span data-stu-id="7de6d-139">WebContainer</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_container) | <span data-ttu-id="7de6d-140">具有靜態公用欄位的類別，在定義執行 Java webcontainer 的 WebApp 時可作為 `withWebContainer()` 的參數。</span><span class="sxs-lookup"><span data-stu-id="7de6d-140">Class with static public fields used as paramters to `withWebContainer()` when defining a WebApp running a Java webcontainer.</span></span> <span data-ttu-id="7de6d-141">Jetty 和 Tomcat 版本均有選項可供選擇。</span><span class="sxs-lookup"><span data-stu-id="7de6d-141">Has choices for both Jetty and Tomcat versions.</span></span>
| [<span data-ttu-id="7de6d-142">PublishingProfile</span><span class="sxs-lookup"><span data-stu-id="7de6d-142">PublishingProfile</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._publishing_profile) | <span data-ttu-id="7de6d-143">使用 `getPublishingProfile()` 方法透過 WebApp 物件來取得。</span><span class="sxs-lookup"><span data-stu-id="7de6d-143">Obtained through a WebApp object using the `getPublishingProfile()` method.</span></span> <span data-ttu-id="7de6d-144">包含 FTP 和 Git 部署資訊，這些資訊包括部署使用者名稱和密碼 (不同於 Azure 帳戶或服務主體認證)。</span><span class="sxs-lookup"><span data-stu-id="7de6d-144">Contains FTP and Git deployment information, including deployment username and password (which is separate from Azure account or service principal credentials).</span></span>
| [<span data-ttu-id="7de6d-145">AppServicePlan</span><span class="sxs-lookup"><span data-stu-id="7de6d-145">AppServicePlan</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_plan) | <span data-ttu-id="7de6d-146">`azure.appServices().appServicePlans().getByResourceGroup()` 所傳回。</span><span class="sxs-lookup"><span data-stu-id="7de6d-146">Returned by `azure.appServices().appServicePlans().getByResourceGroup()`.</span></span> <span data-ttu-id="7de6d-147">有方法可以檢查容量、層級和方案中執行的 Web 應用程式數目。</span><span class="sxs-lookup"><span data-stu-id="7de6d-147">Methods are availble to check the capacity, tier, and number of web apps running in the plan.</span></span>
| [<span data-ttu-id="7de6d-148">AppServicePricingTier</span><span class="sxs-lookup"><span data-stu-id="7de6d-148">AppServicePricingTier</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_pricing_tier) | <span data-ttu-id="7de6d-149">具有靜態公用欄位的類別，代表 App Service 層級。</span><span class="sxs-lookup"><span data-stu-id="7de6d-149">Class with static public fields representing App Service tiers.</span></span> <span data-ttu-id="7de6d-150">用來在應用程式建立期間使用 `withPricingTier()` 定義內嵌的方案層級，或用來在透過 `azure.appServices().appServicePlans().define()` 定義方案時直接定義內嵌的方案層級</span><span class="sxs-lookup"><span data-stu-id="7de6d-150">Used to define a plan tier in-line during app creation with `withPricingTier()` or directly when defining a plan via `azure.appServices().appServicePlans().define()`</span></span>
| [<span data-ttu-id="7de6d-151">JavaVersion</span><span class="sxs-lookup"><span data-stu-id="7de6d-151">JavaVersion</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._java_version) | <span data-ttu-id="7de6d-152">具有靜態公用欄位的類別，代表 App Service 所支援的 Java 版本。</span><span class="sxs-lookup"><span data-stu-id="7de6d-152">Class with static public fields representing Java versions supported by App Service.</span></span> <span data-ttu-id="7de6d-153">在建立新的 webapp 時，於 `define()...create()` 鏈結期間與 `withJavaVersion()` 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="7de6d-153">Used with `withJavaVersion()` during the `define()...create()` chain when creating a new webapp.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7de6d-154">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7de6d-154">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]