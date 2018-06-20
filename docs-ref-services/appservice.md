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
# <a name="azure-app-service-libraries-for-java"></a><span data-ttu-id="2ddcb-104">適用於 Java 的 Azure App Service 程式庫</span><span class="sxs-lookup"><span data-stu-id="2ddcb-104">Azure App Service libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="2ddcb-105">概觀</span><span class="sxs-lookup"><span data-stu-id="2ddcb-105">Overview</span></span>

<span data-ttu-id="2ddcb-106">使用 [Azure App Service](/azure/app-service) 部署和管理網站、Web 應用程式和 REST API。</span><span class="sxs-lookup"><span data-stu-id="2ddcb-106">Deploy and manage websites, web applications, and REST APIs with [Azure App Service](/azure/app-service).</span></span>

<span data-ttu-id="2ddcb-107">若要開始使用 Azure App Service，請參閱[在 Azure 中建立第一個 Java Web 應用程式](/azure/app-service-web/app-service-web-get-started-java)。</span><span class="sxs-lookup"><span data-stu-id="2ddcb-107">To get started with Azure App Service, see [Create your first Java web app in Azure](/azure/app-service-web/app-service-web-get-started-java).</span></span>

## <a name="management-api"></a><span data-ttu-id="2ddcb-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="2ddcb-108">Management API</span></span>

<span data-ttu-id="2ddcb-109">使用管理 API 在 Azure App Service 中部署、調整和設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ddcb-109">Deploy, scale, and configure applications in Azure App Service with the management API.</span></span>

<span data-ttu-id="2ddcb-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="2ddcb-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-appservice</artifactId>
    <version>1.3.0</version>
</dependency>
```   

> [!div class="nextstepaction"]
> [<span data-ttu-id="2ddcb-111">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="2ddcb-111">Explore the Management APIs</span></span>](/java/api/overview/azure/appservice/management)

### <a name="example"></a><span data-ttu-id="2ddcb-112">範例</span><span class="sxs-lookup"><span data-stu-id="2ddcb-112">Example</span></span>

<span data-ttu-id="2ddcb-113">將 webapp 從 Docker 映像部署到在 Linux 上執行的 Azure Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ddcb-113">Deploy a webapp from a Docker image into an Azure Web App running on Linux.</span></span>

```java
WebApp app = azure.webApps().define("newLinuxWebApp")
    .withExistingLinuxPlan(myLinuxAppServicePlan)
    .withExistingResourceGroup("myResourceGroup")
    .withPrivateDockerHubImage("username/my-java-app")
    .withCredentials("dockerHubUser","dockerHubPassword")
    .withAppSetting("PORT","8080").
    .create();
```

## <a name="samples"></a><span data-ttu-id="2ddcb-114">範例</span><span class="sxs-lookup"><span data-stu-id="2ddcb-114">Samples</span></span>

<span data-ttu-id="2ddcb-115">[從 FTP 或 GitHub 部署 Web 應用程式][1]</span><span class="sxs-lookup"><span data-stu-id="2ddcb-115">[Deploy a web app from FTP or GitHub][1]</span></span>  
<span data-ttu-id="2ddcb-116">[使用部署位置在應用程式版本之間交換][2]</span><span class="sxs-lookup"><span data-stu-id="2ddcb-116">[Swap between app versions with deployment slots][2]</span></span>  
<span data-ttu-id="2ddcb-117">[設定自訂網域][3] </span><span class="sxs-lookup"><span data-stu-id="2ddcb-117">[Configure a custom domain][3] </span></span>  
<span data-ttu-id="2ddcb-118">[跨多個區域調整 Web 應用程式][4]</span><span class="sxs-lookup"><span data-stu-id="2ddcb-118">[Scale a web app across multiple regions][4]</span></span>   

<span data-ttu-id="2ddcb-119">深入探索可在應用程式中使用的 [Azure App Service Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice)。</span><span class="sxs-lookup"><span data-stu-id="2ddcb-119">Explore more [sample Java code for Azure App Service](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice) you can use in your apps.</span></span>

[1]: ../docs-ref-conceptual/java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
