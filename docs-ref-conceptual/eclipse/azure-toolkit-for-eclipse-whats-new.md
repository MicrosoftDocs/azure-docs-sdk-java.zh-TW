---
title: "適用於 Eclipse 的 Azure 工具組的新功能"
description: "了解適用於 Eclipse 的 Azure 工具組中的最新功能。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 16b066ea-aae7-4c30-9a12-fa0c3711b93e
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: ed21f4308b2f6e45f022608a2ba32fe7db6922d3
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-eclipse"></a>適用於 Eclipse 的 Azure 工具組的新功能

## <a name="azure-toolkit-for-eclipse-releases"></a>適用於 Eclipse 的 Azure 工具組版本
本文包含適用於 Eclipse 的 Azure 工具組的各種版本和最新更新的相關資訊。

> [!NOTE]
> 請參閱下列網頁，以取得最新的資訊：
> 
> <https://github.com/Microsoft/azure-tools-for-java/releases>

> [!NOTE]
> 另外還有適用於 IntelliJ IDE 的 Azure 工具組。 如需詳細資訊，請參閱 [Azure Toolkit for IntelliJ]。
> 
> 

### <a name="april-14-2017"></a>2017 年 4 月 14 日
適用於 Eclipse 的 Azure 工具組 - 2017 年 4 月版本包含下列增強功能：

* **改善的 Azure 登入體驗**：適用於 Eclipse 的 Azure 工具組現在支援兩種可登入您 Azure 帳戶的方法︰互動式和自動化。 如需詳細資訊，請參閱[適用於 Eclipse 的 Azure 工具組的 Azure 登入指示]。
* **使用 Docker 容器發佈**︰您現在可以使用適用於 Eclipse 的 Azure 工具組，將 web 應用程式發佈作為 Docker 容器。 如需詳細資訊，請參閱[如何使用適用於 Eclipse 的 Azure 工具組，將 Web 應用程式發佈作為 Docker 容器]。
* **儲存體帳戶管理**：適用於 Eclipse 的 Azure 工具組現在支援從 Azure Explorer 檢視管理儲存體帳戶。 如需詳細資訊，請參閱[使用適用於 Eclipse 的 Azure 工具組來管理儲存體帳戶]。
* **虛擬機器管理**：適用於 Eclipse 的 Azure 工具組現在支援從 Azure Explorer 檢視管理虛擬機器。 如需詳細資訊，請參閱[使用適用於 Eclipse 的 Azure 工具組來管理虛擬機器]。
* **移除遠端偵錯支援**。 已從適用於 Eclipse 的 Azure 工具組將 Azure App Service 上的 Java web 應用程式遠端偵錯移除；在解決客戶使用此工具組所遭遇的一些問題時，必須進行此動作。

### <a name="august-26-2016"></a>2016 年 8 月 26 日
適用於 Eclipse 的 Azure 工具組 - 2016 年 8 月版本包含下列增強功能：

* **自訂 JDK 散發套件**。 適用於 Eclipse 的 Azure 工具組現在支援指定任意 JDK 版本，以及將其部署至 Azure WebApp 容器︰
  * 除了 Azure 提供的 JDK 之外，您也可以從 Azul Systems 在 Azure 上提供的多種 Zulu OpenJDK 版本中選擇。
  * 您也可以指定自己的 JDK 散發套件，前提是您必須以 ZIP 檔案的形式將它上傳到儲存體帳戶。
* **Azure Explorer 檢視的增強功能**：
  * 支援使用 Azure 的新 Resource Manager 模型管理虛擬機器︰您可以列出、建立和刪除以 Resource Manager 為基礎的虛擬機器，而不需要離開 IDE。
  * 支援使用 Azure 的 Resource Manager 管理儲存體帳戶 Blob，這項功能可輔助現有的「傳統」儲存體帳戶管理功能。
* **Microsoft JDBC Driver 6.0 for SQL Server**。 這項更新包含適用於 Microsoft SQL Server (6.0 版) 的最新 JDBC 驅動程式，該驅動程式現在以程式庫形式提供，可讓您輕鬆地加入專案中，因而取代舊有版本。

### <a name="june-29-2016"></a>2016 年 6 月 29 日
適用於 Eclipse 的 Azure 工具組 - 2016 年 6 月版本包含下列增強功能：

* **Java 8 需求**。 適用於 Eclipse 的 Azure 工具組現在需要 Java 8，雖然此需求僅適用於此工具組 - 您的應用程式可以繼續使用 Azure 所支援的所有 Java 版本。
* **支援最新的 Java JDK**。 適用於 Eclipse 的 Azure 工具組現在支援最新版本的 Java JDK。
* **支援 Azure SDK v2.9.1**。 最新版的 Azure SDK 現在是 Azure Toolkit for Eclipse 的最低必要條件。
* **整合範例**。 適用於 Eclipse 的 Azure 工具組目前精選了數個範例應用程式，可協助開發人員開始使用。
* **HDInsight 工具整合**。 Azure 的 HDInsight 工具現在隨附於適用於 Eclipse 的 Azure 工具組。 如需詳細資訊，請參閱 [Eclipse 的 HDInsight Tools 外掛程式]。
* **Java Web 應用程式的遠端偵錯**。 適用於 Eclipse 的 Azure 工具組現在支援對 Azure App Service 上的 Java Web 應用程式進行遠端偵錯。
* **支援 Eclipse Luna 版本。** 新的基本必要 Eclipse IDE 版本為 Luna。

### <a name="april-12-2016"></a>2016 年 4 月 12 日
Azure Toolkit for Eclipse - 2016 年 4 月版本包含下列增強功能：

* **支援 Azure SDK v2.9.0**。 最新版的 Azure SDK 現在是 Azure Toolkit for Eclipse 的最低必要條件。
* **與 Azure Web 應用程式支援有關的其他使用性、回應性和效能改進**。 工具組中多項與 Azure 通訊方式的效能最佳化會讓 UI 回應速度更快。
* **能夠在 Eclipse 內刪除 Azure 中現有的 Web 應用程式容器**。 Azure Toolkit for Eclipse 現在可讓您刪除現有的 Azure Web 容器，不需要退出 Eclipse。

