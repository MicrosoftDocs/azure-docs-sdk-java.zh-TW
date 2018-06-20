---
title: 適用於 Java 的 Azure App Service 程式庫
description: 使用 Azure 管理 API 在 Azure App Service 上自動部署 Web 應用程式。
keywords: Azure, Java, SDK, API, Web 應用程式, 行動, App Service
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/09/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: appservice
ms.openlocfilehash: adbb527666553ecc3039ce35c035d017f502c801
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823791"
---
# <a name="azure-app-service-libraries-for-java"></a>適用於 Java 的 Azure App Service 程式庫

## <a name="overview"></a>概觀

使用 [Azure App Service](/azure/app-service) 部署和管理網站、Web 應用程式和 REST API。

若要開始使用 Azure App Service，請參閱[在 Azure 中建立第一個 Java Web 應用程式](/azure/app-service-web/app-service-web-get-started-java)。

## <a name="management-api"></a>管理 API

使用管理 API 在 Azure App Service 中部署、調整和設定應用程式。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-appservice</artifactId>
    <version>1.3.0</version>
</dependency>
```   

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/appservice/management)

### <a name="example"></a>範例

將 webapp 從 Docker 映像部署到在 Linux 上執行的 Azure Web 應用程式。

```java
WebApp app = azure.webApps().define("newLinuxWebApp")
    .withExistingLinuxPlan(myLinuxAppServicePlan)
    .withExistingResourceGroup("myResourceGroup")
    .withPrivateDockerHubImage("username/my-java-app")
    .withCredentials("dockerHubUser","dockerHubPassword")
    .withAppSetting("PORT","8080").
    .create();
```

## <a name="samples"></a>範例

[從 FTP 或 GitHub 部署 Web 應用程式][1]  
[使用部署位置在應用程式版本之間交換][2]  
[設定自訂網域][3]   
[跨多個區域調整 Web 應用程式][4]   

深入探索可在應用程式中使用的 [Azure App Service Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice)。

[1]: ../docs-ref-conceptual/java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
