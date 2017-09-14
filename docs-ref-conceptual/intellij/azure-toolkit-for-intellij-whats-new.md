---
title: "適用於 IntelliJ 的 Azure 工具組新增功能"
description: "了解適用於 IntelliJ 的 Azure 工具組最新功能。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 46ed791f-df59-416a-809e-f52345ad973c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: 861ceb148976faff1f40055094c463171bdfb4b0
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>適用於 IntelliJ 的 Azure 工具組新增功能

## <a name="azure-toolkit-for-intellij-releases"></a>適用於 IntelliJ 的 Azure 工具組版本
本文包含適用於 IntelliJ 的 Azure 工具組的各種版本和最新更新相關資訊。

> [!NOTE]
> 另外還有適用於 Eclipse IDE 的 Azure 工具組。 如需詳細資訊，請參閱 [適用於 Eclipse 的 Azure 工具組]。
> 
> 

### <a name="april-14-2017"></a>2017 年 4 月 14 日
適用於 IntelliJ 的 Azure 工具組 - 2017 年 4 月版本包含下列增強功能：

* **改善的 Azure 登入體驗**：適用於 IntelliJ 的 Azure 工具組現在支援兩種可登入您 Azure 帳戶的方法︰互動式和自動化。 如需詳細資訊，請參閱[適用於 IntelliJ 的 Azure 工具組的 Azure 登入指示]。
* **使用 Docker 容器發佈**︰您現在可以使用適用於 IntelliJ 的 Azure 工具組，將 web 應用程式發佈作為 Docker 容器。 如需詳細資訊，請參閱[如何使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈作為 Docker 容器]。
* **儲存體帳戶管理**：適用於 IntelliJ 的 Azure 工具組現在支援從 Azure Explorer 檢視管理儲存體帳戶。 如需詳細資訊，請參閱[使用適用於 IntelliJ 的 Azure 工具組來管理儲存體帳戶]。
* **虛擬機器管理**：適用於 IntelliJ 的 Azure 工具組現在支援從 Azure Explorer 工具視窗管理虛擬機器。 如需詳細資訊，請參閱[使用適用於 IntelliJ 的 Azure 工具組來管理虛擬機器]。
* **移除遠端偵錯支援**。 已從適用於 IntelliJ 的 Azure 工具組將 Azure App Service 上的 Java web 應用程式遠端偵錯移除；在解決客戶使用此工具組所遭遇的一些問題時，必須進行此動作。

### <a name="august-26-2016"></a>2016 年 8 月 26 日
適用於 IntelliJ 的 Azure 工具組 - 2016 年 8 月版本包含下列增強功能：

* **自訂 JDK 散發套件**。 適用於 IntelliJ 的 Azure 工具組現在支援指定任意 JDK 版本，以及將其部署至 Azure WebApp 容器︰
  * 除了 Azure 提供的 JDK 之外，您也可以從 Azul Systems 在 Azure 上提供的多種 Zulu OpenJDK 版本中選擇。
  * 您也可以指定自己的 JDK 散發套件，前提是您必須以 ZIP 檔案的形式將它上傳到儲存體帳戶。
* **Azure Explorer 檢視的增強功能**：
  * 支援使用 Azure 的新 Resource Manager 模型管理虛擬機器︰您可以列出、建立和刪除以 Resource Manager 為基礎的虛擬機器，而不需要離開 IDE。
  * 支援使用 Azure 的 Resource Manager 管理儲存體帳戶 Blob，這項功能可輔助現有的「傳統」儲存體帳戶管理功能。
* **Microsoft JDBC Driver 6.0 for SQL Server**。 這項更新包含適用於 Microsoft SQL Server (6.0 版) 的最新 JDBC 驅動程式，該驅動程式現在以程式庫形式提供，可讓您輕鬆地加入專案中，因而取代舊有版本。

### <a name="june-29-2016"></a>2016 年 6 月 29 日
適用於 IntelliJ 的 Azure 工具組 - 2016 年 6 月版本包含下列增強功能：

* **Java 8 需求**。 適用於 IntelliJ 的 Azure 工具組現在需要 Java 8，雖然此需求僅適用於此工具組 - 您的應用程式可以繼續使用 Azure 所支援的所有 Java 版本。
* **支援最新的 Java JDK**。 適用於 IntelliJ 的 Azure 工具組現在支援最新版本的 Java JDK。
* **支援 Azure SDK v2.9.1**。 最新版的 Azure SDK 現在是適用於 IntelliJ 的 Azure 工具組的最低必要條件。
* **整合範例**。 適用於 IntelliJ 的 Azure 工具組目前精選了數個範例應用程式，可協助開發人員開始使用。
* **HDInsight 工具整合**。 Azure 的 HDInsight 工具現在隨附於適用於 IntelliJ 的 Azure 工具組。 如需詳細資訊，請參閱 [IntelliJ 的 HDInsight Tools 外掛程式]。
* **Java Web 應用程式的遠端偵錯**。 適用於 IntelliJ 的 Azure 工具組現在支援對 Azure App Service 上的 Java Web 應用程式進行遠端偵錯。

### <a name="april-12-2016"></a>2016 年 4 月 12 日
適用於 IntelliJ 的 Azure 工具組 - 2016 年 4 月版本包含下列增強功能：

* **支援 Azure SDK v2.9.0**。 最新版的 Azure SDK 現在是適用於 IntelliJ 的 Azure 工具組的最低必要條件。
* **與 Azure Web 應用程式支援有關的其他使用性、回應性和效能改進**。 工具組中多項與 Azure 通訊方式的效能最佳化會讓 UI 回應速度更快。
* **能夠在 IntelliJ 中刪除 Azure 現有的 Web 應用程式容器**。 適用於 IntelliJ 的 Azure 工具組現在可讓您刪除現有的 Azure Web 容器，不需要退出 IntelliJ。

## <a name="next-steps"></a>後續步驟

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[適用於 Eclipse 的 Azure 工具組]: ../eclipse/azure-toolkit-for-eclipse.md

[適用於 IntelliJ 的 Azure 工具組的 Azure 登入指示]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[如何使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈作為 Docker 容器]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[使用適用於 IntelliJ 的 Azure 工具組來管理儲存體帳戶]: ./azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer.md
[使用適用於 IntelliJ 的 Azure 工具組來管理虛擬機器]: ./azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer.md

[Azure Java Developer Center]: https://docs.microsoft.com/java/azure

[IntelliJ 的 HDInsight Tools 外掛程式]: /azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin
