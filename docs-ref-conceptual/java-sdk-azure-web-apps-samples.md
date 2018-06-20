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
# <a name="azure-management-libraries-for-java-samples-for-web-apps"></a>針對 Web 應用程式所提供之適用於 Java 的 Azure 管理程式庫範例

| **建立應用程式** ||
|---|---|
| [建立 Web 應用程式並從 FTP 或 GitHub 部署][1] | 透過本機 Git、FTP 以及 GitHub 的持續整合來部署 Web 應用程式。 |
| [建立 Web 應用程式和管理部署位置][2] | 建立 Web 應用程式並部署到預備位置，然後在位置之間交換部署。 |
| **設定應用程式** ||
| [建立 Web 應用程式和設定自訂網域][3] | 使用自訂網域和自我簽署的 SSL 憑證來建立 Web 應用程式。 |
| **調整應用程式規模** ||
| [透過跨多個區域的高可用性來調整 Web 應用程式的規模][4] | 在三個不同的地理區域中調整 Web 應用程式的規模，並使用 Azure 流量管理員讓它們可透過單一端點來供人使用。 | 
| **將應用程式連線至資源** ||
| [將 Web 應用程式連線至儲存體帳戶][5] | 建立 Azure 儲存體帳戶，然後將儲存體帳戶連接字串新增至應用程式設定。 |
| [將 Web 應用程式連線至 SQL 資料庫][6] | 建立 Web 應用程式和 SQL 資料庫，然後將 SQL 資料庫連接字串新增至應用程式設定。 |

[1]: java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
[5]: https://azure.microsoft.com/resources/samples/app-service-java-manage-storage-connections-for-web-apps/
[6]: https://azure.microsoft.com/resources/samples/app-service-java-manage-data-connections-for-web-apps/