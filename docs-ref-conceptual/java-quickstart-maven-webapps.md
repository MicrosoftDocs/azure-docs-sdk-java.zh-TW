---
title: 在 5 分鐘內使用 Maven 將 Java Web 應用程式部署到 Azure | Microsoft Docs
description: 建立以 Maven 建置的 Java 應用程式並部署到 Azure
services: app-service\web
documentationcenter: ''
author: rloutlaw
manager: douge
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: routlaw
ms.openlocfilehash: 70b508118c50b75693e2d746dc1e2919c827cb29
ms.sourcegitcommit: 0f38ef9ad64cffdb7b2e9e966224dfd0af251b0f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2018
ms.locfileid: "42703541"
---
# <a name="create-and-deploy-a-java-app-to-azure-with-maven"></a>使用 Maven 建立 Java 應用程式並部署到 Azure

本快速入門可協助您使用 [Apache Maven](http://maven.apache.org) 和 Azure CLI，在短短幾分鐘內建立簡單的 Java Web 應用程式並部署到 [Azure App Service](/azure/app-service/app-service-value-prop-what-is)。

## <a name="before-you-begin"></a>開始之前

開始之前，請設定下列項目：

- [Git](https://git-scm.com/)
- [Java 8](https://www.azul.com/downloads/zulu/)
- [Maven 3](http://maven.apache.org/download.cgi)
- [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)

如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

## <a name="get-the-sample-code"></a>取得範例程式碼

將範例應用程式存放庫複製到本機電腦。 此範例是簡單的 JSP 應用程式，並已在 `pom.xml` 專案中另外進行 Maven 設定，以供您在本機測試應用程式，並協助將範例部署至 Azure App Service。

```
git clone https://github.com/Azure-Samples/app-service-maven
```

## <a name="run-the-app-locally"></a>在本機執行應用程式

開啟命令提示字元，然後使用 Maven 建置應用程式並在本機的 [Tomcat](https://tomcat.apache.org) Web 容器中執行。 

```
cd app-service-maven
mvn package
mvn tomcat7:run-war
```

開啟網頁瀏覽器，然後巡覽至 http://localhost:8080 以預覽應用程式：

  ![Java 應用程式範例的 Hello World 輸出](media/maven-quickstart/java-app-hello-world-output.png)

## <a name="log-in-to-azure"></a>登入 Azure

使用 `az login` 登入 Azure CLI。 本快速入門使用 CLI 來查詢及建立為了裝載該應用程式所需要的 Azure 資源。
   
```azurecli
az login
```

## <a name="create-a-resource-group"></a>建立資源群組   

使用 [az group create](/cli/azure/group#create) 來建立資源群組。 Azure 資源群組是在其中部署與管理 Azure 資源的邏輯容器。

```azurecli
az group create --location "East US" --name myResourceGroup
```

若要查看可用於 `---location` 的其他可能值，請使用 [az appservice list-locations](/cli/azure/appservice#list-locations)

## <a name="create-an-app-service-plan"></a>建立應用程式服務方案

使用 [az appservice plan create](/cli/azure/appservice/plan#create) 建立**免費**的 [App Service 方案](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。 App Service 方案會配置可供方案中執行之所有 Web 應用程式共用的資源。


```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

App Service 方案準備就緒後，Azure CLI 會顯示類似下列範例的資訊：

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


## <a name="create-a-web-app"></a>建立 Web 應用程式

使用 Azure CLI 的 [az webapp create](/cli/azure/appservice/web#create) 命令，建立會使用方案資源的 Web 應用程式。 Web 應用程式定義會提供空間供您部署程式碼，並提供 URL 供您存取已在執行的應用程式。

在下列命令中，請將 <appname> 預留位置替換成您自己的唯一應用程式名稱。 <appname> 使用於 Web 應用程式的預設主機名稱。 如果 <appname> 不是唯一的，您會收到易懂的錯誤訊息「具有指定名稱的網站已經存在」。

```azurecli
az webapp create --name appname --resource-group myResourceGroup --plan my-free-appservice-plan
```

建立 Web 應用程式後，Azure CLI 會顯示類似下列範例的資訊。

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

## <a name="configure-java-and-tomcat"></a>設定 Java 和 Tomcat

使用 Azure CLI 的 [az webapp config](/cli/azure/appservice/web#config) 命令將 Web 應用程式設定為使用 Java 和 Tomcat。 下列範例會將 App Service 設定為執行 Java 8 和 [Apache Tomcat 8.5](http://tomcat.apache.org/) 以執行應用程式。

```azurecli
az webapp config set \ 
--name appname \
--resource-group myResourceGroup \
--java-container TOMCAT \
--java-version 1.8.0_73  \
--java-container-version 8.5
```

## <a name="configure-maven"></a>設定 Maven 

此範例的 Maven `pom.xml` 會包含可將範例 FTP 到 Azure 的組態，不過您需要對該組態進行自訂，使其部署到您自己的 Web 應用程式。 使用 [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles) 擷取 App Service 認證：

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

將 `az-settings.xml` 中的預留位置取代為輸出中的密碼、使用者名稱和 FTP 主機名稱。 使用者名稱中請務必只使用一個 `\` 字元，而不是像 CLI 輸出一樣地使用逸出值。
   
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

## <a name="deploy-the-sample-application"></a>部署範例應用程式

將範例部署至 Azure，使用 Maven 的 `-s` 參數以使用 `az-settings.xml` 檔案中的組態。

```
mvn install -s az-settings.xml
```

## <a name="browse-to-web-app"></a>瀏覽至 Web 應用程式

檢視在 Azure 中執行的範例：

```azurecli
az webapp browse --name appname --resource-group myResourceGroup
```

## <a name="update-the-app"></a>更新應用程式

使用文字編輯器開啟 `src/main/webapp/index.jsp`，並以下列 JSP 取代現有 JSP。

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

使用 Maven 來部署更新：

```
mvn clean package
mvn install -s az-settings.xml
```

在應用程式重新部署後，請重新整理瀏覽器以檢視您的變更。

## <a name="manage-your-new-azure-app"></a>管理新的 Azure 應用程式

移至 Azure 入口網站，查看您剛建立的 Web 應用程式。

若要這樣做，請登入 [https://portal.azure.com](https://portal.azure.com)。

按一下左側功能表中的 [App Service]，然後按一下 Azure Web 應用程式的名稱。

 ![從入口網站的 webapps 清單選取您的應用程式](media/azure-app-service-portal.png)

對應用程式執行 `curl` 來傳送一些流量，以測試監視功能。

```bash
curl http://<appname>.azurewebsites.net/?[1-30]
```

幾分鐘之後，您就會在監視中看到要求活動。

 ![Azure 入口網站中的監視](media/java-app-monitoring.png)

## <a name="clean-up-resources"></a>清除資源

若要移除此指南所建立的所有資源，請執行下列命令︰

```azurecli
az group delete --name myResrouceGroup
```

## <a name="next-steps"></a>後續步驟

瀏覽完整的 [Azure Java 範例](https://azure.microsoft.com/resources/samples/?term=java)清單。
