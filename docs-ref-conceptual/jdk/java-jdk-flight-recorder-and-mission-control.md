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
# <a name="use-java-flight-recorder-and-mission-control"></a><span data-ttu-id="6e410-103">使用 Java Flight Recorder 和 Mission Control</span><span class="sxs-lookup"><span data-stu-id="6e410-103">Use Java Flight Recorder and Mission Control</span></span>

<span data-ttu-id="6e410-104">Zulu Mission Control 是經過完整測試的 JDK Mission Control 組建，其開放原始碼由 Oracle 於 2018 年建立，並且在 OpenJDK Umbrella 下以專案形式受到管理。</span><span class="sxs-lookup"><span data-stu-id="6e410-104">Zulu Mission Control is a fully-tested build of JDK Mission Control, which was open-sourced by Oracle in 2018 and is managed as a project under the OpenJDK umbrella.</span></span> <span data-ttu-id="6e410-105">Mission Control 與 Java Flight Recorder (JFR) 搭配運作，可為 Java 工作負載提供低額外負荷、互動式的監視和管理功能。</span><span class="sxs-lookup"><span data-stu-id="6e410-105">Coupled with Java Flight Recorder (JFR), Mission Control delivers low-overhead, interactive monitoring and management capabilities for Java workloads.</span></span>

<span data-ttu-id="6e410-106">Zulu Mission Control 與下列的 Java Development Kit (JDK) 和 Java Runtime Environment (JRE) 相容：</span><span class="sxs-lookup"><span data-stu-id="6e410-106">Zulu Mission Control is compatible with the following Java Development Kits (JDKs) and Java Runtime Environments (JREs):</span></span>

* <span data-ttu-id="6e410-107">Zulu 12.1 和更新版本</span><span class="sxs-lookup"><span data-stu-id="6e410-107">Zulu 12.1 and later</span></span>
* <span data-ttu-id="6e410-108">Zulu 11.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="6e410-108">Zulu 11.0 and later</span></span>
* <span data-ttu-id="6e410-109">Zulu 8u202 (8.36) 和更新版本</span><span class="sxs-lookup"><span data-stu-id="6e410-109">Zulu 8u202 (8.36) and later</span></span>
* <span data-ttu-id="6e410-110">Oracle OpenJDK 11 與 15 和更新版本</span><span class="sxs-lookup"><span data-stu-id="6e410-110">Oracle OpenJDK 11 and 15 and later</span></span>
* <span data-ttu-id="6e410-111">Oracle Java 11.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="6e410-111">Oracle Java 11.0 and later</span></span>
* <span data-ttu-id="6e410-112">Oracle Java 8.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="6e410-112">Oracle Java 8.0 and later</span></span>

## <a name="install-zulu-mission-control-and-connect-to-a-jvm"></a><span data-ttu-id="6e410-113">安裝 Zulu Mission Control 並連線至 JVM</span><span class="sxs-lookup"><span data-stu-id="6e410-113">Install Zulu Mission Control and connect to a JVM</span></span>

<span data-ttu-id="6e410-114">若要安裝 Zulu Mission Control、連線至 Java 虛擬機器 (JVM)，以及取得執行中的應用程式各種層面的即時可見性，請執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="6e410-114">To install Zulu Mission Control, connect to a Java Virtual Machine (JVM), and gain real-time visibility into all aspects of a running application, do the following:</span></span>

1.  <span data-ttu-id="6e410-115">[安裝與 Zulu Mission Control 相容的 JDK 和 JRE](java-jdk-install.md)。</span><span class="sxs-lookup"><span data-stu-id="6e410-115">[Install a Zulu Mission Control-compatible JDK and JRE](java-jdk-install.md).</span></span>

1.  <span data-ttu-id="6e410-116">[下載 Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/)、為您的系統選擇適當的版本、將其儲存至本機，並切換至該目錄。</span><span class="sxs-lookup"><span data-stu-id="6e410-116">[Download Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/), choose the appropriate version for your system, save it locally, and change to that directory.</span></span>

1.  <span data-ttu-id="6e410-117">展開下載的檔案。</span><span class="sxs-lookup"><span data-stu-id="6e410-117">Expand the downloaded file.</span></span>

    <span data-ttu-id="6e410-118">**Linux：**</span><span class="sxs-lookup"><span data-stu-id="6e410-118">**Linux:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    <span data-ttu-id="6e410-119">**Windows：**</span><span class="sxs-lookup"><span data-stu-id="6e410-119">**Windows:**</span></span>

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    <span data-ttu-id="6e410-120">**macOS：**</span><span class="sxs-lookup"><span data-stu-id="6e410-120">**macOS:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

1.  <span data-ttu-id="6e410-121">使用其中一個相容的 JDK 啟動您的 Java 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6e410-121">Start your Java application by using one of the compatible JDKs.</span></span> <span data-ttu-id="6e410-122">例如︰</span><span class="sxs-lookup"><span data-stu-id="6e410-122">For example:</span></span>

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

