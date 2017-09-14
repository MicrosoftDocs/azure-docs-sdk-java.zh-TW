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
ms.openlocfilehash: ce0b003cc7c48c8690f4236547ddec36e6ea9d53
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2017
---
# <a name="azure-tools-for-java-developers"></a><span data-ttu-id="24d82-103">適用於 Java 開發人員的 Azure 工具</span><span class="sxs-lookup"><span data-stu-id="24d82-103">Azure tools for Java developers</span></span>

## <a name="client-and-management-libraries"></a><span data-ttu-id="24d82-104">用戶端和管理程式庫</span><span class="sxs-lookup"><span data-stu-id="24d82-104">Client and management libraries</span></span>

<span data-ttu-id="24d82-105">使用適用於 Java 的 Azure 程式庫從應用程式連線至服務並管理 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="24d82-105">Connect to services and manage Azure resources from your applications with the Azure libraries for Java.</span></span> <span data-ttu-id="24d82-106">將此相依性新增至 pom.xml 專案，以將管理程式庫匯入 Maven 專案中。</span><span class="sxs-lookup"><span data-stu-id="24d82-106">Import the management libraries into your Maven projects by adding this dependency to your project *pom.xml*.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.2.1</version>
</dependency>
```

<span data-ttu-id="24d82-107">檢視[完整的程式庫清單](java-sdk-azure-install.md)並[開始使用](java-sdk-azure-get-started.md)適用於 Java 的 Azure 程式庫。</span><span class="sxs-lookup"><span data-stu-id="24d82-107">View the [complete list of libraries](java-sdk-azure-install.md) and [get started](java-sdk-azure-get-started.md) with the Azure libraries for Java.</span></span>

## <a name="eclipse-and-intellij-plugins"></a><span data-ttu-id="24d82-108">Eclipse 和 IntelliJ 外掛程式</span><span class="sxs-lookup"><span data-stu-id="24d82-108">Eclipse and IntelliJ plugins</span></span>

<span data-ttu-id="24d82-109">使用適用於 [Eclipse](eclipse/azure-toolkit-for-eclipse.md) 和 [IntelliJ](intellij/azure-toolkit-for-intellij.md) 的 Azure 工具組從您的 IDE 管理 Azure 資源並部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="24d82-109">Manage Azure resources and deploy apps from your IDE with The Azure toolkits for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) and [IntelliJ](intellij/azure-toolkit-for-intellij.md).</span></span>   

![顯示 Azure Explorer 的 IntelliJ 工具組](media/intelliJ-azure-explorer.png)

<span data-ttu-id="24d82-111">[開始使用適用於 Eclipse 的 Azure 工具組](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [開始使用適用於 IntelliJ 的 Azure 工具組](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span><span class="sxs-lookup"><span data-stu-id="24d82-111">[Get started with Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Get started with Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span></span> 

## <a name="azure-cli-20"></a><span data-ttu-id="24d82-112">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="24d82-112">Azure CLI 2.0</span></span>

<span data-ttu-id="24d82-113">Azure CLI 2.0 提供了命令列體驗供您管理 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="24d82-113">The Azure 2.0 CLI provides a command-line experience to manage Azure resources.</span></span> <span data-ttu-id="24d82-114">您可以透過 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) 在瀏覽器中使用，或者在 macOS、Linux 和 Windows 中[安裝](https://docs.microsoft.com/cli/azure/install-azure-cli)並從命令列中執行。</span><span class="sxs-lookup"><span data-stu-id="24d82-114">You can use it in your browser with [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), or you can [install](https://docs.microsoft.com/cli/azure/install-azure-cli) it on macOS, Linux, and Windows and run it from the command line.</span></span>

<span data-ttu-id="24d82-115">[開始使用 Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="24d82-115">[Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>

## <a name="azure-storage-explorer"></a><span data-ttu-id="24d82-116">Azure 儲存體總管</span><span class="sxs-lookup"><span data-stu-id="24d82-116">Azure Storage Explorer</span></span> 

<span data-ttu-id="24d82-117">從桌面管理 Azure 儲存體帳戶、容器和 blob/檔案。</span><span class="sxs-lookup"><span data-stu-id="24d82-117">Manage Azure storage accounts, containers, and blobs/files from your desktop.</span></span> <span data-ttu-id="24d82-118">Azure 儲存體總管目前為預覽狀態，可在 Windows、macOS 和 Linux 上運作。</span><span class="sxs-lookup"><span data-stu-id="24d82-118">Azure Storage Explorer is currently in Preview and works on Windows, macOS, and Linux.</span></span>

[<span data-ttu-id="24d82-119">開始使用 Azure 儲存體總管</span><span class="sxs-lookup"><span data-stu-id="24d82-119">Get started with Azure Storage Explorer</span></span>](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)