---
title: Azure 開發的 Java JDK 和長期支援
description: Azure 支援的下載與聲明，以用於開發和執行 Java 應用程式。
author: rloutlaw
manager: angerobe
ms.devlang: java
ms.topic: article
ms.date: 11/13/2018
ms.author: routlaw
ms.openlocfilehash: 0ced17eb17c9d2eda7b1fcc12ffff27b303e8d7e
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339032"
---
# <a name="get-java-jdk-downloads-and-support-when-developing-for-azure"></a><span data-ttu-id="3cfdb-103">開發 Azure 時取得 Java JDK 下載和支援</span><span class="sxs-lookup"><span data-stu-id="3cfdb-103">Get Java JDK downloads and support when developing for Azure</span></span>

<span data-ttu-id="3cfdb-104">在 Azure 和 Azure Stack 上的 Java 開發人員可以使用 [Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) 建置及執行生產環境使用的 Java 應用程式，而不會產生額外的支援成本。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-104">Java developers on Azure and Azure Stack can build and run production Java applications using [Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="3cfdb-105">您可以在 Azure 上使用任何想要的 Java 執行階段，但是若使用 Zulu 時，可取得免費的維護更新，且可使用[合格 Azure 支援方案](https://azure.microsoft.com/support/plans/)建立 Microsoft 支援問題。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-105">You can use any Java runtime you want on Azure, but when you use Zulu you get free maintenance updates and can create support issues with Microsoft with a  [qualified Azure support plan](https://azure.microsoft.com/support/plans/).</span></span>

<span data-ttu-id="3cfdb-106">開發人員可以使用自己的 Java 執行階段 (包括 Oracle JDK 和 Red Hat JDK)，在 Azure 上執行應用程式及連線至各項 Azure 服務和功能。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-106">Developers can use their own Java runtimes, including Oracle JDK and Red Hat JDK, to run their apps on Azure and connect to Azure services and features.</span></span> <span data-ttu-id="3cfdb-107">Oracle Java SE 的實際執行版本仍然能夠繼續供 Java 開發人員在 Azure Windows 或 Linux 虛擬機器中執行工作負載。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-107">The production edition of Oracle Java SE continues to be available to Java developers running  workloads in Azure Windows or Linux virtual machines.</span></span>

## <a name="supported-java-versions-and-update-schedule"></a><span data-ttu-id="3cfdb-108">支援的 Java 版本及更新排程</span><span class="sxs-lookup"><span data-stu-id="3cfdb-108">Supported Java versions and update schedule</span></span>

<span data-ttu-id="3cfdb-109">Azul Systems 將提供完整支援的[適用於 Microsoft Azure 的 OpenJDK Zulu Enterprise 組建](https://www.azul.com/downloads/azure-only/zulu/)，用於所有長期支援 (LTS) 版本的 Java，從 Java SE 7、8 和 11 開始。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-109">Azul Systems will provide fully-supported [Zulu Enterprise builds of OpenJDK for Microsoft Azure](https://www.azul.com/downloads/azure-only/zulu/) for all long-term support (LTS) versions of Java, starting with Java SE 7, 8, and 11.</span></span> <span data-ttu-id="3cfdb-110">[Azul 新聞稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)中可找到詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-110">More information can be found in the [Azul press release](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).</span></span>


<span data-ttu-id="3cfdb-111">這些 JDK 版本會有每季的安全性更新和錯誤修正，並視需要提供頻外的重大更新和修補程式。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-111">These JDK releases will have quarterly security updates and bug fixes as well as critical out-of-band updates and patches as needed.</span></span>  <span data-ttu-id="3cfdb-112">這項支援包含將安全性更新和錯誤修正移轉至 Java 7 和 8 報告於較新版本的 Java，如 Java 11，並確保較舊的 Java 版本維持穩定性和安全性。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-112">This support includes back porting of security updates and bug fixes to Java 7 and 8 reported in newer versions of Java such as Java 11, and ensures the continued stability and security of older versions of Java.</span></span>  <span data-ttu-id="3cfdb-113">Azure 客戶有權使用這些安全性更新與平台錯誤修正，而不會造成任何非計劃的 Java SE 訂用帳戶費用。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-113">Azure customers are entitled to these security updates and platform bug fixes without incurring any unplanned Java SE subscription fees.</span></span> <span data-ttu-id="3cfdb-114">每個版本 Java SE 的支援日期會在下圖中醒目顯示。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-114">The dates of support for each version of Java SE are highlighted in the image below.</span></span>

![Azure 的 JDK 支援時間軸](media/azure-jdk-support.png)

<span data-ttu-id="3cfdb-116">Azul Systems 可維護這些版本的 [Java SE 藍圖](https://www.azul.com/products/azul_support_roadmap/)。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-116">Azul Systems maintains a [Java SE roadmap](https://www.azul.com/products/azul_support_roadmap/) for these releases.</span></span>

## <a name="use-for-local-development"></a><span data-ttu-id="3cfdb-117">本機開發使用</span><span class="sxs-lookup"><span data-stu-id="3cfdb-117">Use for local development</span></span> 

<span data-ttu-id="3cfdb-118">開發人員可以[下載](https://www.azul.com/downloads/azure-only/zulu/)適用於 Azure 和 Azure Stack 的 Java JDK 以便在本機開發環境中使用。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-118">Developers can [download](https://www.azul.com/downloads/azure-only/zulu/) Java JDKs for Azure and Azure Stack for use in local devlopment environments.</span></span> <span data-ttu-id="3cfdb-119">下載適用於 Windows、Linux 和 macOS。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-119">Downloads are available for Windows, Linux, and macOS.</span></span> <span data-ttu-id="3cfdb-120">使用 Linux 的開發人員也可以透過 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 套件管理員取得套件。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-120">Developers working on Linux can also get packages through the  [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) and [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) package managers.</span></span>

<span data-ttu-id="3cfdb-121">在開發 Azure 或 Azure Stack 相關項目時，[合格的 Azure 支援方案](https://azure.microsoft.com/support/plans/)會提供適用於 JDK 的本機開發支援。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-121">Support for local development for the JDK is available with a [qualified Azure support plan](https://azure.microsoft.com/support/plans/) when developing for Azure or Azure Stack.</span></span>

## <a name="use-in-docker-containers"></a><span data-ttu-id="3cfdb-122">在 Docker 容器中使用</span><span class="sxs-lookup"><span data-stu-id="3cfdb-122">Use in Docker containers</span></span>

<span data-ttu-id="3cfdb-123">您可以使用 OpenJDK 的 Zulu Enterprise 組建，在選擇的任何散發版本上建置無限制的 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-123">You can build unlimited Docker images using Zulu Enterprise builds of OpenJDK on any distros of your choice.</span></span> <span data-ttu-id="3cfdb-124">以 Azure JDK 的 Azul Zulu Enterprise 為基礎的 Zulu Docker 映像可在 [Microsoft 公用 Docker 存放庫](https://hub.docker.com/r/microsoft/java-jdk/)中取得。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-124">Zulu Docker images based off the Azul Zulu Enterprise for Azure JDKs are available on [Microsoft's public Docker repository](https://hub.docker.com/r/microsoft/java-jdk/).</span></span> <span data-ttu-id="3cfdb-125">用來建置這些映像的 Dockerfile 位於 [Microsoft Java GitHub 存放庫](https://github.com/Microsoft/java/tree/master/docker)中。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-125">The  Dockerfiles that used to build these images are available on [Microsoft's Java GitHub repo](https://github.com/Microsoft/java/tree/master/docker).</span></span>

<span data-ttu-id="3cfdb-126">若要使用這些映像容器化應用程式，您必須在 Dockerfile 中設定 `FROM` 陳述式，然後設定容器與應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-126">To containerize your apps using these images, you will need to set a `FROM` statement in your Dockerfile and then configure the container with your application's dependencies.</span></span> <span data-ttu-id="3cfdb-127">例如，若要執行繫結至連接埠 8080 的 JAR 檔案封裝 Java SE 應用程式：</span><span class="sxs-lookup"><span data-stu-id="3cfdb-127">For example, to run a JAR file packaged Java SE application that binds to port 8080:</span></span>

```Dockerfile
FROM  microsoft/java-jdk:<tag>
EXPOSE 8080
ADD target/hello.jar hello.jar
ENTRYPOINT ["java", "-jar","/hello.jar"]
```

## <a name="azure-service-runtime-support"></a><span data-ttu-id="3cfdb-128">Azure 服務執行階段支援</span><span class="sxs-lookup"><span data-stu-id="3cfdb-128">Azure service runtime support</span></span>

<span data-ttu-id="3cfdb-129">Azure 平台服務，例如 [App Service](/azure/app-service/containers/)、[Azure Functions](/azure/azure-functions/functions-create-first-java-maven)、[Service Fabric](/azure/service-fabric/) 和 [HDInsight](/azure/hdinsight/) 使用 OpenJDK 的 Zulu Enterprise 組建，內建 Java 次要版本的自動修補功能與安全性修補程式和錯誤修正。</span><span class="sxs-lookup"><span data-stu-id="3cfdb-129">Azure platform services such as [App Service](/azure/app-service/containers/), [Functions](/azure/azure-functions/functions-create-first-java-maven), [Service Fabric](/azure/service-fabric/) and [HDInsight](/azure/hdinsight/)  use Zulu Enterprise builds of OpenJDK with built-in auto-patching of minor releases of Java with security patches and bug fixes.</span></span>