### <a name="march-7-2016"></a>2016 年 3 月 7 日
Azure Toolkit for Eclipse - 2016 年 3 月版本包含下列增強功能：

* **支援輕量型 Java 應用程式的快速部署**。 Azure Toolkit for Eclipse 現在支援輕量型 Java 應用程式在 Azure Web 應用程式容器中的快速部署，所以現在部署 Java 應用程式是以秒計而不以分鐘計。
* **支援使用 Azure 總管檢視管理 Web 應用程式**。 工具組中的 Azure 總管檢視現在支援列出、啟動及停止 Azure Web 應用程式。
* **更新 Tomcat、Jetty 和 Zulu OpenJDK 發佈**。 Azure Toolkit for Eclipse 支援在 Azure 雲端服務部署 Java 的 Tomcat、Jetty 和 Zulu OpenJDK 更新版本。

### <a name="january-4-2016"></a>2016 年 1 月 4 日
適用於 Eclipse 的 Azure 工具組 - 2016 年 1 月版本包含下列增強功能：

* **支援 Zulu OpenJDK 更新**。 如需詳細資訊，請參閱 [Zulu OpenJDK 的 Azul Systems 網頁]。
* **已更新 Tomcat 和 Jetty 散發套件**。 已更新 Microsoft Azure 上所提供、可搭配適用於 Eclipse 的 Azure 工具組使用的 Jetty 和 Tomcat 散發套件
* **適用於 Azure 的 Eclipse 和 IntelliJ 工具組之間的功能同位**。 適用於 Eclipse 的 Azure 工具組和 [Azure Toolkit for IntelliJ] 現在支援相同的功能集合。

### <a name="september-1-2015"></a>2015 年 9 月 1 日
適用於 Eclipse 的 Azure 工具組 - 2015 年 9 月版本包含下列增強功能：

* **支援 Zulu OpenJDK 更新**。 如需詳細資訊，請參閱 [Zulu OpenJDK 的 Azul Systems 網頁]。
* **已更新 Tomcat 和 Jetty 散發套件**。 已更新 Microsoft Azure 上所提供、可搭配適用於 Eclipse 的 Azure 工具組使用的 Jetty 和 Tomcat 散發套件 (這些散發套件可讓開發人員使用適用於 Eclipse 的 Azure 工具組建立快速開發與測試專案)。
* **支援自動更新的 Tomcat 和 Jetty 參考**。 除了 Azure 上所提供之 Tomcat 和 Jetty 的特定版本外，開發人員現在還可以參考稱為「最新 (自動更新)」的散發套件，這個散發套件會在下一次回收角色執行個體時，自動更新為每個 Jetty 或 Tomcat 主要版本的最新散發套件  (回收程序會自動發生，但開發人員可以透過 Azure 入口網站手動觸發回收程序)。這項新功能表示開發人員不需要重新部署應用程式就能夠更新其伺服器軟體  (
* 這項功能目前僅供開發和測試目的或非任務關鍵應用程式使用，並不建議用於生產環境)。
* **適用於 Azure 儲存體中 blob、佇列和資料表的 Azure 總管檢視**。 這可讓開發人員直接從 Eclipse IDE 中對其儲存體構件執行一組常見的工作。 例如：刪除、上傳或下載 blob。

### <a name="august-1-2015"></a>2015 年 8 月 1 日
適用於 Eclipse 的 Azure 工具組 - 2015 年 8 月版本包含下列增強功能：

* **Application Insights 檢測金鑰管理**。 這項更新可讓您直接從 Eclipse IDE 取得、建立及管理 Application Insights 檢測金鑰。
* **Microsoft JDBC Driver 4.1 for SQL Server**。 這項更新包含適用於 Microsoft SQL Server 之最新 JDBC 驅動程式的支援。
* **Azure SDK 2.7 版**。 這個最新的 Azure SDK 更新是工具組安裝在 Windows 上時的新必要條件  (請注意，在非 Windows 作業系統上則不需要此更新)。
* **支援 Zulu OpenJDK v7 更新**。 如需詳細資訊，請參閱 [Zulu OpenJDK 的 Azul Systems 網頁]。

### <a name="may-1-2015"></a>2015 年 5 月 1 日
適用於 Eclipse 的 Azure 工具組 - 2015 年 5 月版本包含下列增強功能：

* **改善伺服器選擇 UI**。 此版本簡化了在非 Windows 作業系統上使用工具組的方式。
* **支援 Maven 專案**。 此版本支援將 Maven 專案做為應用程式，工具組可部署到 Azure，並設定 Application Insights。
* **Azure SDK 2.6 版**。 這個最新的 Azure SDK 更新是工具組安裝在 Windows 上時的新必要條件  (請注意，在非 Windows 作業系統上則不需要此更新)。
* **升級部署而非重新發佈部署**。 如果要在先前的版本已經上線時重新發佈部署專案，此工具組現在會使用 Azure 的部署升級功能，而不會像過去一樣先關閉先前的部署再從頭開始重新發佈。 這可讓您的雲端服務盡可能保持運作而不中斷，協助您即使是在更新期間也能達到高可用性，並加快重新發佈程序的速度。
* **支援最新的 Zulu OpenJDK v8 - 更新 40**。 如需詳細資訊，請參閱 [Zulu OpenJDK 的 Azul Systems 網頁]。

### <a name="march-9-2015"></a>2015 年 3 月 9 日
適用於 Eclipse 的 Azure 工具組 - 2015 年 3 月版本包含下列增強功能：

* **支援 Mac、Ubuntu 和其他 Linux 類別**。 這一版的適用於 Eclipse 的 Azure 工具組加入了 Mac OS 和數個 Unix 平台的支援，讓開發人員能夠藉由安裝此工具組，從執行非 Windows 作業系統的 Eclipse 建立和設定 Java 專案並將其發佈至 Azure 雲端服務 (PaaS)。

