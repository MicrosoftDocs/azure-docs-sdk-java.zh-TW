---
title: Azure 開發的 Java JDK 和長期支援
description: Azure 支援的下載與聲明，以用於開發和執行 Java 應用程式。
author: rloutlaw
manager: angerobe
ms.devlang: java
ms.topic: article
ms.date: 10/15/2017
ms.author: routlaw
ms.openlocfilehash: 2865219f350990e8b07f7d2cd99f536168a6b8d4
ms.sourcegitcommit: 7df2d442ad7cbdb235e5dd35302a9b73379c23d5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026994"
---
# <a name="get-java-jdk-downloads-and-support-when-developing-for-azure"></a><span data-ttu-id="73907-103">開發 Azure 時取得 Java JDK 下載和支援</span><span class="sxs-lookup"><span data-stu-id="73907-103">Get Java JDK downloads and support when developing for Azure</span></span>

<span data-ttu-id="73907-104">在 Azure 和 Azure Stack 上的 Java 開發人員可以使用 [OpenJDK 的 Azul Systems Zulu Enterprise 組建](https://www.azul.com/downloads/azure-only/zulu/)建置及執行生產環境使用的 Java 應用程式，而不會產生額外的支援成本。</span><span class="sxs-lookup"><span data-stu-id="73907-104">Java developers on Azure and Azure Stack can build and run production Java applications using [Azul Systems Zulu Enterprise builds of OpenJDK](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="73907-105">您可以在 Azure 上使用任何想要的 Java 執行階段，但是若使用 Zulu 時，可取得免費的維護更新，且可使用[合格 Azure 支援方案](https://azure.microsoft.com/support/plans/)建立 Microsoft 支援問題。</span><span class="sxs-lookup"><span data-stu-id="73907-105">You can use any Java runtime you want on Azure, but when you use Zulu you get free maintenance updates and can create support issues with Microsoft with a  [qualified Azure support plan](https://azure.microsoft.com/support/plans/).</span></span>

## <a name="supported-java-versions-and-update-schedule"></a><span data-ttu-id="73907-106">支援的 Java 版本及更新排程</span><span class="sxs-lookup"><span data-stu-id="73907-106">Supported Java versions and update schedule</span></span>

<span data-ttu-id="73907-107">Azul Systems 將提供完整支援的[適用於 Microsoft Azure 的 OpenJDK Zulu Enterprise 組建](https://www.azul.com/downloads/azure-only/zulu/)，用於所有長期支援 (LTS) 版本的 Java，從 Java SE 7、8 和 11 開始。</span><span class="sxs-lookup"><span data-stu-id="73907-107">Azul Systems will provide fully-supported [Zulu Enterprise builds of OpenJDK for Microsoft Azure](https://www.azul.com/downloads/azure-only/zulu/) for all long-term support (LTS) versions of Java, starting with Java SE 7, 8, and 11.</span></span> <span data-ttu-id="73907-108">[Azul 新聞稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)中可找到詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="73907-108">More information can be found in the [Azul press release](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).</span></span>


<span data-ttu-id="73907-109">這些 JDK 版本會有每季的安全性更新和錯誤修正，並視需要提供頻外的重大更新和修補程式。</span><span class="sxs-lookup"><span data-stu-id="73907-109">These JDK releases will have quarterly security updates and bug fixes as well as critical out-of-band updates and patches as needed.</span></span>  <span data-ttu-id="73907-110">這項支援包含將安全性更新和錯誤修正移轉至 Java 7 和 8 報告於較新版本的 Java，如 Java 11，並確保較舊的 Java 版本維持穩定性和安全性。</span><span class="sxs-lookup"><span data-stu-id="73907-110">This support includes back porting of security updates and bug fixes to Java 7 and 8 reported in newer versions of Java such as Java 11, and ensures the continued stability and security of older versions of Java.</span></span>  <span data-ttu-id="73907-111">Azure 客戶有權使用這些安全性更新與平台錯誤修正，而不會造成任何非計劃的 Java SE 訂用帳戶費用。</span><span class="sxs-lookup"><span data-stu-id="73907-111">Azure customers are entitled to these security updates and platform bug fixes without incurring any unplanned Java SE subscription fees.</span></span> <span data-ttu-id="73907-112">每個版本 Java SE 的支援日期會在下圖中醒目顯示。</span><span class="sxs-lookup"><span data-stu-id="73907-112">The dates of support for each version of Java SE are highlighted in the image below.</span></span>

![Azure 的 JDK 支援時間軸](media/azure-jdk-support.png)

<span data-ttu-id="73907-114">Azul Systems 可維護這些版本的 [Java SE 藍圖](https://www.azul.com/products/azul_support_roadmap/)。</span><span class="sxs-lookup"><span data-stu-id="73907-114">Azul Systems maintains a [Java SE roadmap](https://www.azul.com/products/azul_support_roadmap/) for these releases.</span></span>

## <a name="use-for-local-development"></a><span data-ttu-id="73907-115">本機開發使用</span><span class="sxs-lookup"><span data-stu-id="73907-115">Use for Local development</span></span> 

<span data-ttu-id="73907-116">開發人員可以從 [Azul Systems 的網站](https://www.azul.com/downloads/azure-only/zulu/)下載適用於 Azure 和 Azure Stack 的 Java JDK。</span><span class="sxs-lookup"><span data-stu-id="73907-116">Developers can download Java JDKs for Azure and Azure Stack from [Azul Systems' website](https://www.azul.com/downloads/azure-only/zulu/).</span></span> <span data-ttu-id="73907-117">下載適用於 Windows、Linux 和 macOS。</span><span class="sxs-lookup"><span data-stu-id="73907-117">Downloads are available for Windows, Linux, and macOS.</span></span> <span data-ttu-id="73907-118">使用 Linux 的開發人員也可以透過 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 套件管理員取得套件。</span><span class="sxs-lookup"><span data-stu-id="73907-118">Developers working on Linux can also get packages through the  [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) and [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) package managers.</span></span>

<span data-ttu-id="73907-119">若透過[合格的 Azure 支援方案](https://azure.microsoft.com/support/plans/)開發 Azure 或 Azure Stack，則會提供支援 Azure 的 Azul Zulu JDK 產品支援。</span><span class="sxs-lookup"><span data-stu-id="73907-119">Product support for the Azure-supported Azul Zulu JDK is available through when developing for Azure or Azure Stack with a [qualified Azure support plan](https://azure.microsoft.com/support/plans/).</span></span>

## <a name="use-in-docker-containers"></a><span data-ttu-id="73907-120">在 Docker 容器中使用</span><span class="sxs-lookup"><span data-stu-id="73907-120">Use in Docker containers</span></span>

<span data-ttu-id="73907-121">您可以使用 OpenJDK 的 Zulu Enterprise 組建，在選擇的任何散發版本上建置無限制的 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="73907-121">You can build unlimited Docker images using Zulu Enterprise builds of OpenJDK on any distros of your choice.</span></span> <span data-ttu-id="73907-122">以 Azure JDK 的 Azul Zulu Enterprise 為基礎的 Zulu Docker 映像可在 [Microsoft 公用 Docker 存放庫](https://hub.docker.com/r/microsoft/java-jdk/)中取得。</span><span class="sxs-lookup"><span data-stu-id="73907-122">Zulu Docker images based off the Azul Zulu Enterprise for Azure JDKs are available on [Microsoft's public Docker repository](https://hub.docker.com/r/microsoft/java-jdk/).</span></span> <span data-ttu-id="73907-123">用來建置這些映像的 Dockerfile 位於 [Microsoft Java GitHub 存放庫](https://github.com/Microsoft/java/tree/master/docker)中。</span><span class="sxs-lookup"><span data-stu-id="73907-123">The  Dockerfiles that used to build these images are available on [Microsoft's Java GitHub repo](https://github.com/Microsoft/java/tree/master/docker).</span></span>

<span data-ttu-id="73907-124">若要使用這些映像容器化應用程式，您必須在 Dockerfile 中設定 `FROM` 陳述式，然後設定容器與應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="73907-124">To containerize your apps using these images, you will need to set a `FROM` statement in your Dockerfile and then configure the container with your application's dependencies.</span></span> <span data-ttu-id="73907-125">例如，若要執行繫結至連接埠 8080 的 JAR 檔案封裝 Java SE 應用程式：</span><span class="sxs-lookup"><span data-stu-id="73907-125">For example, to run a JAR file packaged Java SE application that binds to port 8080:</span></span>

```Dockerfile
FROM  microsoft/java-jdk:<tag>
EXPOSE 8080
ADD target/hello.jar hello.jar
ENTRYPOINT ["java", "-jar","/hello.jar"]
```

## <a name="azure-service-runtime-support"></a><span data-ttu-id="73907-126">Azure 服務執行階段支援</span><span class="sxs-lookup"><span data-stu-id="73907-126">Azure service runtime support</span></span>

<span data-ttu-id="73907-127">Azure 平台服務，例如 [App Service](/azure/app-service/containers/)、[Azure Functions](/azure/azure-functions/functions-create-first-java-maven)、[Service Fabric](/azure/service-fabric/) 和 [HDInsight](/azure/hdinsight/) 使用 OpenJDK 的 Zulu Enterprise 組建，內建 Java 次要版本的自動修補功能與安全性修補程式和錯誤修正。</span><span class="sxs-lookup"><span data-stu-id="73907-127">Azure platform services such as [App Service](/azure/app-service/containers/), [Functions](/azure/azure-functions/functions-create-first-java-maven), [Service Fabric](/azure/service-fabric/) and [HDInsight](/azure/hdinsight/)  use Zulu Enterprise builds of OpenJDK with built-in auto-patching of minor releases of Java with security patches and bug fixes.</span></span>