1.  <span data-ttu-id="6e410-123">啟動 Zulu Mission Control。</span><span class="sxs-lookup"><span data-stu-id="6e410-123">Start Zulu Mission Control.</span></span>

    <span data-ttu-id="6e410-124">**Linux：**</span><span class="sxs-lookup"><span data-stu-id="6e410-124">**Linux:**</span></span>

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    <span data-ttu-id="6e410-125">**Windows：**</span><span class="sxs-lookup"><span data-stu-id="6e410-125">**Windows:**</span></span>

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    <span data-ttu-id="6e410-126">**macOS：**</span><span class="sxs-lookup"><span data-stu-id="6e410-126">**macOS:**</span></span>

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

1.  <span data-ttu-id="6e410-127">(選擇性) 切換 Mission Control 的 JVM 安裝。</span><span class="sxs-lookup"><span data-stu-id="6e410-127">(Optional) Switch the JVM installation for Mission Control.</span></span>

    <span data-ttu-id="6e410-128">在 Windows 裝置上，*zmc.exe* 會使用在登錄中設定的預設 JVM 安裝。</span><span class="sxs-lookup"><span data-stu-id="6e410-128">On Windows devices, *zmc.exe* uses the default JVM installation that's configured in the registry.</span></span> <span data-ttu-id="6e410-129">Zulu Mission Control 必須從完整的 JDK 啟動，才能自動偵測本機 JVM 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e410-129">Zulu Mission Control must be launched from a full JDK to be able to detect local JVM instances automatically.</span></span> <span data-ttu-id="6e410-130">如果安裝是 JRE，則不會偵測到任何 JVM，且您將收到下列警告：</span><span class="sxs-lookup"><span data-stu-id="6e410-130">If the installation is a JRE, no JVM will be detected, and you will receive the following warning:</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="6e410-131">![JDK 安裝為僅限 JRE 時出現的警告](../media/jdk/azul-jfr-1.png)</span><span class="sxs-lookup"><span data-stu-id="6e410-131">![Warning if JDK installation is JRE-only](../media/jdk/azul-jfr-1.png)</span></span>

    <span data-ttu-id="6e410-132">若要變更 Mission Control 所使用的 JVM，請執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="6e410-132">To change the JVM that's used by Mission Control, do the following:</span></span> 

    <span data-ttu-id="6e410-133">a.</span><span class="sxs-lookup"><span data-stu-id="6e410-133">a.</span></span> <span data-ttu-id="6e410-134">開啟 *zmc.ini* 組態檔，此檔案位於 *zmc.exe* 檔案所在的相同目錄中。</span><span class="sxs-lookup"><span data-stu-id="6e410-134">Open the *zmc.ini* configuration file, which is in the same directory as the *zmc.exe* file.</span></span>

    <span data-ttu-id="6e410-135">b.</span><span class="sxs-lookup"><span data-stu-id="6e410-135">b.</span></span> <span data-ttu-id="6e410-136">在 `-vmargs` 一行前面加入兩行：</span><span class="sxs-lookup"><span data-stu-id="6e410-136">Before the line `-vmargs`, add two lines:</span></span>  

       * <span data-ttu-id="6e410-137">在第一行中，輸入 `–vm`。</span><span class="sxs-lookup"><span data-stu-id="6e410-137">On the first line, enter `–vm`.</span></span>  
       * <span data-ttu-id="6e410-138">在第二行中，輸入您的 JDK 安裝路徑 (例如 `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`)。</span><span class="sxs-lookup"><span data-stu-id="6e410-138">On the second line, enter the path to your JDK installation (for example, `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`).</span></span>

1.  <span data-ttu-id="6e410-139">執行下列作業，以找出執行您的應用程式的 JVM：</span><span class="sxs-lookup"><span data-stu-id="6e410-139">Locate the JVM that's running your application by doing the following:</span></span>

    <span data-ttu-id="6e410-140">a.</span><span class="sxs-lookup"><span data-stu-id="6e410-140">a.</span></span> <span data-ttu-id="6e410-141">在 Zulu Mission Control 視窗的左窗格中，選取 [JVM 瀏覽器]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="6e410-141">In the left pane of the Zulu Mission Control window, select the **JVM Browser** tab.</span></span>

    <span data-ttu-id="6e410-142">b.</span><span class="sxs-lookup"><span data-stu-id="6e410-142">b.</span></span> <span data-ttu-id="6e410-143">在清單中，選取並展開執行您的應用程式的 JVM 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e410-143">In the list, select and expand the JVM instance that's running your application.</span></span>

    ![展開的清單中顯示的 JVM 執行個體](../media/jdk/azul-jfr-2.png)