> [!NOTE]
> 這項功能目前為預覽狀態，因此不建議用於生產環境。 雖然此功能沒有客戶支援服務等級協定 (SLA)，但仍歡迎您提供相關意見反應。
> 
> 

* **新的 Application Insights 外掛程式**。 開發人員現在可以在 Azure 上使用 Application Insights 以設定自動伺服器遙測。
* **Ant 型命令列部署自動化**。 這項功能可讓開發人員在 Eclipse 外部使用 Ant 自動發佈較新版的部署。 第一次從 Eclipse 部署專案後，系統就會為專案自動設定預先產生的指令碼，後續的部署就可以使用指令碼完全自動進行部署 (只能透過命令列)。
* **在 Azure 上提供 Tomcat 和 Jetty 以讓部署程序變得簡單、快速**。 開發人員現在可以直接參考 Azure 上提供的各種 Tomcat 或 Jetty 版本，而不必將 Java 伺服器上傳到其帳戶 (或透過工具組)，因此不需要針對快速入門案例上傳 Java 伺服器。
* **用於將 Java Web 應用程式發佈至 Azure 雲端服務的快捷方法**。 為了減少簡單開發和測試案例的學習曲線，開發人員現在可以更直接地將 Java 應用程式發佈至 Azure。 應用程式不需要經歷建立及設定 Azure 部署專案的程序，而是會使用 Tomcat v8 和 Zulu JVM (OpenJDK) 的預設執行個體進行部署。

### <a name="january-30-2015"></a>2015 年 1 月 30 日
適用於 Eclipse 的 Azure 工具組 - 2015 年 1 月版本包含下列增強功能：

* **支援 IBM® WebSphere® Application Server Liberty Core**。 此版本將 IBM WebSphere Application Server Liberty Core 增加到支援的應用程式伺服器清單中，透過這些伺服器，工具組將能夠部署到 Azure。 這個最新的新增項目會擴充已包含各種版本之 Tomcat、Jetty、JBoss 和 GlassFish 的工具組目前&quot;現成&quot;支援的應用程式伺服器清單。
* **加入 Application Insights SDK**。 這個新發行的用戶端 API 程式庫 (v0.9.0) 現在是 Azure Libraries for Java 封裝的一部分。
* **已更新 Azure Libraries for Java 的封裝**。 此更新包括 Azure Libraries for Java v0.7.0 和 Storage Client API v2.0.0 以及新發佈的 Application Insights SDK v0.9.0。

### <a name="november-12-2014"></a>2014 年 11 月 12 日
適用於 Eclipse 的 Azure 工具組 - 2014 年 11 月版本包含下列增強功能：

* **支援 Azure SDK 2.5**。 這個最新的 Azure SDK 更新是工具組的新必要條件。
* **支援 Zulu OpenJDK v1.8、v1.7 和 v1.6 封裝的更新版本**。 如需詳細資訊，請參閱 [Zulu OpenJDK 的 Azul Systems 網頁]。
* **支援雲端服務新的標準 D 大小**，以提供更高的效能和額外的記憶體資源。 如需詳細資訊，請參閱＜ [Azure 的虛擬機器和雲端服務大小]＞。

### <a name="october-17-2014"></a>2014 年 10 月 17 日
適用於 Eclipse 的 Azure 工具組 - 2014 年 10 月版本包含下列增強功能：

* **改進發佈到雲端案例的效能**。 若使用者有多個訂用帳戶和儲存體帳戶，載入訂用帳戶資訊的速度變得更快了。
* **支援 Zulu OpenJDK v1.8 封裝的更新版本**。 如需詳細資訊，請參閱 [Zulu OpenJDK 的 Azul Systems 網頁]。
* **支援淘汰舊版的第三方 JDK**。 對於新的部署專案，已淘汰的 JDK 封裝將不會再顯示於下拉式功能表中。 參考已淘汰之 JDK 封裝的現有專案目前還是能夠繼續參考，但建議您升級這類專案使其依賴最新的 JDK 封裝。
* **已更新 Azure Libraries for Java 用戶端 API 程式庫的封裝版本**。 如需詳細資訊，請參閱 [Microsoft Azure 用戶端 API]。
* **錯誤修正。** 此版本包含許多依據使用者報告和測試所做出的其他錯誤修正。

### <a name="august-5-2014"></a>2014 年 8 月 5 日
適用於 Eclipse 的 Azure 工具組 - 2014 年 8 月版本包含下列增強功能：

* **支援 Azure SDK 2.4。** 舊版 Eclipse 工具組無法使用這個新發佈的 SDK。
* **已更新 Zulu OpenJDK v1.6、v1.7 和 v1.8 封裝的版本。** 如需詳細資訊，請參閱 [Zulu OpenJDK 的 Azul Systems 網頁]。
* **已更新 Azure Libraries for Java 用戶端 API 程式庫的封裝版本。** 如需詳細資訊，請參閱 [Microsoft Azure 用戶端 API]。
* **支援最新的發佈設定檔案格式。** 已加入 2.0 版發佈設定檔案格式的支援。
* **發佈到雲端功能背後的架構變更。** 此工具組現在會使用新發佈之適用於 Java 的 Microsoft Azure 用戶端 API 來支援其發佈到雲端功能。
* **錯誤修正。** 此版本包含許多使用者要求的錯誤修正。

### <a name="june-12-2014"></a>2014 年 6 月 12 日
適用於 Eclipse 的 Azure 工具組 - 2014 年 6 月版本是次要服務更新，可提供下列增強功能：

