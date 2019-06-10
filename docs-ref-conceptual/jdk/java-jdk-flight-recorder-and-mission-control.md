---
title: Java Flight Recorder 和 Mission Control
description: 使用 Java Flight Recorder 和 Mission Control 收集和檢閱應用程式資料的指引。
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: b27e0f741f1322b7e8e1df363dbb2f40a3d34d53
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568561"
---
# <a name="using-java-flight-recorder-jfr-and-mission-control"></a>使用 Java Flight Recorder (JFR) 和 Mission Control

Zulu Mission Control 是經過完整測試的 JDK Mission Control 組建，其開放原始碼由 Oracle 建立於 2018 年，並且在 OpenJDK Umbrella 下以專案形式受到管理。 Mission Control 與 Flight Recorder 搭配運作，可為 Java 工作負載提供低額外負荷、互動式的監視和管理功能。

Zulu Mission Control 與下列 JDK/JRE 相容：

* Zulu 12.1 和更新版本
* Zulu 11.0 和更新版本
* Zulu 8u202 (8.36) 和更新版本
* Oracle OpenJDK 11+15 和更新版本
* Oracle Java 11.0 和更新版本
* Oracle Java 8.0 和更新版本

請依照下列步驟安裝 Zulu Mission Control、連線至 Java 虛擬機器 (JVM)，以及取得執行中的應用程式各種層面的即時可見性。

1.  [安裝與 Zulu Mission Control 相容的 JDK/JRE](java-jdk-install.md)。

2.  從 [Azul 下載網站](https://www.azul.com/products/zulu-mission-control/)下載 Zulu Mission Control、為您的系統選擇適當的版本、將其儲存至本機，並切換至該目錄。

3.  展開下載的檔案。

    **Linux：**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows：**

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    **MacOS：**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4.  使用其中一個相容的 JDK 啟動您的 Java 應用程式。 例如：

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5.  啟動 Zulu Mission Control

    **Linux：**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows：**

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    **MacOS：**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6.  切換 Mission Control 的 JVM 安裝 (選擇性)

    在 Windows 上，*zmc.exe* 會使用在登錄中設定的預設 JVM 安裝。 Zulu Mission Control 必須從完整的 JDK 啟動，才能自動偵測本機 JVM 執行個體。 如果這是 JRE，您將看到下列警告：

    > [!div class="mx-imgBorder"]
    ![JDK 安裝為僅限 JRE 時出現的警告](../media/jdk/azul-jfr-1.png)

    若要變更 Mission Control 所使用的 JVM，請遵循下列步驟： 
    1.  開啟 *zmc.ini* 組態檔，此檔案與 *zmc.exe* 位於相同的目錄中
    2.  在 `-vmargs` 一行前面加入兩行：
        * 在第一行中，撰寫 `–vm`
        * 在第二行中，撰寫您的 JDK 安裝路徑。 (例如 `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`)。

7.  找出執行您應用程式的 JVM
    1.  在 Zulu Mission Control 視窗的左上方窗格中，按一下標示為「JVM 瀏覽器」  的索引標籤。
    2.  針對執行應用程式的 JVM 執行個體，選取並展開左上方的清單項目。

    > [!div class="mx-imgBorder"]
    ![為您的 JVM 執行個體展開左上方的清單項目](../media/jdk/azul-jfr-2.png)


8.  視需要啟動飛行記錄
    1.  如果 Flight Recorder 顯示「沒有記錄」，請以滑鼠右鍵按一下 [JVM 瀏覽器] 索引標籤中的 Flight Recorder 一行，然後選取 [啟動飛行記錄...] 
    2.  選取固定的持續時間記錄或連續記錄，以及分析組態 (細部) 或連續組態 (較低的額外負荷)，然後按一下 [完成]  。

    > [!div class="mx-imgBorder"]
    ![啟動飛行記錄](../media/jdk/azul-jfr-3.png)

9.  傾印飛行記錄
    1.  飛行記錄應該會出現在 JVM 瀏覽器中的 Flight Recorder 這一行下方。 以滑鼠右鍵按一下代表飛行記錄的這一行，然後選取 [傾印整個記錄]  。
    2.  Zulu Mission Control 視窗右側的大型窗格中會出現新的索引標籤。 此窗格代表剛剛從執行應用程式的 JVM 傾印的飛行記錄。

10. 使用 Zulu Mission Control 檢查飛行記錄
    1.  在 Zulu Mission Control 視窗的左側窗格中，選取標示為「大綱」  的索引標籤 (如果尚未啟用)。 此索引標籤包含不同的檢視，分別顯示在飛行記錄中收集到的資料。
 
    > [!div class="mx-imgBorder"]
    ![檢閱飛行記錄](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a>資源

我們也準備了[示範影片](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)，由 Azul Systems Deputy CTO Simon Ritter 負責講解。 影片會引導您完成 Flight Recorder 和 Zulu Mission Control 的組態與設定。 Flight Recorder 的討論從 31:30 開始。

