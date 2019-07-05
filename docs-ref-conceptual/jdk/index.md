---
title: Azure 開發的 Java JDK 和長期支援
description: 本文介紹 Azure 支援針對 Java 應用程式的開發和執行而提供的下載連結與聲明。
author: bmitchell287
manager: douge
ms.devlang: java
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: brendm
ms.openlocfilehash: 1bb93f775686b5b0f127c586dda802f4eb5f775d
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533632"
---
# <a name="java-long-term-support-for-azure-and-azure-stack"></a>Java 對於 Azure 和 Azure Stack 的長期支援

身為 Microsoft Azure 和 Azure Stack 上的 Java 開發人員，您可以使用 [Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) 建置及執行生產 Java 應用程式，而不會產生額外的支援成本。 您可以在 Azure 上使用任何想要的 Java 執行階段，但在使用 Zulu 時，您可以取得免費的維護更新，並且可透過 Microsoft 解決支援問題。

> [!div class="nextstepaction"]
> [下載並安裝 Java](java-jdk-install.md)

## <a name="long-term-support"></a>長期支援

* [Java 11](https://www.azul.com/downloads/azure-only/zulu/#java11)
* [Java 8](https://www.azul.com/downloads/azure-only/zulu/#java8)
* [Java 7](https://www.azul.com/downloads/azure-only/zulu/#java7)

## <a name="technical-preview"></a>技術預覽

* [Java 12](https://www.azul.com/downloads/azure-only/zulu/#java12)

## <a name="what-is-the-zulu-openjdk-for-azure"></a>什麼是適用於 Azure 的 Zulu OpenJDK？

OpenJDK 的 Azul Zulu Enterprise 組建是免費、多平台、可實際執行的 OpenJDK 散發套件，適用於 Azure 和 Azure Stack，由 Microsoft 與 Azul Systems 提供支援。 這些散發套件：

* 是封裝為 Java Development Kit (JDK)、Java Runtime Environment (JRE) 和無周邊 JRE 的純開放原始碼 OpenJDK 組建。 這些二進位檔是完全相容且符合規範的商業 Java Standard Edition (SE) 組建，可與 Azure 和 Azure Stack 上的 Java 應用程式或元件搭配使用。
* 具備長期支援 (LTS)，包括錯誤修正、效能增強和安全性修補程式。
* 可用來在 Windows、Linux 和 macOS 上開發和執行 Java 應用程式。
* 可作為 Docker Hub 上的容器映像，以及 Azure Marketplace 中的虛擬機器 (Windows 和 Linux)。
* 供 Azure 用來支援許多 Azure 服務，例如：
  * Azure App Service (Windows)
  * Azure App Service (Linux)
  * Azure Functions
  * Azure Service Fabric
  * Azure HDInsight
  * Azure 搜尋服務
  * Azure DevOps
  * Azure Cloud Shell  

## <a name="supported-java-versions-and-update-schedule"></a>支援的 Java 版本及更新排程

Azul Systems 提供完整支援的[適用於 Azure 的 OpenJDK Zulu Enterprise 組建](https://www.azul.com/downloads/azure-only/zulu/)，用於所有 LTS 版本的 Java，從 Java SE 7、8 和 11 開始。 如需詳細資訊，請參閱 [Azul 新聞稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)。

|Java SE LTS  |支援期限  |
|---------|----------|
|[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7) |2023 年 7 月 |
|[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8) |2025 年 3 月|
|[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11) |2026 年 9 月|
|[![Java 12](../media/jdk/java-12.png)]() |**預覽**|

這些 JDK 版本會有每季的安全性更新和錯誤修正，並視需要提供頻外的重大更新和修補程式。 這項支援包括可回移在較新版本的 Java (如 Java 11) 中報告的 Java 7 和 8 安全性更新和錯誤修正。 此支援可確保較舊的 Java 版本能夠維持穩定性和安全性。 Azure 客戶可取得這些安全性更新與平台錯誤修正，而不會造成任何非計劃的 Java SE 訂用帳戶費用。

Azul Systems 可維護這些版本的 [Java SE 藍圖](https://www.azul.com/products/azul_support_roadmap/)。

## <a name="benefits-for-developers"></a>對開發人員的好處

Azul Zulu JDK 版本：

* 同時受到 Microsoft 和 Azul Systems 的支援。

   * Zulu 二進位檔可供實際執行之用，並且受到 Microsoft 和 Azul Systems 的支援。
   * Zulu 隨附 Java 7、8 和 11 的零成本 LTS (針對 Java 17 也將提供 LTS)。 您可以在需要時才升級 Java 版本。
   * Java 7 的支援期限為 2023 年 7 月。 Java 8 和 11 在 2024 年之後仍受支援。
   * Microsoft 致力於在支援多項 Azure 服務的內部機器上執行 Zulu。

* 可供實際執行之用。

   * 其 OpenJDK 組建採用 100% 開放原始碼。
   * 許多 Java SE 散發套件皆為簡易替換。
   * JDK、JRE 和無周邊 JRE。
   * Java 7、8 和 11。
   * 經驗證與使用 OpenJDK Community Technology Compatibility Kit (TCK) 的 Java SE 規格相容。
   * 身為開發人員，您將持續取得 Java SE 的生產更新，包括 Java SE 7、8 和 11 的錯誤修正、效能增強和安全性修補程式。

* 支援多平台 Zulu 支援多平台和版本的二進位檔，包括：

   * Windows 用戶端
     * 10
     * 8.1
     * 8、7
   * Windows Server
     * 2016 R2
     * 2016
     * 2012 R2
     * 2012
     * 2008 R2
   * Linux，包括
     * RHEL
     * CentOS
     * Ubuntu
     * SLES
     * Debian
     * Oracle Linux
   * macOS X
   * 以多種套件類型提供：
     * MSI、ZIP、TAR、DEB、RPM 和 DMG

    多基底 OS 映像上已針對 Zulu JDK、JRE 和無周邊 JRE 通過認證的 Docker 容器映像，可從 Docker 取得。

    Hub：

    * [JDK](https://hub.docker.com/_/microsoft-java-jdk)
    * [JRE](https://hub.docker.com/_/microsoft-java-jre)
    * [無周邊 JRE](https://hub.docker.com/_/microsoft-java-jre-headless)

* 無成本。

   * Microsoft 免費提供您在 Azure 上建置及調整 Java 應用程式所需的一切。 透過 Zulu，您將免費取得 Java 應用程式的安全性更新和平台錯誤修正。
   * Zulu Java 8、11 和 12 (預覽) 中提供 [Java Flight Recorder 和 Mission Control](java-jdk-flight-recorder-and-mission-control.md)。

* 可作為非 LTS 版本的技術預覽。

   * 透過技術預覽，您將能夠在終將逐漸替換為 Java 17 LTS 的短期版本中以漸進方式測試新功能。

* OpenJDK 的變更會傳送至上游。

   * Azul Systems 認可者會推送 Zulu OpenJDK 的變更，而使上游存放庫的內容豐富而完整。

一如既往，身為 Java 開發人員，您可以在 Azure 中導入自己的 Java 執行階段，包括 Oracle JDK 和 Red Hat JDK。 您也可以使用安全的基礎結構和功能豐富的服務。 Oracle Java SE 的實際執行版本可供您在 Azure 上的 Windows 或 Linux 虛擬機器中執行 Java 工作負載。

## <a name="use-java-jdks-for-local-development"></a>使用 Java JDK 進行本機開發 

您可以[下載適用於 Azure 和 Azure Stack 的 Java JDK](https://www.azul.com/downloads/azure-only/zulu/)，以便在本機開發環境中使用。 下載適用於 Windows、Linux 和 macOS。 如果您使用 Linux，您也可以透過 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 套件管理員取得套件。

如需其他指引，請參閱[適用於 Azure 的 Docker 映像](java-jdk-docker-images.md)。