* **支援 Zulu OpenJDK 封裝 v1.8。** 如需詳細資訊，請參閱 [Zulu OpenJDK 的 Azul Systems 網頁]。
* **已更新 Zulu OpenJDK v1.6 和 1.7 封裝的版本。** 如需詳細資訊，請參閱 [Zulu OpenJDK 的 Azul Systems 網頁]。
* **已更新 Azure Libraries for Java 用戶端 API 程式庫的封裝版本。** 如需詳細資訊，請參閱 [Microsoft Azure 用戶端 API]。
* **錯誤修正。** 此版本包含許多使用者要求的錯誤修正。

### <a name="april-4-2014"></a>2014 年 4 月 4 日
適用於 Eclipse 的 Azure 外掛程式 - 2014 年 4 月版本已發行。 這個更新需搭配屬於必要條件的 Azure SDK 2.3 版，而且在您安裝外掛程式時會自動下載此 SDK。 此更新包含 2014 年 2 月預覽以來所推出的新功能、錯誤修正，以及一些根據意見反應所開發出的使用性增強功能：

* **支援 Azure SDK 2.3 版本。** 適用於 Eclipse 的 Azure 外掛程式 - 2014 年 4 月版本需要 Azure SDK 2.3。 在使用新的外掛程式時，如果您還沒有 Azure SDK 2.3，系統會提示您允許安裝此 SDK。 請勿將 Azure SDK 2.3 用在舊版的外掛程式。
* **不需要完整部署封裝即可升級應用程式。** 在部署屬於專案一部分的 Java 應用程式時，外掛程式現在會自動將這些應用程式上傳到您選定的儲存體帳戶，以便您可以更新專案，並回收角色執行個體以部署最新的應用程式版本，而不需要重建並重新部署整個封裝。
* **Tomcat 8 現在是可辨識的應用程式伺服器。** 如果您在 [Azure 部署專案] 對話方塊的 [伺服器] 索引標籤中選取您電腦上的 Tomcat 8 安裝目錄，外掛程式現在會自動偵測到它，並能夠以自動化方式部署 Tomcat 8，其情形類似清單中已存在的舊版 Tomcat。
* **Azul Zulu OpenJDK 封裝更新：v1.7 更新 51 和 v1.6 更新 47。** 自這一版開始，有提供 Azul System 的 Zulu Open JDK v7 封裝更新 51。 此外，也開始提供 Zulu Open JDK v6 封裝，從更新 47 開始。 除了這些更新之外，先前已提供 Zulu Open JDK v7 封裝更新 45、更新 40 和更新 25。
* **支援 A8 和 A9 Microsoft Azure 虛擬機器大小。** 您現在可以將雲端服務部署到高記憶體的 A8 和 A9 虛擬機器大小。 如需這些 VM 大小的詳細資訊，請參閱 [Azure 的虛擬機器和雲端服務大小]。
* **針對啟用 SSL 的角色，自動從 HTTP 重新導向至 HTTPS。** 當您的雲端服務只包含 HTTPS 角色時，如果使用者的要求指定 HTTP，它會自動重新導向至 HTTPS。 您不需要建立個別角色來處理 HTTP 要求。
* **用於本機模擬的快速模擬器。** 在本機上偵錯應用程式時，現在會使用 Azure 快速模擬器做為模擬器。
* **Azure 已更名為 Microsoft Azure。** UI 畫面現在會反映 Azure 已更名的事實而不再稱為 Azure。

### <a name="february-6-2014"></a>2014 年 2 月 6日
適用於 Eclipse 的 Azure 外掛程式 - 2014 年 2 月預覽已發行。 此更新包含 2013 年 10 月預覽以來所推出的新功能、錯誤修正，以及一些根據意見反應所開發出的使用性增強功能：

* **支援 SSL 卸載。** 已將安全通訊端層 (SSL) 卸載新增為一項功能，讓您可以在 Azure 上的 Java 部署中輕鬆啟用超文字安全傳輸通訊協定 (HTTPS) 支援，而不必在 Java 應用程式伺服器中設定 SSL。 在工作階段親和性和/或驗證通訊案例中，這個功能特別有用。 例如，在使用此工具組已經支援的存取控制服務 (ACS) 篩選器時。 如需詳細資訊，請參閱 [SSL 卸載]和[如何使用 SSL 卸載]。
* **GlassFish 4 現在是可辨識的應用程式伺服器。** 如果您在 [Azure 部署專案] 對話方塊的 [伺服器] 索引標籤中選取您電腦上的 GlassFish 4 安裝目錄，外掛程式現在會自動偵測到它，並能夠以自動化方式部署 GlassFish OSE 4，其情形類似清單中已存在的 GlassFish OSE 3 版本。
* **Azul Zulu OpenJDK 封裝更新 45。** 自這一版開始，現在會提供 Azul System 的 Zulu (Open JDK v7 封裝) 更新 45，除此之外，先前已提供更新 40 和更新 25。
* **支援私人端點連接埠的「自動」功能。** 您可以將輸入端點和內部端點的私人連接埠設定為自動，以讓 Azure 自動為該端點指派連接埠。 先前您只能指派特定連接埠號碼。
* **支援在建立自我簽署憑證的 UI 中自訂憑證名稱 (CN)。** 先前會對所有新憑證使用相同的硬式編碼名稱，但現在您可以指定自有憑證名稱，以協助區分 Azure 入口網站中具有不同用途的多個憑證。
* **Azure 工具列：** Azure 工具列已做了下列變更： 
  * ![][ic710876] 已新增 [新增 Azure 部署專案] 這個圖示。
  * ![][ic710877] 已加入這個圖示來做為建立自我簽署憑證對話方塊的捷徑。
* **支援 A5 Azure 虛擬機器大小。** 您現在可以將雲端服務部署到高記憶體的 A5 虛擬機器大小。 如需此 VM 大小的詳細資訊，請參閱 [Azure 的虛擬機器和雲端服務大小]。
* **支援 Microsoft Windows Server 2012 R2。** 您現在可以選取 Windows Server 2012 R2 做為雲端作業系統。

