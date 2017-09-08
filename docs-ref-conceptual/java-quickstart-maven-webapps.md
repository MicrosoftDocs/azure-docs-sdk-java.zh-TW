---
title: "在 5 分鐘內使用 Maven 將 Java Web 應用程式部署到 Azure | Microsoft Docs"
description: "建立以 Maven 建置的 Java 應用程式並部署到 Azure"
services: app-service\web
documentationcenter: 
author: rloutlaw
manager: douge
editor: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: routlaw
ms.openlocfilehash: 9c3d39e12b00dd1dd3d5f76162ce0f387c7a947c
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="create-and-deploy-a-java-app-to-azure-with-maven"></a><span data-ttu-id="6a788-103">使用 Maven 建立 Java 應用程式並部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="6a788-103">Create and deploy a Java app to Azure with Maven</span></span>

<span data-ttu-id="6a788-104">本快速入門可協助您使用 [Apache Maven](http://maven.apache.org) 和 Azure CLI，在短短幾分鐘內建立簡單的 Java Web 應用程式並部署到 [Azure App Service](/azure/app-service/app-service-value-prop-what-is)。</span><span class="sxs-lookup"><span data-stu-id="6a788-104">This quickstart helps you create and deploy a simple Java web app to [Azure App Service](/azure/app-service/app-service-value-prop-what-is) in just a few minutes using [Apache Maven](http://maven.apache.org) and the Azure CLI.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6a788-105">開始之前</span><span class="sxs-lookup"><span data-stu-id="6a788-105">Before you begin</span></span>

<span data-ttu-id="6a788-106">開始之前，請設定下列項目：</span><span class="sxs-lookup"><span data-stu-id="6a788-106">Before you start, set up the following:</span></span>

- [<span data-ttu-id="6a788-107">Git</span><span class="sxs-lookup"><span data-stu-id="6a788-107">Git</span></span>](https://git-scm.com/)
- [<span data-ttu-id="6a788-108">Java 8</span><span class="sxs-lookup"><span data-stu-id="6a788-108">Java 8</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
- [<span data-ttu-id="6a788-109">Maven 3</span><span class="sxs-lookup"><span data-stu-id="6a788-109">Maven 3</span></span>](http://maven.apache.org/download.cgi)
- [<span data-ttu-id="6a788-110">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="6a788-110">Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-az-cli2)

<span data-ttu-id="6a788-111">如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。</span><span class="sxs-lookup"><span data-stu-id="6a788-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="get-the-sample-code"></a><span data-ttu-id="6a788-112">取得範例程式碼</span><span class="sxs-lookup"><span data-stu-id="6a788-112">Get the sample code</span></span>

<span data-ttu-id="6a788-113">將範例應用程式存放庫複製到本機電腦。</span><span class="sxs-lookup"><span data-stu-id="6a788-113">Clone the sample app repository to your local machine.</span></span> <span data-ttu-id="6a788-114">此範例是簡單的 JSP 應用程式，並已在 `pom.xml` 專案中另外進行 Maven 設定，以供您在本機測試應用程式，並協助將範例部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="6a788-114">The sample is a simple JSP application with additional Maven configuration in the project `pom.xml` to test the app locally and help deploy the sample to Azure App Service.</span></span>

```
git clone https://github.com/Azure-Samples/app-service-maven
```

## <a name="run-the-app-locally"></a><span data-ttu-id="6a788-115">在本機執行應用程式</span><span class="sxs-lookup"><span data-stu-id="6a788-115">Run the app locally</span></span>

<span data-ttu-id="6a788-116">開啟命令提示字元，然後使用 Maven 建置應用程式並在本機的 [Tomcat](https://tomcat.apache.org) Web 容器中執行。</span><span class="sxs-lookup"><span data-stu-id="6a788-116">Open a command prompt and use Maven to build the app and run it in a local [Tomcat](https://tomcat.apache.org) web container.</span></span> 

```
cd app-service-maven
mvn package
mvn tomcat7:run-war
```

<span data-ttu-id="6a788-117">開啟網頁瀏覽器並瀏覽至 http://localhost:8080 以預覽應用程式：</span><span class="sxs-lookup"><span data-stu-id="6a788-117">Open a web browser and navigate to http://localhost:8080 to preview the app:</span></span>

  ![Java 應用程式範例的 Hello World 輸出](media/maven-quickstart/java-app-hello-world-output.png)

## <a name="log-in-to-azure"></a><span data-ttu-id="6a788-119">登入 Azure</span><span class="sxs-lookup"><span data-stu-id="6a788-119">Log in to Azure</span></span>

<span data-ttu-id="6a788-120">使用 `az login` 登入 Azure CLI。</span><span class="sxs-lookup"><span data-stu-id="6a788-120">Log in to the Azure CLI with `az login`.</span></span> <span data-ttu-id="6a788-121">本快速入門使用 CLI 來查詢及建立為了裝載該應用程式所需要的 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="6a788-121">This quickstart uses the CLI to query and create the Azure resources needed to host the app.</span></span>
   
```azurecli
az login
```

## <a name="create-a-resource-group"></a><span data-ttu-id="6a788-122">建立資源群組</span><span class="sxs-lookup"><span data-stu-id="6a788-122">Create a resource group</span></span>   

<span data-ttu-id="6a788-123">使用 [az group create](/cli/azure/group#create) 來建立資源群組。</span><span class="sxs-lookup"><span data-stu-id="6a788-123">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="6a788-124">Azure 資源群組是在其中部署與管理 Azure 資源的邏輯容器。</span><span class="sxs-lookup"><span data-stu-id="6a788-124">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

```azurecli
az group create --location "East US" --name myResourceGroup
```

<span data-ttu-id="6a788-125">若要查看可用於 `---location` 的其他可能值，請使用 [az appservice list-locations](/cli/azure/appservice#list-locations)</span><span class="sxs-lookup"><span data-stu-id="6a788-125">To see other possible values you can use for `---location`, use [az appservice list-locations](/cli/azure/appservice#list-locations)</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="6a788-126">建立應用程式服務方案</span><span class="sxs-lookup"><span data-stu-id="6a788-126">Create an App Service plan</span></span>

<span data-ttu-id="6a788-127">使用 [az appservice plan create](/cli/azure/appservice/plan#create) 建立**免費**的 [App Service 方案](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。</span><span class="sxs-lookup"><span data-stu-id="6a788-127">Create a **FREE** [App Service plan](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) using [az appservice plan create](/cli/azure/appservice/plan#create).</span></span> <span data-ttu-id="6a788-128">App Service 方案會配置可供方案中執行之所有 Web 應用程式共用的資源。</span><span class="sxs-lookup"><span data-stu-id="6a788-128">App Service plans allocate resources shared across all web apps running in the plan.</span></span>


```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="6a788-129">App Service 方案準備就緒後，Azure CLI 會顯示類似下列範例的資訊：</span><span class="sxs-lookup"><span data-stu-id="6a788-129">When the App Service Plan is ready, the Azure CLI shows information similar to the following example:</span></span>

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/my-free-appservice-plan",
    "location": "East US",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```


## <a name="create-a-web-app"></a><span data-ttu-id="6a788-130">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="6a788-130">Create a web app</span></span>

<span data-ttu-id="6a788-131">使用 Azure CLI 的 [az webapp create](/cli/azure/appservice/web#create) 命令，建立會使用方案資源的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6a788-131">Create a web app that uses the plan's resources using the Azure CLI [az webapp create](/cli/azure/appservice/web#create) command.</span></span> <span data-ttu-id="6a788-132">Web 應用程式定義會提供空間供您部署程式碼，並提供 URL 供您存取已在執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6a788-132">The web app definition provides a space to deploy the code to and a URL to access the app once it's running.</span></span>

<span data-ttu-id="6a788-133">在下列命令中，請將 <appname> 預留位置替換成您自己的唯一應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="6a788-133">In the command below substitute your own unique app name where you see the <appname> placeholder.</span></span> <span data-ttu-id="6a788-134"><appname> 使用於 Web 應用程式的預設主機名稱。</span><span class="sxs-lookup"><span data-stu-id="6a788-134">The <appname> is used in the default hostname for the web app.</span></span> <span data-ttu-id="6a788-135">如果 <appname> 不是唯一的，您會收到易懂的錯誤訊息「具有指定名稱的網站已經存在」。</span><span class="sxs-lookup"><span data-stu-id="6a788-135">If <appname> is not unique, you get the friendly error message "Website with given name already exists."</span></span>

```azurecli
az webapp create --name appname --resource-group myResourceGroup --plan my-free-appservice-plan
```

<span data-ttu-id="6a788-136">建立 Web 應用程式後，Azure CLI 會顯示類似下列範例的資訊。</span><span class="sxs-lookup"><span data-stu-id="6a788-136">When the Web App has been created, the Azure CLI shows information similar to the following example.1</span></span>

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<appname>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<appname>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "East US",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/my-free-appservice-plan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

## <a name="configure-java-and-tomcat"></a><span data-ttu-id="6a788-137">設定 Java 和 Tomcat</span><span class="sxs-lookup"><span data-stu-id="6a788-137">Configure Java and Tomcat</span></span>

<span data-ttu-id="6a788-138">使用 Azure CLI 的 [az webapp config](/cli/azure/appservice/web#config) 命令將 Web 應用程式設定為使用 Java 和 Tomcat。</span><span class="sxs-lookup"><span data-stu-id="6a788-138">Configure the web app to use Java and Tomcat using the Azure CLI's [az webapp config](/cli/azure/appservice/web#config) command.</span></span> <span data-ttu-id="6a788-139">下列範例會將 App Service 設定為執行 Java 8 和 [Apache Tomcat 8.5](http://tomcat.apache.org/) 以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="6a788-139">The example below configures App Service to run Java 8 and [Apache Tomcat 8.5](http://tomcat.apache.org/) to run the app.</span></span>

```azurecli
az webapp config set \ 
--name appname \
--resource-group myResourceGroup \
--java-container TOMCAT \
--java-version 1.8.0_73  \
--java-container-version 8.5
```

## <a name="configure-maven"></a><span data-ttu-id="6a788-140">設定 Maven</span><span class="sxs-lookup"><span data-stu-id="6a788-140">Configure Maven</span></span> 

<span data-ttu-id="6a788-141">此範例的 Maven `pom.xml` 會包含可將範例 FTP 到 Azure 的組態，不過您需要對該組態進行自訂，使其部署到您自己的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6a788-141">The sample's Maven `pom.xml` includes configuration to FTP the sample into Azure, but you'll need to customize it to deploy to your own web app.</span></span> <span data-ttu-id="6a788-142">使用 [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles) 擷取 App Service 認證：</span><span class="sxs-lookup"><span data-stu-id="6a788-142">Retreive your App Seevice credentials with [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles):</span></span>

```azurecli
az webapp deployment list-publishing-profiles  \
--name appname \ 
--resource-group myResourceGroup  \
--query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" 
```

```json
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-045.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "appname\\$appname"
  }
]
```

<span data-ttu-id="6a788-143">將 `az-settings.xml` 中的預留位置取代為輸出中的密碼、使用者名稱和 FTP 主機名稱。</span><span class="sxs-lookup"><span data-stu-id="6a788-143">Replace the placeholders in the `az-settings.xml` file with password, username, and FTP hostname from the output.</span></span> <span data-ttu-id="6a788-144">使用者名稱中請務必只使用一個 `\` 字元，而不是像 CLI 輸出一樣地使用逸出值。</span><span class="sxs-lookup"><span data-stu-id="6a788-144">Make sure to only use one `\` character in the username instead of the escaped value from the CLI output.</span></span>
   
```XML
...
    <server>
      <id>azure-hello-world</id>
      <username>appname\$appname</username>
      <password>aBcDeFgHiJkLmNoPqRsTuVwXyZ</password>
    </server>
...
   <configuration>
     <fromFile>${basedir}/target/AzureAppDemo-0.0.1-SNAPSHOT.war</fromFile>
     <url>ftp://waws-prod-blu-045.ftp.azurewebsites.windows.net/site/wwwroot</url>
     <toFile>webapps/ROOT.war</toFile>
     <serverId>azure-hello-world</serverId>
   </configuration>
...
```

## <a name="deploy-the-sample-application"></a><span data-ttu-id="6a788-145">部署範例應用程式</span><span class="sxs-lookup"><span data-stu-id="6a788-145">Deploy the sample application</span></span>

<span data-ttu-id="6a788-146">將範例部署至 Azure，使用 Maven 的 `-s` 參數以使用 `az-settings.xml` 檔案中的組態。</span><span class="sxs-lookup"><span data-stu-id="6a788-146">Deploy with sample to Azure, using Maven's `-s` parameter to use the configuration in the `az-settings.xml` file.</span></span>

```
mvn install -s az-settings.xml
```

## <a name="browse-to-web-app"></a><span data-ttu-id="6a788-147">瀏覽至 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="6a788-147">Browse to web app</span></span>

<span data-ttu-id="6a788-148">檢視在 Azure 中執行的範例：</span><span class="sxs-lookup"><span data-stu-id="6a788-148">View the sample running in Azure:</span></span>

```azurecli
az webapp browse --name appname --resource-group myResourceGroup
```

## <a name="update-the-app"></a><span data-ttu-id="6a788-149">更新應用程式</span><span class="sxs-lookup"><span data-stu-id="6a788-149">Update the app</span></span>

<span data-ttu-id="6a788-150">使用文字編輯器開啟 `src/main/webapp/index.jsp`，並以下列 JSP 取代現有 JSP。</span><span class="sxs-lookup"><span data-stu-id="6a788-150">Using a text editor, open `src/main/webapp/index.jsp`, and replace the existing JSP with the one below.</span></span>

```html
<%--
  Copyright (c) Microsoft Corporation. All rights reserved.
  Licensed under the MIT License. See License.txt in the project root for
  license information.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java"  import="java.util.Date" %>
<html>
<head>
  <title>Azure Samples Hello World</title>
</head>
<body>
  <H1>Hello Azure!</H1>
   Current time is: <%= new Date() %>
</body>
</html>
```

<span data-ttu-id="6a788-151">使用 Maven 來部署更新：</span><span class="sxs-lookup"><span data-stu-id="6a788-151">Deploy the update with Maven:</span></span>

```
mvn clean package
mvn install -s az-settings.xml
```

<span data-ttu-id="6a788-152">在應用程式重新部署後，請重新整理瀏覽器以檢視您的變更。</span><span class="sxs-lookup"><span data-stu-id="6a788-152">Refresh your browser after the app redeploys to view your changes.</span></span>

## <a name="manage-your-new-azure-app"></a><span data-ttu-id="6a788-153">管理新的 Azure 應用程式</span><span class="sxs-lookup"><span data-stu-id="6a788-153">Manage your new Azure app</span></span>

<span data-ttu-id="6a788-154">移至 Azure 入口網站，查看您剛建立的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6a788-154">Go to the Azure portal to take a look at the web app you just created.</span></span>

<span data-ttu-id="6a788-155">若要這麼做，請登入 [https://portal.azure.com](https://portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="6a788-155">To do this, sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>

<span data-ttu-id="6a788-156">按一下左側功能表中的 [App Service]，然後按一下 Azure Web 應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="6a788-156">From the left menu, click **App Service**, then click the name of your Azure web app.</span></span>

 ![從入口網站的 webapps 清單選取您的應用程式](media/azure-app-service-portal.png)

<span data-ttu-id="6a788-158">對應用程式執行 `curl` 來傳送一些流量，以測試監視功能。</span><span class="sxs-lookup"><span data-stu-id="6a788-158">Test the monitoring by running `curl` against the app to send some traffic.</span></span>

```bash
curl http://<appname>.azurewebsites.net/?[1-30]
```

<span data-ttu-id="6a788-159">幾分鐘之後，您就會在監視中看到要求活動。</span><span class="sxs-lookup"><span data-stu-id="6a788-159">You'll see the request activity in the monitoring after a couple of minutes.</span></span>

 ![Azure 入口網站中的監視](media/java-app-monitoring.png)

## <a name="clean-up-resources"></a><span data-ttu-id="6a788-161">清除資源</span><span class="sxs-lookup"><span data-stu-id="6a788-161">Clean up resources</span></span>

<span data-ttu-id="6a788-162">若要移除此指南所建立的所有資源，請執行下列命令︰</span><span class="sxs-lookup"><span data-stu-id="6a788-162">To remove all the resources created in this guide, run the following command:</span></span>

```azurecli
az group delete --name myResrouceGroup
```

## <a name="next-steps"></a><span data-ttu-id="6a788-163">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6a788-163">Next steps</span></span>

<span data-ttu-id="6a788-164">瀏覽完整的 [Azure Java 範例](https://azure.microsoft.com/resources/samples/?term=java)清單。</span><span class="sxs-lookup"><span data-stu-id="6a788-164">Browse the full list of [Azure Java samples](https://azure.microsoft.com/resources/samples/?term=java).</span></span>