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
# <a name="get-java-jdk-downloads-and-support-when-developing-for-azure"></a>開發 Azure 時取得 Java JDK 下載和支援

在 Azure 和 Azure Stack 上的 Java 開發人員可以使用 [OpenJDK 的 Azul Systems Zulu Enterprise 組建](https://www.azul.com/downloads/azure-only/zulu/)建置及執行生產環境使用的 Java 應用程式，而不會產生額外的支援成本。 您可以在 Azure 上使用任何想要的 Java 執行階段，但是若使用 Zulu 時，可取得免費的維護更新，且可使用[合格 Azure 支援方案](https://azure.microsoft.com/support/plans/)建立 Microsoft 支援問題。

## <a name="supported-java-versions-and-update-schedule"></a>支援的 Java 版本及更新排程

Azul Systems 將提供完整支援的[適用於 Microsoft Azure 的 OpenJDK Zulu Enterprise 組建](https://www.azul.com/downloads/azure-only/zulu/)，用於所有長期支援 (LTS) 版本的 Java，從 Java SE 7、8 和 11 開始。 [Azul 新聞稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)中可找到詳細資訊。


這些 JDK 版本會有每季的安全性更新和錯誤修正，並視需要提供頻外的重大更新和修補程式。  這項支援包含將安全性更新和錯誤修正移轉至 Java 7 和 8 報告於較新版本的 Java，如 Java 11，並確保較舊的 Java 版本維持穩定性和安全性。  Azure 客戶有權使用這些安全性更新與平台錯誤修正，而不會造成任何非計劃的 Java SE 訂用帳戶費用。 每個版本 Java SE 的支援日期會在下圖中醒目顯示。

![Azure 的 JDK 支援時間軸](media/azure-jdk-support.png)

Azul Systems 可維護這些版本的 [Java SE 藍圖](https://www.azul.com/products/azul_support_roadmap/)。

## <a name="use-for-local-development"></a>本機開發使用 

開發人員可以從 [Azul Systems 的網站](https://www.azul.com/downloads/azure-only/zulu/)下載適用於 Azure 和 Azure Stack 的 Java JDK。 下載適用於 Windows、Linux 和 macOS。 使用 Linux 的開發人員也可以透過 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 套件管理員取得套件。

若透過[合格的 Azure 支援方案](https://azure.microsoft.com/support/plans/)開發 Azure 或 Azure Stack，則會提供支援 Azure 的 Azul Zulu JDK 產品支援。

## <a name="use-in-docker-containers"></a>在 Docker 容器中使用

您可以使用 OpenJDK 的 Zulu Enterprise 組建，在選擇的任何散發版本上建置無限制的 Docker 映像。 以 Azure JDK 的 Azul Zulu Enterprise 為基礎的 Zulu Docker 映像可在 [Microsoft 公用 Docker 存放庫](https://hub.docker.com/r/microsoft/java-jdk/)中取得。 用來建置這些映像的 Dockerfile 位於 [Microsoft Java GitHub 存放庫](https://github.com/Microsoft/java/tree/master/docker)中。

若要使用這些映像容器化應用程式，您必須在 Dockerfile 中設定 `FROM` 陳述式，然後設定容器與應用程式的相依性。 例如，若要執行繫結至連接埠 8080 的 JAR 檔案封裝 Java SE 應用程式：

```Dockerfile
FROM  microsoft/java-jdk:<tag>
EXPOSE 8080
ADD target/hello.jar hello.jar
ENTRYPOINT ["java", "-jar","/hello.jar"]
```

## <a name="azure-service-runtime-support"></a>Azure 服務執行階段支援

Azure 平台服務，例如 [App Service](/azure/app-service/containers/)、[Azure Functions](/azure/azure-functions/functions-create-first-java-maven)、[Service Fabric](/azure/service-fabric/) 和 [HDInsight](/azure/hdinsight/) 使用 OpenJDK 的 Zulu Enterprise 組建，內建 Java 次要版本的自動修補功能與安全性修補程式和錯誤修正。