### <a name="october-22-2013"></a>2013 年 10 月 22日
適用於 Eclipse 的 Azure 外掛程式 - 2013 年 10 月預覽已發行。 此更新包含 2013 年 9 月預覽以來所推出的新功能、錯誤修正，以及一些根據意見反應所開發出的使用性增強功能：

* **支援 Azure SDK 2.2 版本。** 適用於 Eclipse 的 Azure 外掛程式 - 2013 年 10 月預覽支援 Azure SDK 2.2。 此外掛程式仍然可以使用 Azure SDK 2.1，而且如果您還沒有至少安裝 Azure SDK 2.1，則會自動安裝 Azure SDK 2.2。
* **Azul Zulu OpenJDK 封裝更新 40。** 正如 2013 年 9 月預覽所宣佈的，此外掛程式現在能直接在 Azure 上使用第三方提供的 JDK，而不需要您上傳自己的 JDK。 在 2013 年 10 月版本中，現在會提供 Azul System 的 Zulu (Open JDK v7 封裝) 更新 40，除此之外，原先已發佈了更新 25。
* **活動記錄檔中的雲端部署連結。** 在 Azure 活動記錄檔中，當部署的狀態為 [已發佈] 時，您可以按一下 [已發佈]，因為它現在是部署的連結，然後部署就會在瀏覽器中開啟  ([已發佈] 狀態先前標示為 [執行中])。
* **發佈時可以選擇目標 OS。** [發佈至 Azure] 對話方塊包含新的 [目標 OS] 欄位，可讓您更容易找到要設定的目標作業系統。
* **自動覆寫先前的部署。** [發佈至 Azure] 對話方塊包含新的 [覆寫先前的部署] 核取方塊。 如果選取此選項，就會在發佈新的部署時自動覆寫先前的部署。當您沒有先取消發佈先前的部署就將新部署發佈至相同位置時，您不會再遇到 &quot;409 衝突&quot;問題。
* **Jetty 9 現在是可辨識的應用程式伺服器。** 如果您在 [Azure 部署專案] 對話方塊的 [伺服器] 索引標籤中選取您電腦上的 Jetty 9 安裝目錄，外掛程式現在會自動偵測到它，並能夠以自動化方式部署 Jetty 9，其情形類似清單中已存在的舊版 Jetty。
* **從 [專案] 內容功能表新增角色。** **Azure** 專案內容功能表現在包含新的 [新增角色] 功能表項目，可讓您更快且更容易地找到要新增到 Azure 專案的新角色。
* **Azure Libraries for Java 程式庫封裝更新。** 此更新是以 [Microsoft Azure 用戶端 API]。

### <a name="september-25-2013"></a>2013 年 9 月 25 日
適用於 Eclipse 的 Azure 外掛程式 - 2013 年 9 月預覽已發行。 此更新包含 2013 年 8 月預覽以來所推出的新功能、錯誤修正，以及一些根據意見反應所開發出的使用性增強功能：

* **能夠部署 Azure 上提供的 Azul Zulu OpenJDK 封裝。** 在指定要用於 Azure 部署的 JDK 時已加入新的選項。 使用此選項，您可以直接在 Azure 雲端上部署第三方 JDK 封裝，而不必上傳您自己的 JDK。 Azul Systems 提供了第一個這類封裝，其名稱為 Zulu，是以 OpenJDK 為基礎，而您現在可以使用此選項部署這個封裝。
* **Azure Libraries for Java 程式庫封裝更新。** 此更新是以 [Microsoft Azure 用戶端 API]。

### <a name="august-1-2013"></a>2013 年 8 月 1日
適用於 Eclipse 的 Azure 外掛程式 - 2013 年 8 月預覽已發行。 這個更新需搭配屬於必要條件的 Azure SDK 2.1 版，而且在您安裝外掛程式時會自動下載此 SDK。 此更新包含 2013 年 7 月預覽以來所推出的新功能、錯誤修正，以及一些根據意見反應所開發出的使用性增強功能：

* **移除將本機 JDK 和本機應用程式伺服器納入成為部署封裝一部分的選項。** 若要將這些元件內嵌在封裝中，最好是在部署期間從雲端儲存體下載 JDK 和應用程式伺服器，因為下載這些項目會導致較小的部署封裝大小、更快速的部署時間以及更容易進行維護。 因此，我們移除了將 JDK 和應用程式伺服器納入部署封裝的選項。 已設定為將本機 JDK 和本機應用程式伺服器納入成為部署封裝一部分的現有專案，會自動轉換為將 JDK 和應用程式伺服器自動上傳到雲端儲存體。
* **支援 Azure SDK 2.1 版本。** 適用於 Eclipse 的 Azure 外掛程式 - 2013 年 8 月預覽需要 Azure SDK 2.1。 請勿將 2013 年 8 月預覽用於舊版 Azure SDK，此外，也請勿將 Azure SDK 2.1 用於舊版適用於 Eclipse 的 Azure 外掛程式。
* **支援 Eclipse Kepler 版本。** 與此相關的是，新的最小必要 Eclipse IDE 版本為 Indigo。 適用於 Eclipse 的 Azure 外掛程式不會再於 Helios 上進行正式測試。

### <a name="july-3-2013"></a>2013 年 7 月 3 日
適用於 Eclipse 的 Azure 外掛程式 - 2013 年 7 月預覽已發行。 此更新包含 2013 年 5 月預覽以來所推出的新功能、錯誤修正，以及一些根據意見反應所開發出的使用性增強功能：

