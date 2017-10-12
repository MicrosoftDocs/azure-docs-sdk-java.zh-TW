---
title: "適用於 Azure Java 開發人員的工具 | Microsoft Docs"
description: "供 Java 開發人員在 Azure 上工作的 IDE 整合、模擬器、資源總管和命令列介面。"
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 4/10/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: ff3ea805daefb3c0a413b109e431d2235a5dc5b8
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2017
---
# <a name="azure-tools-for-java-developers"></a>適用於 Java 開發人員的 Azure 工具

## <a name="client-and-management-libraries"></a>用戶端和管理程式庫

使用適用於 Java 的 Azure 程式庫從應用程式連線至服務並管理 Azure 資源。 將此相依性新增至 pom.xml 專案，以將管理程式庫匯入 Maven 專案中。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

檢視[完整的程式庫清單](java-sdk-azure-install.md)並[開始使用](java-sdk-azure-get-started.md)適用於 Java 的 Azure 程式庫。

## <a name="eclipse-and-intellij-plugins"></a>Eclipse 和 IntelliJ 外掛程式

使用適用於 [Eclipse](eclipse/azure-toolkit-for-eclipse.md) 和 [IntelliJ](intellij/azure-toolkit-for-intellij.md) 的 Azure 工具組從您的 IDE 管理 Azure 資源並部署應用程式。   

![顯示 Azure Explorer 的 IntelliJ 工具組](media/intelliJ-azure-explorer.png)

[開始使用適用於 Eclipse 的 Azure 工具組](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [開始使用適用於 IntelliJ 的 Azure 工具組](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app) 

## <a name="azure-cli-20"></a>Azure CLI 2.0

Azure CLI 2.0 提供了命令列體驗供您管理 Azure 資源。 您可以透過 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) 在瀏覽器中使用，或者在 macOS、Linux 和 Windows 中[安裝](https://docs.microsoft.com/cli/azure/install-azure-cli)並從命令列中執行。

[開始使用 Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)。

## <a name="azure-storage-explorer"></a>Azure 儲存體總管 

從桌面管理 Azure 儲存體帳戶、容器和 blob/檔案。 Azure 儲存體總管目前為預覽狀態，可在 Windows、macOS 和 Linux 上運作。

[開始使用 Azure 儲存體總管](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)