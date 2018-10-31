---
title: 適用於 Azure Java 開發人員的工具 | Microsoft Docs
description: 供 Java 開發人員在 Azure 上工作的 IDE 整合、模擬器、資源總管和命令列介面。
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 4/10/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: dac0a1c576974a141950919292129890f4e15be4
ms.sourcegitcommit: 19876d17fed0afd9af0cb8e161f5a463696e74cf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "49634451"
---
# <a name="azure-tools-for-java-developers"></a><span data-ttu-id="c58d3-103">適用於 Java 開發人員的 Azure 工具</span><span class="sxs-lookup"><span data-stu-id="c58d3-103">Azure tools for Java developers</span></span>

## <a name="supported-jdk-runtimes"></a><span data-ttu-id="c58d3-104">支援的 JDK 執行階段</span><span class="sxs-lookup"><span data-stu-id="c58d3-104">Supported JDK runtimes</span></span>

<span data-ttu-id="c58d3-105">在 Azure 和 Azure Stack 上的 Java 開發人員可以使用 [OpenJDK 的 Azul Systems Zulu Enterprise 組建](https://www.azul.com/downloads/azure-only/zulu/)建置及執行生產環境使用的 Java 7、8、和 11 應用程式，而不會產生額外的支援成本。</span><span class="sxs-lookup"><span data-stu-id="c58d3-105">Java developers on Azure and Azure Stack can build and run production Java 7,8, and 11 applications using [Azul Systems Zulu Enterprise builds of OpenJDK](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="c58d3-106">如果您目前使用其他 JDK 執行 Java 應用程式，請考慮移轉至 Azure 上的 Zulu 以取得免費支援和維護。</span><span class="sxs-lookup"><span data-stu-id="c58d3-106">If you’re currently running Java apps with other JDKs, consider migrating to Zulu on Azure for free support and maintenance.</span></span> 

<span data-ttu-id="c58d3-107">[深入了解](java-supported-jdk-runtime.md) Azure 支援的 Java 執行階段。</span><span class="sxs-lookup"><span data-stu-id="c58d3-107">[Learn more](java-supported-jdk-runtime.md) about Azure supported Java runtimes.</span></span>

## <a name="eclipse-and-intellij-plugins"></a><span data-ttu-id="c58d3-108">Eclipse 和 IntelliJ 外掛程式</span><span class="sxs-lookup"><span data-stu-id="c58d3-108">Eclipse and IntelliJ plugins</span></span>

<span data-ttu-id="c58d3-109">使用適用於 [Eclipse](eclipse/azure-toolkit-for-eclipse.md) 和 [IntelliJ](intellij/azure-toolkit-for-intellij.md) 的 Azure 工具組從您的 IDE 管理 Azure 資源並部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="c58d3-109">Manage Azure resources and deploy apps from your IDE with The Azure toolkits for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) and [IntelliJ](intellij/azure-toolkit-for-intellij.md).</span></span>   

![顯示 Azure Explorer 的 IntelliJ 工具組](media/intelliJ-azure-explorer.png)

<span data-ttu-id="c58d3-111">[開始使用適用於 Eclipse 的 Azure 工具組](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [開始使用適用於 IntelliJ 的 Azure 工具組](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span><span class="sxs-lookup"><span data-stu-id="c58d3-111">[Get started with Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Get started with Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span></span> 

## <a name="visual-studio-code"></a><span data-ttu-id="c58d3-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c58d3-112">Visual Studio Code</span></span>

<span data-ttu-id="c58d3-113">[VS Code](https://code.visualstudio.com/) 是適用於 MacOS、Windows 和 Linux 的程式碼編輯器，雖輕量卻功能強大。</span><span class="sxs-lookup"><span data-stu-id="c58d3-113">[VS Code](https://code.visualstudio.com/) is a lightweight but powerful code editor available for MacOS, Windows, and Linux.</span></span> <span data-ttu-id="c58d3-114">VS Code 可透過一組延伸模組來支援簡單、新型的 Java 開發工作流程，這組延伸模組可提供專案支援、程式碼完成、偵錯、Linting 和瀏覽。</span><span class="sxs-lookup"><span data-stu-id="c58d3-114">VS Code supports a simple, modern Java development workflow through a set of extensions that provide project support, code completion, debugging, linting, and navigation.</span></span>

<span data-ttu-id="c58d3-115">[開始使用 VS Code 和 Java](https://code.visualstudio.com/docs/java)
[適用於 VS Code 的 Java 延伸套件](https://code.visualstudio.com/docs/java/extensions)</span><span class="sxs-lookup"><span data-stu-id="c58d3-115">[Get Started with VS Code and Java](https://code.visualstudio.com/docs/java)
[Java extension pack for VS Code](https://code.visualstudio.com/docs/java/extensions)</span></span>  

## <a name="azure-cli-20"></a><span data-ttu-id="c58d3-116">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c58d3-116">Azure CLI 2.0</span></span>

<span data-ttu-id="c58d3-117">Azure CLI 2.0 提供了命令列體驗供您管理 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="c58d3-117">The Azure 2.0 CLI provides a command-line experience to manage Azure resources.</span></span> <span data-ttu-id="c58d3-118">您可以透過 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) 在瀏覽器中使用，或者在 macOS、Linux 和 Windows 中[安裝](https://docs.microsoft.com/cli/azure/install-azure-cli)並從命令列中執行。</span><span class="sxs-lookup"><span data-stu-id="c58d3-118">You can use it in your browser with [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), or you can [install](https://docs.microsoft.com/cli/azure/install-azure-cli) it on macOS, Linux, and Windows and run it from the command line.</span></span>

<span data-ttu-id="c58d3-119">[開始使用 Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="c58d3-119">[Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>

## <a name="azure-storage-explorer"></a><span data-ttu-id="c58d3-120">Azure 儲存體總管</span><span class="sxs-lookup"><span data-stu-id="c58d3-120">Azure Storage Explorer</span></span> 

<span data-ttu-id="c58d3-121">從桌面管理 Azure 儲存體帳戶、容器和 blob/檔案。</span><span class="sxs-lookup"><span data-stu-id="c58d3-121">Manage Azure storage accounts, containers, and blobs/files from your desktop.</span></span> <span data-ttu-id="c58d3-122">Azure 儲存體總管目前為預覽狀態，可在 Windows、macOS 和 Linux 上運作。</span><span class="sxs-lookup"><span data-stu-id="c58d3-122">Azure Storage Explorer is currently in Preview and works on Windows, macOS, and Linux.</span></span>

[<span data-ttu-id="c58d3-123">開始使用 Azure 儲存體總管</span><span class="sxs-lookup"><span data-stu-id="c58d3-123">Get started with Azure Storage Explorer</span></span>](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