* **能夠建立新的儲存體帳戶。** [新增儲存體帳戶] 對話方塊中已新增 [新增] 按鈕。 這可讓您在 Eclipse 外掛程式內建立儲存體帳戶，而不需要您登入 Azure 管理入口網站  (您必須已經有 Azure 訂用帳戶才能使用此功能)。如需建立新儲存體帳戶的詳細資訊，請參閱[建立新的儲存體帳戶]。
* **儲存體帳戶有新的 &quot;(自動)&quot; 選項可供用來自動部署 JDK 和伺服器，以及用來進行快取。** 對 JDK 和應用程式伺服器使用 [自動上傳] 選項時，您現在可以在上傳 JDK 和應用程式伺服器時，或在使用 Azure 快取時，針對要使用的 URL 和儲存體帳戶指定 [(自動)]。 然後，這些功能就會自動使用您在 [發佈至 Azure]  對話方塊中選取的相同儲存體帳戶。 [在 Eclipse 中建立適用於 Azure 的 Hello World 應用程式]教學課程已更新為使用新的 [(自動)] 選項。
* **能夠設定 Azure 服務端點。** 指定服務端點以決定應用程式要部署至全域 Azure 平台、由位於中國的 21Vianet 所運作的 Azure 或私人 Azure 平台，並由該平台進行管理。 如需詳細資訊，請參閱 [Azure 服務端點]。
* **大型部署可以指定本機儲存資源。** 如果您的部署太大，預設的 approot 資料夾無法容納時，您現在可以指定本機儲存資源來做為 JDK 和應用程式伺服器的部署目的地。 如需詳細資訊，請參閱 [部署大型部署]。
* **支援 A6 和 A7 Azure 虛擬機器大小。** 您現在可以將雲端服務部署到高記憶體的 A6 和 A7 虛擬機器大小。 如需這些大小的詳細資訊，請參閱 [Azure 的虛擬機器和雲端服務大小]。
* **Azure Libraries for Java 程式庫封裝更新。** 此更新是以 [Microsoft Azure 用戶端 API]。

### <a name="may-1-2013"></a>2013 年 5 月 1 日
適用於 Eclipse 的 Azure 外掛程式 - 2013 年 5 月預覽已發行。 這個重大更新需搭配屬於必要條件的 Azure SDK 2.0 版，而且在您安裝外掛程式時會自動下載此 SDK。 此版本包含 2013 年 2 月預覽以來所推出的新功能、錯誤修正，以及一些根據意見反應所開發出的使用性增強功能：

* **自動上傳 JDK 和應用程式伺服器到 Azure 儲存體，以及從 Azure 儲存體進行部署。** 這個新選項會視需要將選取的 JDK 和應用程式伺服器自動上傳至指定的 Azure 儲存體帳戶，並從該處部署這些元件，而不必在部署封裝中內嵌或讓使用者屆時手動上傳。 這個普遍要求的功能可以大幅提高部署 JDK 和伺服器元件的便利性，對新手使用者來說更是如此。 如需使用這些選項的逐步解說，請參閱 [在 Eclipse 中建立適用於 Azure 的 Hello World 應用程式]。
* **集中式儲存體帳戶追蹤，以及能夠更輕鬆地參考儲存體帳戶 (透過下拉式控制項)。** 這適用於多個依賴儲存體的功能，例如 JDK 和伺服器元件部署，以及快取。 如需詳細資訊，請參閱 [Azure 儲存體帳戶清單]。
* **簡化發佈到雲端精靈的遠端存取設定。** 您只需要輸入使用者名稱和密碼就能啟用遠端存取，或是將欄位空白以讓遠端存取保持停用。
* **Azure Libraries for Java 程式庫封裝更新。** 此更新是以 [Microsoft Azure 用戶端 API]。
* **支援 Windows Server 2012 上的黏性工作階段。** 黏性工作階段先前只能在 Windows Server 2008 R2 上運作，但現在兩種雲端作業系統目標皆支援工作階段親和性。
* **改進封裝上傳效能。** 即使 JDK 和應用程式伺服器內嵌於部署封裝中，部署程序的上傳部分仍會比舊版快上近兩倍。

### <a name="february-8-2013"></a>2013 年 2 月 8 日
適用於 Eclipse 的 Azure 外掛程式 - 2013 年 2 月預覽已發行。 這個次要更新包含 2012 年 11 月預覽以來所推出的錯誤修正、根據意見反應所開發出的使用性增強功能，以及一些新增的功能：

* 支援從公用或私人 Azure blob 儲存體下載項目部署 JDK、應用程式伺服器和任意其他元件，而不必在部署至雲端時將它們納入部署封裝中。
* 透過在 [Azure 角色屬性] 的 [元件] 區段中新增 [上移] 和 [下移] 按鈕，而能夠變更角色的使用者定義元件的處理順序。
* 根據 **Azure Libraries for Java 封裝** 的 0.4.0 版，更新 [Microsoft Azure 用戶端 API]程式庫。

### <a name="november-5-2012"></a>2012 年 11 月 5 日
適用於 Eclipse 的 Azure 外掛程式 - 2012 年 11 月預覽已發行。 這個重大更新包含 2012 年 9 月預覽以來所推出的一些新功能、其他錯誤修正，以及根據意見反應所開發出的使用性增強功能：

* 支援將 Microsoft Windows Server 2012 做為雲端作業系統。
* 支援 Memcached 用戶端的 Azure 共置快取。
* 加入 Apache Qpid JMS 用戶端程式庫以利用 Azure AMQP 型傳訊。
* 改良 **新增專案** 精靈，在其結尾處加入新頁面，以讓使用者能夠在其專案中快速啟用幾個常見的重要功能：黏性工作階段、快取和遠端偵錯。
* 在計算模擬器中執行時會自動將角色執行個體減少為 1 個，以避免伺服器執行個體之間發生連接埠繫結衝突。

### <a name="september-28-2012"></a>2012 年 9 月 28 日
適用於 Eclipse 的 Azure 外掛程式 - 2012 年 9 月預覽已發行。 此服務更新包含 2012 年 8 月預覽以來所推出的一些其他錯誤修正，以及一些根據意見反應所開發出的現有功能使用性增強功能：

* 支援將 Microsoft Windows 8 和 Microsoft Windows Server 2012 做為開發作業系統，解決先前讓外掛程式無法在這些作業系統上正常運作的問題。
* 改善指定端點連接埠範圍的支援。
* 與包含空格的檔案路徑相關的錯誤修正。
* 改善角色內容功能表，以便更快速地存取角色特定的組態設定。
* 微幅調整 **發佈到雲端** 精靈和一些其他錯誤修正。