1.  <span data-ttu-id="6e410-145">視需要啟動飛行記錄。</span><span class="sxs-lookup"><span data-stu-id="6e410-145">Start a flight recording, if necessary.</span></span>

    <span data-ttu-id="6e410-146">a.</span><span class="sxs-lookup"><span data-stu-id="6e410-146">a.</span></span> <span data-ttu-id="6e410-147">在左窗格的 [Flight Recorder]  下方，如果顯示「沒有記錄」  訊息，請以滑鼠右鍵按一下 [Flight Recorder]  並選取 [啟動飛行記錄]  ，以啟動記錄。</span><span class="sxs-lookup"><span data-stu-id="6e410-147">In the left pane, under **Flight Recorder**, if a *No Recordings* message is displayed, start a recording by right-clicking **Flight Recorder** and then selecting **Start Flight Recording**.</span></span>

    <span data-ttu-id="6e410-148">b.</span><span class="sxs-lookup"><span data-stu-id="6e410-148">b.</span></span> <span data-ttu-id="6e410-149">選取 [固定時間記錄]  或 [連續記錄]  ，並選取 [分析]  組態 (細部) 或 [連續]  組態 (較低的額外負荷)，然後選取 [完成]  。</span><span class="sxs-lookup"><span data-stu-id="6e410-149">Select either **Time fixed recording** or **Continuous recording**, and either a **Profiling** configuration (fine-grained) or a **Continuous** configuration (lower overhead), and then select **Finish**.</span></span>

    ![啟動飛行記錄](../media/jdk/azul-jfr-3.png)

    <span data-ttu-id="6e410-151">飛行記錄應該會出現在 JVM 瀏覽器中的 **Flight Recorder** 這一行下方。</span><span class="sxs-lookup"><span data-stu-id="6e410-151">A flight recording should appear below the **Flight Recorder** line in the JVM browser.</span></span>

1. <span data-ttu-id="6e410-152">傾印飛行記錄。</span><span class="sxs-lookup"><span data-stu-id="6e410-152">Dump the flight recording.</span></span> <span data-ttu-id="6e410-153">若要這麼做，請以滑鼠右鍵按一下代表飛行記錄的這一行，然後選取 [傾印整個記錄]  。</span><span class="sxs-lookup"><span data-stu-id="6e410-153">To do so, right-click the line that represents the flight recording, and then select **Dump whole recording**.</span></span>

    <span data-ttu-id="6e410-154">Zulu Mission Control 視窗右側的大型窗格中會出現新的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="6e410-154">A new tab appears in the large pane on the right side of the Zulu Mission Control window.</span></span> <span data-ttu-id="6e410-155">此窗格代表剛剛從執行應用程式的 JVM 傾印的飛行記錄。</span><span class="sxs-lookup"><span data-stu-id="6e410-155">This pane represents the flight recording that was just dumped from the JVM that's running your application.</span></span>

1. <span data-ttu-id="6e410-156">使用 Zulu Mission Control 檢查飛行記錄。</span><span class="sxs-lookup"><span data-stu-id="6e410-156">Examine the flight recording by using Zulu Mission Control.</span></span> <span data-ttu-id="6e410-157">若要這麼做，請在 Zulu Mission Control 視窗的左窗格中，選取 [大綱]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="6e410-157">To do so, select the **Outline** tab in the left pane of the Zulu Mission Control window.</span></span> <span data-ttu-id="6e410-158">此索引標籤會顯示在飛行記錄中收集到之資料的不同檢視。</span><span class="sxs-lookup"><span data-stu-id="6e410-158">This tab displays various views of the data that's collected in the flight recording.</span></span>
 
    ![檢閱飛行記錄](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a><span data-ttu-id="6e410-160">資源</span><span class="sxs-lookup"><span data-stu-id="6e410-160">Resources</span></span>

<span data-ttu-id="6e410-161">若要深入了解，請移至 Azul Systems 網站，並檢視 [Azul 網路研討會：開放原始碼 Flight Recorder 和 Mission Control - 管理和測量 OpenJDK 8 效能](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)。</span><span class="sxs-lookup"><span data-stu-id="6e410-161">To learn more, go to the Azul Systems site and view [Azul Webinar: Open Source Flight Recorder and Mission Control - Managing and Measuring OpenJDK 8 Performance](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/).</span></span> <span data-ttu-id="6e410-162">這段影片由 Azul Systems Deputy CTO Simon Ritter 負責講解。</span><span class="sxs-lookup"><span data-stu-id="6e410-162">The video is narrated by Azul Systems Deputy CTO Simon Ritter.</span></span> <span data-ttu-id="6e410-163">Flight Recorder 的討論從 31:30 開始。</span><span class="sxs-lookup"><span data-stu-id="6e410-163">The Flight Recorder discussion starts at 31:30.</span></span>

