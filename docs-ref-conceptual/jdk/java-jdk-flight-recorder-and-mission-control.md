---
title: Java Flight Recorder 和 Mission Control
description: 使用 Java Flight Recorder 和 Mission Control 收集和檢閱應用程式資料的指引。
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: 64f64f2e5891fccf9d62510f39bd99d73457d590
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533627"
---
# <a name="use-java-flight-recorder-and-mission-control"></a>使用 Java Flight Recorder 和 Mission Control

Zulu Mission Control 是經過完整測試的 JDK Mission Control 組建，其開放原始碼由 Oracle 於 2018 年建立，並且在 OpenJDK Umbrella 下以專案形式受到管理。 Mission Control 與 Java Flight Recorder (JFR) 搭配運作，可為 Java 工作負載提供低額外負荷、互動式的監視和管理功能。

Zulu Mission Control 與下列的 Java Development Kit (JDK) 和 Java Runtime Environment (JRE) 相容：

* Zulu 12.1 和更新版本
* Zulu 11.0 和更新版本
* Zulu 8u202 (8.36) 和更新版本
* Oracle OpenJDK 11 與 15 和更新版本
* Oracle Java 11.0 和更新版本
* Oracle Java 8.0 和更新版本

## <a name="install-zulu-mission-control-and-connect-to-a-jvm"></a>安裝 Zulu Mission Control 並連線至 JVM

若要安裝 Zulu Mission Control、連線至 Java 虛擬機器 (JVM)，以及取得執行中的應用程式各種層面的即時可見性，請執行下列作業：

1.  [安裝與 Zulu Mission Control 相容的 JDK 和 JRE](java-jdk-install.md)。

1.  [下載 Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/)、為您的系統選擇適當的版本、將其儲存至本機，並切換至該目錄。

1.  展開下載的檔案。

    **Linux：**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows：**

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    **macOS：**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

1.  使用其中一個相容的 JDK 啟動您的 Java 應用程式。 例如︰

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

1.  啟動 Zulu Mission Control。

    **Linux：**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows：**

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    **macOS：**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

1.  (選擇性) 切換 Mission Control 的 JVM 安裝。

    在 Windows 裝置上，*zmc.exe* 會使用在登錄中設定的預設 JVM 安裝。 Zulu Mission Control 必須從完整的 JDK 啟動，才能自動偵測本機 JVM 執行個體。 如果安裝是 JRE，則不會偵測到任何 JVM，且您將收到下列警告：

    > [!div class="mx-imgBorder"]
    ![JDK 安裝為僅限 JRE 時出現的警告](../media/jdk/azul-jfr-1.png)

    若要變更 Mission Control 所使用的 JVM，請執行下列作業： 

    a. 開啟 *zmc.ini* 組態檔，此檔案位於 *zmc.exe* 檔案所在的相同目錄中。

    b. 在 `-vmargs` 一行前面加入兩行：  

       * 在第一行中，輸入 `–vm`。  
       * 在第二行中，輸入您的 JDK 安裝路徑 (例如 `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`)。

1.  執行下列作業，以找出執行您的應用程式的 JVM：

    a. 在 Zulu Mission Control 視窗的左窗格中，選取 [JVM 瀏覽器]  索引標籤。

    b. 在清單中，選取並展開執行您的應用程式的 JVM 執行個體。

    ![展開的清單中顯示的 JVM 執行個體](../media/jdk/azul-jfr-2.png)


1.  視需要啟動飛行記錄。

    a. 在左窗格的 [Flight Recorder]  下方，如果顯示「沒有記錄」  訊息，請以滑鼠右鍵按一下 [Flight Recorder]  並選取 [啟動飛行記錄]  ，以啟動記錄。

    b. 選取 [固定時間記錄]  或 [連續記錄]  ，並選取 [分析]  組態 (細部) 或 [連續]  組態 (較低的額外負荷)，然後選取 [完成]  。

    ![啟動飛行記錄](../media/jdk/azul-jfr-3.png)

    飛行記錄應該會出現在 JVM 瀏覽器中的 **Flight Recorder** 這一行下方。

1. 傾印飛行記錄。 若要這麼做，請以滑鼠右鍵按一下代表飛行記錄的這一行，然後選取 [傾印整個記錄]  。

    Zulu Mission Control 視窗右側的大型窗格中會出現新的索引標籤。 此窗格代表剛剛從執行應用程式的 JVM 傾印的飛行記錄。

1. 使用 Zulu Mission Control 檢查飛行記錄。 若要這麼做，請在 Zulu Mission Control 視窗的左窗格中，選取 [大綱]  索引標籤。 此索引標籤會顯示在飛行記錄中收集到之資料的不同檢視。
 
    ![檢閱飛行記錄](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a>資源

若要深入了解，請移至 Azul Systems 網站，並檢視 [Azul 網路研討會：開放原始碼 Flight Recorder 和 Mission Control - 管理和測量 OpenJDK 8 效能](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)。 這段影片由 Azul Systems Deputy CTO Simon Ritter 負責講解。 Flight Recorder 的討論從 31:30 開始。