### <a name="august-28-2012"></a>2012 年 8 月 28 日
適用於 Eclipse 的 Azure 外掛程式 - 2012 年 8 月預覽已發行。 此服務更新包含 2012 年 7 月預覽以來所推出的其他錯誤修正，以及幾個根據意見反應所開發出的現有功能使用性增強功能：

* 在 [Azure 存取控制服務篩選器] 對話方塊內：
  * **有選項可將簽署的憑證內嵌** 在應用程式的 WAR 檔案中，以簡化雲端部署。
  * **有選項可將自我簽署的憑證建立** 在 ACS 篩選 UI 內。 如需 Azure 存取控制服務篩選器的其他資訊，請參閱 [如何使用 Eclipse 搭配 Azure 存取控制服務來驗證 Web 使用者]。
* 在 Azure 部署專案精靈 (也適用於角色的 [伺服器組態] 屬性頁) 內：
  * **自動找到 JDK 在電腦上的位置** (如有需要可予以覆寫)。
  * **自動偵測伺服器類型** (在您選取應用程式伺服器安裝目錄時)。

### <a name="july-15-2012"></a>2012 年 7 月 15 日
適用於 Eclipse 的 Azure 外掛程式 - 2012 年 7 月預覽可解決 2012 年 6 月版本發行後，使用者所找到及 (或) 報告的許多最高優先順序錯誤。 這只是服務更新，因此未包含任何新功能。

### <a name="june-7-2012"></a>2012 年 6 月 7日
適用於 Eclipse 的 Azure 外掛程式 - 2012 年 6 月 CTP 已發行。 新功能包括：

* **新增 Azure 部署專案精靈：** 可讓您直接在改進的精靈 UI 中選取 JDK、Java 應用程式伺服器和 Java 應用程式。 包含在立即可用的伺服器組態清單中以供您從中選擇的項目包括 Tomcat 6、Tomcat 7、GlassFish OSE 3、Jetty 7、Jetty 8、JBoss 6 和 JBoss 7 (獨立版)。 此外，您也可以自訂伺服器組態清單。 此 UI 改進功能可替代先前的主要方法，也就是拖放壓縮檔並複製到啟動指令碼。 該方法仍可正常運作，但可能只會用於更進階的案例。
* **伺服器組態角色屬性頁：** 可讓您在建立專案後，輕鬆地切換與部署相關聯的 JDK、Java 應用程式伺服器和應用程式。 如需詳細資訊，請參閱 [伺服器組態屬性]。
* **&quot;發佈到雲端&quot;精靈：**可讓您直接從 Eclipse 將專案輕鬆部署至 Azure，自動執行先前需要手動處理的麻煩認證提取程序、登入 Azure 管理入口網站、上傳套件等等。如需如何直接將專案部署至 Azure 的範例，請參閱[在 Eclipse 中建立適用於 Azure 的 Hello World 應用程式]。
* **Azure 工具列：** Eclipse 現已提供 Azure 工具列，其中包含叫用下列功能的按鈕：
  * ![][ic710879] **在 Azure 模擬器中執行**：在模擬器中執行專案。
  * ![][ic710880] **重設 Azure 模擬器**：重設模擬器。
  * ![][ic710881] **建置 Azure 的雲端封裝**：編譯封裝以進行部署。
  * ![][ic710876] **新增 Azure 部署專案**：建立新的 Azure 部署專案。
  * ![][ic710882] **發佈至 Azure 雲端**：將專案發佈到 Azure。
  * ![][ic710883] **取消發佈**：刪除部署。
  * [在 Eclipse 中建立適用於 Azure 的 Hello World 應用程式]中用到許多上述 Azure 工具列按鈕。
* **Azure Libraries for Java：** 在 Eclipse 中，現已提供做為單一 Azure Libraries for Java 程式庫封裝的一部分，搭配外掛程式安裝並且也包含所有必要相依項目。 您只需要將一個程式庫參考加入 Java 專案中，不必再另外下載任何項目。 如需詳細資訊，請參閱 [安裝 Azure Toolkit for Eclipse]。
* **在安裝外掛程式期間有提供 Microsoft JDBC Driver 4.0 for SQL Server：** 在安裝新外掛程式期間，可以安裝最新版的 Microsoft JDBC Driver for SQL Server。
* **在安裝外掛程式期間有提供 Azure 存取控制服務篩選器：** 納入工具組中做為 Eclipse 程式庫的這個新元件，可讓 Java Web 應用程式使用各種身分識別提供者 (例如 Google、Live.com 和 Yahoo!)，順暢地利用 Azure 存取控制服務 (ACS) 驗證。 您不需要自行撰寫驗證邏輯，只要設定幾個選項，然後讓篩選器執行讓使用者使用 ACS 進行登入的困難工作。 您可以完全專注於撰寫程式碼，以讓使用者根據要求物件內的篩選器傳回給應用程式的身分識別來存取資源。 如需如何使用 ACS 篩選器的教學課程，請參閱 [如何使用 Eclipse 搭配 Azure 存取控制服務來驗證 Web 使用者]。
* **自動偵測 Azure SDK 1.7 必要條件：** 在建立新的 Azure 部署專案時，會自動下載 Azure SDK 1.7 (如果尚未安裝)。
* **執行個體端點：** 允許直接存取連接埠端點以便與負載平衡的角色執行個體進行通訊。 您可以透過 [ [端點屬性] ] 頁面上的端點 UI 來新增執行個體端點。 這有助於在具有多個執行個體部署的案例中，對雲端上執行的特定計算執行個體進行遠端偵錯和 JMX 診斷。 
* **元件 UI：** 可讓進階使用者在專案的個別 Azure 角色與其他外部資源 (例如 Java 應用程式專案) 之間輕鬆設定專案相依性，同時也可輕鬆描述其部署邏輯。 如需詳細資訊，請參閱 [元件屬性]。
* **自動升級舊版專案：** 當您開啟的工作區具有以舊版外掛程式建立的 Azure 專案時，舊專案在 Eclipse 中將會顯示為關閉狀態，因為舊版專案與新版本不相容。 如果您嘗試開啟其中一個舊專案，將會啟動升級精靈。 如果您同意升級，將會建立新的專案 (名稱後會附加 **_Upgraded**)，並自動更新為使用新版本。 您可以視需要重新命名新專案。 升級過程中不會修改原始專案 (並且會保持關閉狀態)。

