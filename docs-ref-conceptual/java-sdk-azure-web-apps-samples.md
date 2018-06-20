---
title: 適用於 Java 的 Azure 管理程式庫 Web 應用程式範例
description: 取得程式碼範例，以使用適用於 Java 的 Azure 管理程式庫來建立和更新裝載於 App Service 中的 Azure Web 應用程式
keywords: Azure, Java, SDK, API, Maven, Gradle, Web 應用程式, app service
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 43633e5c-9fb1-4807-ba63-e24c126754e2
ms.openlocfilehash: 2f1e43f3835ffcdb138bf7e29a1656b7ee381281
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
ms.locfileid: "21930894"
---
# <a name="azure-management-libraries-for-java-samples-for-web-apps"></a><span data-ttu-id="4e64a-104">針對 Web 應用程式所提供之適用於 Java 的 Azure 管理程式庫範例</span><span class="sxs-lookup"><span data-stu-id="4e64a-104">Azure management libraries for Java samples for web apps</span></span>

| <span data-ttu-id="4e64a-105">**建立應用程式**</span><span class="sxs-lookup"><span data-stu-id="4e64a-105">**Create an app**</span></span> ||
|---|---|
| <span data-ttu-id="4e64a-106">[建立 Web 應用程式並從 FTP 或 GitHub 部署][1]</span><span class="sxs-lookup"><span data-stu-id="4e64a-106">[Create a web app and deploy from FTP or GitHub][1]</span></span> | <span data-ttu-id="4e64a-107">透過本機 Git、FTP 以及 GitHub 的持續整合來部署 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4e64a-107">Deploy web apps from local Git, FTP, and continuous integration from GitHub.</span></span> |
| <span data-ttu-id="4e64a-108">[建立 Web 應用程式和管理部署位置][2]</span><span class="sxs-lookup"><span data-stu-id="4e64a-108">[Create a web app and manage deployment slots][2]</span></span> | <span data-ttu-id="4e64a-109">建立 Web 應用程式並部署到預備位置，然後在位置之間交換部署。</span><span class="sxs-lookup"><span data-stu-id="4e64a-109">Create a web app and deploy to staging slots, and then swap deployments between slots.</span></span> |
| <span data-ttu-id="4e64a-110">**設定應用程式**</span><span class="sxs-lookup"><span data-stu-id="4e64a-110">**Configure app**</span></span> ||
| <span data-ttu-id="4e64a-111">[建立 Web 應用程式和設定自訂網域][3]</span><span class="sxs-lookup"><span data-stu-id="4e64a-111">[Create a web app and configure a custom domain][3]</span></span> | <span data-ttu-id="4e64a-112">使用自訂網域和自我簽署的 SSL 憑證來建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4e64a-112">Create a web app with a custom domain and self-signed SSL certificate.</span></span> |
| <span data-ttu-id="4e64a-113">**調整應用程式規模**</span><span class="sxs-lookup"><span data-stu-id="4e64a-113">**Scale apps**</span></span> ||
| <span data-ttu-id="4e64a-114">[透過跨多個區域的高可用性來調整 Web 應用程式的規模][4]</span><span class="sxs-lookup"><span data-stu-id="4e64a-114">[Scale a web app with high availability across multiple regions][4]</span></span> | <span data-ttu-id="4e64a-115">在三個不同的地理區域中調整 Web 應用程式的規模，並使用 Azure 流量管理員讓它們可透過單一端點來供人使用。</span><span class="sxs-lookup"><span data-stu-id="4e64a-115">Scale a web app in three different geographical regions and make them available through a single endpoint using Azure Traffic Manager.</span></span> | 
| <span data-ttu-id="4e64a-116">**將應用程式連線至資源**</span><span class="sxs-lookup"><span data-stu-id="4e64a-116">**Connect app to resources**</span></span> ||
| <span data-ttu-id="4e64a-117">[將 Web 應用程式連線至儲存體帳戶][5]</span><span class="sxs-lookup"><span data-stu-id="4e64a-117">[Connect a web app to a storage account][5]</span></span> | <span data-ttu-id="4e64a-118">建立 Azure 儲存體帳戶，然後將儲存體帳戶連接字串新增至應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="4e64a-118">Create an Azure storage account and add the storage account connection string to the app settings.</span></span> |
| <span data-ttu-id="4e64a-119">[將 Web 應用程式連線至 SQL 資料庫][6]</span><span class="sxs-lookup"><span data-stu-id="4e64a-119">[Connect a web app to a SQL database][6]</span></span> | <span data-ttu-id="4e64a-120">建立 Web 應用程式和 SQL 資料庫，然後將 SQL 資料庫連接字串新增至應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="4e64a-120">Create a web app and SQL database, and then add the SQL database connection string to the app settings.</span></span> |

[1]: java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
[5]: https://azure.microsoft.com/resources/samples/app-service-java-manage-storage-connections-for-web-apps/
[6]: https://azure.microsoft.com/resources/samples/app-service-java-manage-data-connections-for-web-apps/