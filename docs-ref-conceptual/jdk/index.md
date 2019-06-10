---
title: Azure 開發的 Java JDK 和長期支援
description: Azure 支援的下載與聲明，以用於開發和執行 Java 應用程式。
author: bmitchell287
manager: douge
ms.devlang: java
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: brendm
ms.openlocfilehash: 340f6149ffe39ccbb2d7de534ed63b8784d1f63a
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270887"
---
# <a name="java-long-term-support-for-azure-and-azure-stack"></a><span data-ttu-id="cab84-103">Java 對於 Azure 和 Azure Stack 的長期支援</span><span class="sxs-lookup"><span data-stu-id="cab84-103">Java Long-Term Support for Azure and Azure Stack</span></span>

<span data-ttu-id="cab84-104">在 Azure 和 Azure Stack 上的 Java 開發人員可以使用 [Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) 建置及執行生產環境使用的 Java 應用程式，而不會產生額外的支援成本。</span><span class="sxs-lookup"><span data-stu-id="cab84-104">Java developers on Azure and Azure Stack can build and run production Java applications using [Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="cab84-105">您可以在 Azure 上使用任何想要的 Java 執行階段，但在使用 Zulu 時，您可以取得免費的維護更新，並且可建立 Microsoft 支援問題。</span><span class="sxs-lookup"><span data-stu-id="cab84-105">You can use any Java runtime you want on Azure, but when you use Zulu you get free maintenance updates and can create support issues with Microsoft.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cab84-106">下載並安裝 Java</span><span class="sxs-lookup"><span data-stu-id="cab84-106">Download and install Java</span></span>](java-jdk-install.md)

## <a name="long-term-support-lts"></a><span data-ttu-id="cab84-107">長期支援 (LTS)</span><span class="sxs-lookup"><span data-stu-id="cab84-107">Long-term support (LTS)</span></span>

* [<span data-ttu-id="cab84-108">Java 11</span><span class="sxs-lookup"><span data-stu-id="cab84-108">Java 11</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java11)
* [<span data-ttu-id="cab84-109">Java 8</span><span class="sxs-lookup"><span data-stu-id="cab84-109">Java 8</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java8)
* [<span data-ttu-id="cab84-110">Java 7</span><span class="sxs-lookup"><span data-stu-id="cab84-110">Java 7</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java7)

## <a name="technical-preview"></a><span data-ttu-id="cab84-111">技術預覽</span><span class="sxs-lookup"><span data-stu-id="cab84-111">Technical preview</span></span>

* [<span data-ttu-id="cab84-112">Java 12</span><span class="sxs-lookup"><span data-stu-id="cab84-112">Java 12</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java12)

## <a name="what-is-the-zulu-openjdk-for-azure"></a><span data-ttu-id="cab84-113">什麼是適用於 Azure 的 Zulu OpenJDK？</span><span class="sxs-lookup"><span data-stu-id="cab84-113">What is the Zulu OpenJDK for Azure?</span></span>

<span data-ttu-id="cab84-114">OpenJDK 的 Azul Zulu Enterprise 組建是免費、多平台、可實際執行的 OpenJDK 散發套件，適用於 Azure 和 Azure Stack，由 Microsoft 與 Azul Systems 提供支援。</span><span class="sxs-lookup"><span data-stu-id="cab84-114">Azul Zulu Enterprise builds of OpenJDK are a no-cost, multi-platform, production-ready distribution of the OpenJDK for Azure and Azure Stack backed by Microsoft and Azul Systems.</span></span> <span data-ttu-id="cab84-115">這些散發套件：</span><span class="sxs-lookup"><span data-stu-id="cab84-115">These distributions are:</span></span>

* <span data-ttu-id="cab84-116">是封裝為 Java Development Kit (JDK)、Java Runtime Environment (JRE) 和無周邊 JRE 的純開放原始碼 OpenJDK 組建。</span><span class="sxs-lookup"><span data-stu-id="cab84-116">100% open source builds of OpenJDK packaged as Java Development Kits (JDKs), Java Runtime Environments (JREs), and Headless JREs.</span></span> <span data-ttu-id="cab84-117">這些二進位檔是完全相容且符合規範的商業 Java Standard Edition (SE) 組建，可與 Azure 和 Azure Stack 上的 Java 應用程式或元件搭配使用。</span><span class="sxs-lookup"><span data-stu-id="cab84-117">These binaries are fully compatible and compliant commercial builds of Java Standard Edition (SE) that can be used with Java applications or components on Azure and Azure Stack.</span></span>
* <span data-ttu-id="cab84-118">具備長期支援，包括錯誤修正、效能增強和安全性修補程式。</span><span class="sxs-lookup"><span data-stu-id="cab84-118">Provided with long-term support including bug fixes, performance enhancements, and security patches.</span></span>
* <span data-ttu-id="cab84-119">可用來在 Windows、Linux 和 MacOS 上開發和執行 Java 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cab84-119">Available for developing and running Java applications on Windows, Linux, and MacOS.</span></span>
* <span data-ttu-id="cab84-120">可作為 Docker Hub 上的容器映像，以及 Azure Marketplace 上的虛擬機器 (Windows 和 Linux)。</span><span class="sxs-lookup"><span data-stu-id="cab84-120">Available as container images on Docker Hub and as virtual machines (Windows and Linux) on Azure Marketplace.</span></span>
* <span data-ttu-id="cab84-121">供 Microsoft Azure 用來支援許多 Azure 服務，例如：</span><span class="sxs-lookup"><span data-stu-id="cab84-121">Used by Microsoft Azure to power many Azure services, such as:</span></span>
  * <span data-ttu-id="cab84-122">App Service Windows</span><span class="sxs-lookup"><span data-stu-id="cab84-122">App Service Windows</span></span>
  * <span data-ttu-id="cab84-123">App Service Linux</span><span class="sxs-lookup"><span data-stu-id="cab84-123">App Service Linux</span></span>
  * <span data-ttu-id="cab84-124">Functions</span><span class="sxs-lookup"><span data-stu-id="cab84-124">Functions</span></span>
  * <span data-ttu-id="cab84-125">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="cab84-125">Service Fabric</span></span>
  * <span data-ttu-id="cab84-126">HDInsight</span><span class="sxs-lookup"><span data-stu-id="cab84-126">HDInsight</span></span>
  * <span data-ttu-id="cab84-127">Search</span><span class="sxs-lookup"><span data-stu-id="cab84-127">Search</span></span>
  * <span data-ttu-id="cab84-128">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="cab84-128">Azure DevOps</span></span>
  * <span data-ttu-id="cab84-129">Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="cab84-129">Cloud Shell</span></span>  

## <a name="supported-java-versions-and-update-schedule"></a><span data-ttu-id="cab84-130">支援的 Java 版本及更新排程</span><span class="sxs-lookup"><span data-stu-id="cab84-130">Supported Java versions and update schedule</span></span>

<span data-ttu-id="cab84-131">Azul Systems 提供完整支援的[適用於 Microsoft Azure 的 OpenJDK Zulu Enterprise 組建](https://www.azul.com/downloads/azure-only/zulu/)，用於所有長期支援 (LTS) 版本的 Java，從 Java SE 7、8 和 11 開始。</span><span class="sxs-lookup"><span data-stu-id="cab84-131">Azul Systems provides fully-supported [Zulu Enterprise builds of OpenJDK for Microsoft Azure](https://www.azul.com/downloads/azure-only/zulu/) for all long-term support (LTS) versions of Java, starting with Java SE 7, 8, and 11.</span></span> <span data-ttu-id="cab84-132">[Azul 新聞稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)中可找到詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="cab84-132">More information can be found in the [Azul press release](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).</span></span>

|<span data-ttu-id="cab84-133">Java SE LTS</span><span class="sxs-lookup"><span data-stu-id="cab84-133">Java SE LTS</span></span>  |<span data-ttu-id="cab84-134">支援期限</span><span class="sxs-lookup"><span data-stu-id="cab84-134">Support until</span></span>  |
|---------|----------|
|<span data-ttu-id="cab84-135">[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7)</span><span class="sxs-lookup"><span data-stu-id="cab84-135">[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7)</span></span> |<span data-ttu-id="cab84-136">2023 年 7 月</span><span class="sxs-lookup"><span data-stu-id="cab84-136">July 2023</span></span> |
|<span data-ttu-id="cab84-137">[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8)</span><span class="sxs-lookup"><span data-stu-id="cab84-137">[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8)</span></span> |<span data-ttu-id="cab84-138">2025 年 3 月</span><span class="sxs-lookup"><span data-stu-id="cab84-138">March 2025</span></span>|
|<span data-ttu-id="cab84-139">[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11)</span><span class="sxs-lookup"><span data-stu-id="cab84-139">[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11)</span></span> |<span data-ttu-id="cab84-140">2026 年 9 月</span><span class="sxs-lookup"><span data-stu-id="cab84-140">Sept. 2026</span></span>|
|<span data-ttu-id="cab84-141">[![Java 12](../media/jdk/java-12.png)]()</span><span class="sxs-lookup"><span data-stu-id="cab84-141">[![Java 12](../media/jdk/java-12.png)]()</span></span> |<span data-ttu-id="cab84-142">**預覽**</span><span class="sxs-lookup"><span data-stu-id="cab84-142">**PREVIEW**</span></span>|

<span data-ttu-id="cab84-143">這些 JDK 版本會有每季的安全性更新和錯誤修正，並視需要提供頻外的重大更新和修補程式。</span><span class="sxs-lookup"><span data-stu-id="cab84-143">These JDK releases have quarterly security updates, bug fixes, and critical out-of-band updates and patches as needed.</span></span>  <span data-ttu-id="cab84-144">這項支援包括可回移將在較新版本的 Java (如 Java 11) 中報告的 Java 7 和 8 安全性更新和錯誤修正，以確保較舊的 Java 版本維持穩定性和安全性。</span><span class="sxs-lookup"><span data-stu-id="cab84-144">This support includes back ports of security updates and bug fixes to Java 7 and 8 reported in newer versions of Java such as Java 11, which ensures the continued stability and security of older versions of Java.</span></span>  <span data-ttu-id="cab84-145">Azure 客戶可取得這些安全性更新與平台錯誤修正，而不會造成任何非計劃的 Java SE 訂用帳戶費用。</span><span class="sxs-lookup"><span data-stu-id="cab84-145">Azure customers can get these security updates and platform bug fixes without incurring any unplanned Java SE subscription fees.</span></span>

<span data-ttu-id="cab84-146">Azul Systems 可維護這些版本的 [Java SE 藍圖](https://www.azul.com/products/azul_support_roadmap/)。</span><span class="sxs-lookup"><span data-stu-id="cab84-146">Azul Systems maintains a [Java SE roadmap](https://www.azul.com/products/azul_support_roadmap/) for these releases.</span></span>

## <a name="benefits-for-developers"></a><span data-ttu-id="cab84-147">對開發人員的好處</span><span class="sxs-lookup"><span data-stu-id="cab84-147">Benefits for developers</span></span>

<span data-ttu-id="cab84-148">Azul Zulu JDK 版本：</span><span class="sxs-lookup"><span data-stu-id="cab84-148">The Azul Zulu JDK releases are:</span></span>

1. <span data-ttu-id="cab84-149">同時受到 Microsoft 和 Azul Systems 的支援</span><span class="sxs-lookup"><span data-stu-id="cab84-149">Backed and supported by both Microsoft and Azul Systems</span></span>

   * <span data-ttu-id="cab84-150">Zulu 二進位檔可供實際執行之用，並且受到 Microsoft 和 Azul Systems 的支援</span><span class="sxs-lookup"><span data-stu-id="cab84-150">Zulu binaries are production-ready and backed by Microsoft and Azul Systems</span></span>
   * <span data-ttu-id="cab84-151">Zulu 隨 Java 7、8 和 11 的附零成本長期支援 (LTS)。</span><span class="sxs-lookup"><span data-stu-id="cab84-151">Zulu comes with zero-cost long-term support (LTS) for Java 7, 8, and 11.</span></span> <span data-ttu-id="cab84-152">(針對 Java 17 也將提供 LTS)。</span><span class="sxs-lookup"><span data-stu-id="cab84-152">(LTS will be provided for Java 17, as well).</span></span> <span data-ttu-id="cab84-153">您可以在需要時才升級 Java 版本。</span><span class="sxs-lookup"><span data-stu-id="cab84-153">You can upgrade Java versions only when you need to.</span></span>
   * <span data-ttu-id="cab84-154">Java 7 的支援期限為 2023 年 7 月。</span><span class="sxs-lookup"><span data-stu-id="cab84-154">Java 7 supported until July 2023.</span></span> <span data-ttu-id="cab84-155">Java 8 和 11 在 2024 年之後仍受支援。</span><span class="sxs-lookup"><span data-stu-id="cab84-155">Java 8 and 11 are supported beyond 2024.</span></span>
   * <span data-ttu-id="cab84-156">Microsoft 致力於在支援多項 Azure 服務的內部機器上執行 Zulu。</span><span class="sxs-lookup"><span data-stu-id="cab84-156">Microsoft is committed to running Zulu internally on machines that power many Azure services.</span></span>

2. <span data-ttu-id="cab84-157">可供實際執行之用</span><span class="sxs-lookup"><span data-stu-id="cab84-157">Production-ready</span></span>

   * <span data-ttu-id="cab84-158">其 OpenJDK 組建採用 100% 開放原始碼。</span><span class="sxs-lookup"><span data-stu-id="cab84-158">100% open-source for its builds of OpenJDK.</span></span>
   * <span data-ttu-id="cab84-159">許多 Java SE 散發套件皆為簡易替換。</span><span class="sxs-lookup"><span data-stu-id="cab84-159">Drop-in replacements for many Java SE distributions.</span></span>
   * <span data-ttu-id="cab84-160">JDK、JRE 和無周邊 JRE</span><span class="sxs-lookup"><span data-stu-id="cab84-160">JDK, JRE, and JRE-headless</span></span>
   * <span data-ttu-id="cab84-161">Java 7、8 和 11</span><span class="sxs-lookup"><span data-stu-id="cab84-161">Java 7, 8, and 11</span></span>
   * <span data-ttu-id="cab84-162">經驗證與使用 OpenJDK Community Technology Compatibility Kit (TCK) 的 Java SE 規格相容。</span><span class="sxs-lookup"><span data-stu-id="cab84-162">Verified compliant with the Java SE  specifications using the OpenJDK Community Technology Compatibility Kit (TCK).</span></span>
   * <span data-ttu-id="cab84-163">開發人員將持續取得 Java SE 的生產更新，包括 Java SE 7、8 和 11 的錯誤修正、效能增強和安全性修補程式。</span><span class="sxs-lookup"><span data-stu-id="cab84-163">Developers will continue to receive production updates for Java SE, including bug fixes, performance enhancements, and security patches for Java SE 7, 8, and 11.</span></span>

3. <span data-ttu-id="cab84-164">支援多平台</span><span class="sxs-lookup"><span data-stu-id="cab84-164">Supported for multi-platform.</span></span> <span data-ttu-id="cab84-165">Zulu 支援多平台和版本的二進位檔，包括：</span><span class="sxs-lookup"><span data-stu-id="cab84-165">Zulu supports binaries for multiple platforms and versions, including:</span></span>

   * <span data-ttu-id="cab84-166">Windows 用戶端</span><span class="sxs-lookup"><span data-stu-id="cab84-166">Windows Client</span></span>
     * <span data-ttu-id="cab84-167">10</span><span class="sxs-lookup"><span data-stu-id="cab84-167">10</span></span>
     * <span data-ttu-id="cab84-168">8.1</span><span class="sxs-lookup"><span data-stu-id="cab84-168">8.1</span></span>
     * <span data-ttu-id="cab84-169">8、7</span><span class="sxs-lookup"><span data-stu-id="cab84-169">8, 7</span></span>
   * <span data-ttu-id="cab84-170">Windows Server</span><span class="sxs-lookup"><span data-stu-id="cab84-170">Windows Server</span></span>
     * <span data-ttu-id="cab84-171">2016R2</span><span class="sxs-lookup"><span data-stu-id="cab84-171">2016R2</span></span>
     * <span data-ttu-id="cab84-172">2016</span><span class="sxs-lookup"><span data-stu-id="cab84-172">2016</span></span>
     * <span data-ttu-id="cab84-173">2012 R2</span><span class="sxs-lookup"><span data-stu-id="cab84-173">2012 R2</span></span>
     * <span data-ttu-id="cab84-174">2012</span><span class="sxs-lookup"><span data-stu-id="cab84-174">2012</span></span>
     * <span data-ttu-id="cab84-175">2008 R2</span><span class="sxs-lookup"><span data-stu-id="cab84-175">2008 R2</span></span>
   * <span data-ttu-id="cab84-176">Linux，包括</span><span class="sxs-lookup"><span data-stu-id="cab84-176">Linux, including</span></span>
     * <span data-ttu-id="cab84-177">RHEL</span><span class="sxs-lookup"><span data-stu-id="cab84-177">RHEL</span></span>
     * <span data-ttu-id="cab84-178">CentOS</span><span class="sxs-lookup"><span data-stu-id="cab84-178">CentOS</span></span>
     * <span data-ttu-id="cab84-179">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="cab84-179">Ubuntu</span></span>
     * <span data-ttu-id="cab84-180">SLES</span><span class="sxs-lookup"><span data-stu-id="cab84-180">SLES</span></span>
     * <span data-ttu-id="cab84-181">Debian</span><span class="sxs-lookup"><span data-stu-id="cab84-181">Debian</span></span>
     * <span data-ttu-id="cab84-182">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="cab84-182">Oracle Linux</span></span>
   * <span data-ttu-id="cab84-183">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="cab84-183">Mac OS X</span></span>
   * <span data-ttu-id="cab84-184">以多種套件類型提供：</span><span class="sxs-lookup"><span data-stu-id="cab84-184">delivered in multiple package types:</span></span>
     * <span data-ttu-id="cab84-185">MSI、ZIP、TAR、DEB、RPM 和 DMG</span><span class="sxs-lookup"><span data-stu-id="cab84-185">MSI, ZIP, TAR, DEB, RPM, and DMG</span></span>

    <span data-ttu-id="cab84-186">多基底 OS 映像上已針對 Zulu JDK、JRE 和無周邊 JRE 通過認證的 Docker 容器映像，可從 Docker 取得。</span><span class="sxs-lookup"><span data-stu-id="cab84-186">Certified Docker container images for Zulu JDK, JRE, and JRE-headless on multiple base OS images are available at Docker.</span></span>

    <span data-ttu-id="cab84-187">Hub：</span><span class="sxs-lookup"><span data-stu-id="cab84-187">Hub:</span></span>

    * [<span data-ttu-id="cab84-188">JDK</span><span class="sxs-lookup"><span data-stu-id="cab84-188">JDK</span></span>](https://hub.docker.com/_/microsoft-java-jdk)
    * [<span data-ttu-id="cab84-189">JRE</span><span class="sxs-lookup"><span data-stu-id="cab84-189">JRE</span></span>](https://hub.docker.com/_/microsoft-java-jre)
    * [<span data-ttu-id="cab84-190">無周邊 JRE</span><span class="sxs-lookup"><span data-stu-id="cab84-190">JRE-headless</span></span>](https://hub.docker.com/_/microsoft-java-jre-headless)

4. <span data-ttu-id="cab84-191">無成本</span><span class="sxs-lookup"><span data-stu-id="cab84-191">No cost</span></span>

   * <span data-ttu-id="cab84-192">Microsoft 免費提供您在 Azure 上建置及調整 Java 應用程式所需的一切。</span><span class="sxs-lookup"><span data-stu-id="cab84-192">Microsoft provides everything you need to build and scale Java apps on Azure at no cost to you.</span></span> <span data-ttu-id="cab84-193">透過 Zulu，您將免費取得 Java 應用程式的安全性更新和平台錯誤修正。</span><span class="sxs-lookup"><span data-stu-id="cab84-193">Through Zulu you'll receive free security updates and platform bug fixes for Java apps without any fees.</span></span>
   * <span data-ttu-id="cab84-194">Zulu Java 8、11 和 12 (預覽) 中提供 [Java Flight Recorder 和 Mission Control](java-jdk-flight-recorder-and-mission-control.md)。</span><span class="sxs-lookup"><span data-stu-id="cab84-194">[Java Flight Recorder and Mission Control](java-jdk-flight-recorder-and-mission-control.md) are available in Zulu Java 8, 11, and 12 (Preview).</span></span>

5. <span data-ttu-id="cab84-195">非 LTS 版本的技術預覽</span><span class="sxs-lookup"><span data-stu-id="cab84-195">Tech Preview of Non-LTS versions</span></span>

   * <span data-ttu-id="cab84-196">技術預覽讓您有機會在終將逐漸替換為 Java 17 LTS 的短期版本中以漸進方式測試新功能。</span><span class="sxs-lookup"><span data-stu-id="cab84-196">Tech previews provide you with opportunities to progressively test new features as they are delivered in short-term versions that will eventually graduate to Java 17 LTS.</span></span>

6. <span data-ttu-id="cab84-197">OpenJDK 的變更會傳送至上游</span><span class="sxs-lookup"><span data-stu-id="cab84-197">Changes to OpenJDK are up-streamed</span></span>

   * <span data-ttu-id="cab84-198">Azul Systems 認可者會推送 Zulu OpenJDK 的變更，而使上游存放庫的內容豐富而完整。</span><span class="sxs-lookup"><span data-stu-id="cab84-198">Azul Systems committers push Zulu changes to OpenJDK which makes the upstream repo comprehensive and inclusive.</span></span>

<span data-ttu-id="cab84-199">一如以往，開發人員可以在 Azure 上使用自備的 Java 執行階段 (包括 Oracle JDK 和 Red Hat JDK)，以及使用安全的基礎結構和功能豐富的服務。</span><span class="sxs-lookup"><span data-stu-id="cab84-199">As always, Java developers can bring their own Java run-times, including Oracle JDK and Red Hat JDK, to Azure and use  the secure infrastructure and feature-rich services.</span></span> <span data-ttu-id="cab84-200">Oracle Java SE 的實際執行版本仍可供 Java 開發人員在 Azure 上的 Windows 或 Linux 虛擬機器中執行 Java 工作負載。</span><span class="sxs-lookup"><span data-stu-id="cab84-200">The production edition of Oracle Java SE is also available to Java developers for running Java workloads in Windows or Linux virtual machines on Azure.</span></span>

## <a name="use-for-local-development"></a><span data-ttu-id="cab84-201">本機開發使用</span><span class="sxs-lookup"><span data-stu-id="cab84-201">Use for local development</span></span> 

<span data-ttu-id="cab84-202">開發人員可以[下載](https://www.azul.com/downloads/azure-only/zulu/)適用於 Azure 和 Azure Stack 的 Java JDK，以便在本機開發環境中使用。</span><span class="sxs-lookup"><span data-stu-id="cab84-202">Developers can [download](https://www.azul.com/downloads/azure-only/zulu/) Java JDKs for Azure and Azure Stack for use in local development environments.</span></span> <span data-ttu-id="cab84-203">下載適用於 Windows、Linux 和 macOS。</span><span class="sxs-lookup"><span data-stu-id="cab84-203">Downloads are available for Windows, Linux, and macOS.</span></span> <span data-ttu-id="cab84-204">使用 Linux 的開發人員也可以透過 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 套件管理員取得套件。</span><span class="sxs-lookup"><span data-stu-id="cab84-204">Developers working on Linux can also get packages through the [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) and [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) package managers.</span></span>

<span data-ttu-id="cab84-205">如需其他指引，請參閱[適用於 Azure 的 Docker 映像](java-jdk-docker-images.md)。</span><span class="sxs-lookup"><span data-stu-id="cab84-205">For additional guidance see [Docker images for Azure](java-jdk-docker-images.md).</span></span>