### <a name="december-10-2011"></a>2011 年 12 月 10日
適用於 Eclipse 的 Azure 外掛程式 - 2011 年 12 月 CTP 已發行。 新功能包括：

* **工作階段親和性 (&quot;黏性工作階段&quot;) 支援：**只需單一核取方塊，就可讓 Java 應用程式變成可設定狀態且進行叢集。 如需詳細資訊，請參閱 [工作階段親和性]。
* **預先製作的啟動指令碼範例：** 針對最熱門的 Java 伺服器 (Tomcat、Jetty、JBoss、GlassFish)，您可以直接從專案的範例目錄複製/貼到啟動指令碼。
* **即時模擬器啟動輸出：** 您現在可以在專用的主控台視窗中觀看啟動指令碼所有步驟的執行過程，這個視窗會顯示 Azure 在執行指令碼時的進度和失敗情形。
* **自動的輕量型 java.exe 監視：** 在 java.exe 停止執行時，這會使用部署中自動包含的輕量型預先製作指令碼強制回收角色。
* **遠端 Java 應用程式偵錯設定 UI：** 可讓您輕鬆地驅使 Eclipse 的遠端偵錯工具存取模擬器或 Azure 雲端中執行的 Java 應用程式，以便您可以逐步執行和即時偵錯 Java 程式碼。 如需詳細資訊，請參閱 [在 Eclipse 中偵錯 Azure 應用程式]。
* **本機儲存資源設定 UI：** 讓您不必再透過直接操作 XML 來設定本機資源。 這項功能也可讓您在本機資源已透過環境變數 (可直接從啟動指令碼參考) 進行部署之後，存取本機資源的有效檔案路徑。 如需詳細資訊，請參閱 [本機儲存體屬性]。
* **環境變數設定 UI：** 讓您不必再透過手動編輯設定 XML 來設定環境變數。 如需詳細資訊，請參閱 [環境變數屬性]。
* **適用於 SQL Azure 的 JDBC 驅動程式：** 透過外掛程式安裝為完美整合的 Eclipse 程式庫，讓您更輕鬆地針對 SQL Azure 進行程式設計。 
* **以內容功能表快速存取角色設定 UI**：只要以滑鼠右鍵按一下角色資料夾，然後按一下 [屬性]。
* **自訂 Azure 專案和角色資料夾圖示：** 讓您在工作區和專案中更容易看見和瀏覽。

## <a name="next-steps"></a>後續步驟

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[安裝 Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse-installation.md

[適用於 Eclipse 的 Azure 工具組的 Azure 登入指示]: azure-toolkit-for-eclipse-sign-in-instructions.md
[如何使用適用於 Eclipse 的 Azure 工具組，將 Web 應用程式發佈作為 Docker 容器]: ./azure-toolkit-for-eclipse-publish-as-docker-container.md
[使用適用於 Eclipse 的 Azure 工具組來管理儲存體帳戶]: ./azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer.md
[使用適用於 Eclipse 的 Azure 工具組來管理虛擬機器]: ./azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer.md

[Azure Java Developer Center]: https://docs.microsoft.com/java/azure

[Zulu OpenJDK 的 Azul Systems 網頁]: http://go.microsoft.com/fwlink/?LinkId=402457
[Azure 服務端點]: http://go.microsoft.com/fwlink/?LinkID=699526
[Azure 儲存體帳戶清單]: http://go.microsoft.com/fwlink/?LinkID=699528
[元件屬性]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[在 Eclipse 中建立適用於 Azure 的 Hello World 應用程式]: http://go.microsoft.com/fwlink/?LinkID=699533
[在 Eclipse 中偵錯 Azure 應用程式]: http://go.microsoft.com/fwlink/?LinkID=699535
[部署大型部署]: http://go.microsoft.com/fwlink/?LinkID=699536
[端點屬性]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[環境變數屬性]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[Eclipse 的 HDInsight Tools 外掛程式]: /azure/hdinsight/hdinsight-apache-spark-eclipse-tool-plugin
[如何使用 Eclipse 搭配 Azure 存取控制服務來驗證 Web 使用者]: http://go.microsoft.com/fwlink/?LinkID=264703
[如何使用 SSL 卸載]: http://go.microsoft.com/fwlink/?LinkID=699545
[本機儲存體屬性]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[Microsoft Azure 用戶端 API]: http://go.microsoft.com/fwlink/?LinkId=280397
[伺服器組態屬性]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[工作階段親和性]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL 卸載]: http://go.microsoft.com/fwlink/?LinkID=699549
[建立新的儲存體帳戶]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
[Azure 的虛擬機器和雲端服務大小]: http://go.microsoft.com/fwlink/?LinkId=466520

<!-- IMG List -->

[ic710876]: media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710877]: media/azure-toolkit-for-eclipse-whats-new/ic710877.png
[ic710879]: media/azure-toolkit-for-eclipse-whats-new/ic710879.png
[ic710880]: media/azure-toolkit-for-eclipse-whats-new/ic710880.png
[ic710881]: media/azure-toolkit-for-eclipse-whats-new/ic710881.png
[ic710876]: media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710882]: media/azure-toolkit-for-eclipse-whats-new/ic710882.png
[ic710883]: media/azure-toolkit-for-eclipse-whats-new/ic710883.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh694270.aspx -->
