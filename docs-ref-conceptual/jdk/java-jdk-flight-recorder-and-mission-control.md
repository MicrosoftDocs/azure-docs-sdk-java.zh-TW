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
# <a name="using-java-flight-recorder-jfr-and-mission-control"></a><span data-ttu-id="82e08-103">使用 Java Flight Recorder (JFR) 和 Mission Control</span><span class="sxs-lookup"><span data-stu-id="82e08-103">Using Java Flight Recorder (JFR) and Mission Control</span></span>

<span data-ttu-id="82e08-104">Zulu Mission Control 是經過完整測試的 JDK Mission Control 組建，其開放原始碼由 Oracle 建立於 2018 年，並且在 OpenJDK Umbrella 下以專案形式受到管理。</span><span class="sxs-lookup"><span data-stu-id="82e08-104">Zulu Mission Control is a fully-tested build of JDK Mission Control, which was open sourced by Oracle in 2018 and is managed as a project under the OpenJDK umbrella.</span></span> <span data-ttu-id="82e08-105">Mission Control 與 Flight Recorder 搭配運作，可為 Java 工作負載提供低額外負荷、互動式的監視和管理功能。</span><span class="sxs-lookup"><span data-stu-id="82e08-105">Coupled with Flight Recorder, Mission Control delivers low-overhead, interactive monitoring and management capabilities for Java workloads.</span></span>

<span data-ttu-id="82e08-106">Zulu Mission Control 與下列 JDK/JRE 相容：</span><span class="sxs-lookup"><span data-stu-id="82e08-106">Zulu Mission Control is compatible with the following JDKs/JREs:</span></span>

* <span data-ttu-id="82e08-107">Zulu 12.1 和更新版本</span><span class="sxs-lookup"><span data-stu-id="82e08-107">Zulu 12.1 and later</span></span>
* <span data-ttu-id="82e08-108">Zulu 11.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="82e08-108">Zulu 11.0 and later</span></span>
* <span data-ttu-id="82e08-109">Zulu 8u202 (8.36) 和更新版本</span><span class="sxs-lookup"><span data-stu-id="82e08-109">Zulu 8u202 (8.36) and later</span></span>
* <span data-ttu-id="82e08-110">Oracle OpenJDK 11+15 和更新版本</span><span class="sxs-lookup"><span data-stu-id="82e08-110">Oracle OpenJDK 11+15 and later</span></span>
* <span data-ttu-id="82e08-111">Oracle Java 11.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="82e08-111">Oracle Java 11.0 and later</span></span>
* <span data-ttu-id="82e08-112">Oracle Java 8.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="82e08-112">Oracle Java 8.0 and later</span></span>

<span data-ttu-id="82e08-113">請依照下列步驟安裝 Zulu Mission Control、連線至 Java 虛擬機器 (JVM)，以及取得執行中的應用程式各種層面的即時可見性。</span><span class="sxs-lookup"><span data-stu-id="82e08-113">Follow the steps below to install Zulu Mission Control, connect to a Java Virtual Machine (JVM), and gain real-time visibility into all aspects of a running application.</span></span>

1.  <span data-ttu-id="82e08-114">[安裝與 Zulu Mission Control 相容的 JDK/JRE](java-jdk-install.md)。</span><span class="sxs-lookup"><span data-stu-id="82e08-114">[Install a Zulu Mission Control compatible JDK/JRE](java-jdk-install.md).</span></span>

2.  <span data-ttu-id="82e08-115">從 [Azul 下載網站](https://www.azul.com/products/zulu-mission-control/)下載 Zulu Mission Control、為您的系統選擇適當的版本、將其儲存至本機，並切換至該目錄。</span><span class="sxs-lookup"><span data-stu-id="82e08-115">Download Zulu Mission Control from [the Azul download site](https://www.azul.com/products/zulu-mission-control/), choose the appropriate version for your system, save it locally, and change to that directory.</span></span>

3.  <span data-ttu-id="82e08-116">展開下載的檔案。</span><span class="sxs-lookup"><span data-stu-id="82e08-116">Expand the downloaded file.</span></span>

    <span data-ttu-id="82e08-117">**Linux：**</span><span class="sxs-lookup"><span data-stu-id="82e08-117">**Linux:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    <span data-ttu-id="82e08-118">**Windows：**</span><span class="sxs-lookup"><span data-stu-id="82e08-118">**Windows:**</span></span>

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    <span data-ttu-id="82e08-119">**MacOS：**</span><span class="sxs-lookup"><span data-stu-id="82e08-119">**MacOS:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4.  <span data-ttu-id="82e08-120">使用其中一個相容的 JDK 啟動您的 Java 應用程式。</span><span class="sxs-lookup"><span data-stu-id="82e08-120">Start your Java application using one of the compatible JDKs.</span></span> <span data-ttu-id="82e08-121">例如：</span><span class="sxs-lookup"><span data-stu-id="82e08-121">E.g.:</span></span>

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5.  <span data-ttu-id="82e08-122">啟動 Zulu Mission Control</span><span class="sxs-lookup"><span data-stu-id="82e08-122">Start Zulu Mission Control</span></span>

    <span data-ttu-id="82e08-123">**Linux：**</span><span class="sxs-lookup"><span data-stu-id="82e08-123">**Linux:**</span></span>

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    <span data-ttu-id="82e08-124">**Windows：**</span><span class="sxs-lookup"><span data-stu-id="82e08-124">**Windows:**</span></span>

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    <span data-ttu-id="82e08-125">**MacOS：**</span><span class="sxs-lookup"><span data-stu-id="82e08-125">**MacOS:**</span></span>

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6.  <span data-ttu-id="82e08-126">切換 Mission Control 的 JVM 安裝 (選擇性)</span><span class="sxs-lookup"><span data-stu-id="82e08-126">Switch the JVM installation for Mission Control (Optional)</span></span>

    <span data-ttu-id="82e08-127">在 Windows 上，*zmc.exe* 會使用在登錄中設定的預設 JVM 安裝。</span><span class="sxs-lookup"><span data-stu-id="82e08-127">On Windows, *zmc.exe* will use the default JVM installation configured in the registry.</span></span> <span data-ttu-id="82e08-128">Zulu Mission Control 必須從完整的 JDK 啟動，才能自動偵測本機 JVM 執行個體。</span><span class="sxs-lookup"><span data-stu-id="82e08-128">Zulu Mission Control must be launched from a full JDK to be able to detect local JVM instances automatically.</span></span> <span data-ttu-id="82e08-129">如果這是 JRE，您將看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="82e08-129">If this is a JRE, you will see the warning below:</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="82e08-130">![JDK 安裝為僅限 JRE 時出現的警告](../media/jdk/azul-jfr-1.png)</span><span class="sxs-lookup"><span data-stu-id="82e08-130">![Warning if JDK install is JRE-only](../media/jdk/azul-jfr-1.png)</span></span>

    <span data-ttu-id="82e08-131">若要變更 Mission Control 所使用的 JVM，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="82e08-131">To change the JVM used by Mission Control, follow these steps:</span></span> 
    1.  <span data-ttu-id="82e08-132">開啟 *zmc.ini* 組態檔，此檔案與 *zmc.exe* 位於相同的目錄中</span><span class="sxs-lookup"><span data-stu-id="82e08-132">Open *zmc.ini* configuration file, located in the same directory as the *zmc.exe*</span></span>
    2.  <span data-ttu-id="82e08-133">在 `-vmargs` 一行前面加入兩行：</span><span class="sxs-lookup"><span data-stu-id="82e08-133">Before the line `-vmargs`, add two lines:</span></span>
        * <span data-ttu-id="82e08-134">在第一行中，撰寫 `–vm`</span><span class="sxs-lookup"><span data-stu-id="82e08-134">On the first line, write `–vm`</span></span>
        * <span data-ttu-id="82e08-135">在第二行中，撰寫您的 JDK 安裝路徑。</span><span class="sxs-lookup"><span data-stu-id="82e08-135">On the second line, write the path to your JDK installation.</span></span> <span data-ttu-id="82e08-136">(例如 `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`)。</span><span class="sxs-lookup"><span data-stu-id="82e08-136">(For example, `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`).</span></span>

7.  <span data-ttu-id="82e08-137">找出執行您應用程式的 JVM</span><span class="sxs-lookup"><span data-stu-id="82e08-137">Locate the JVM running your application</span></span>
    1.  <span data-ttu-id="82e08-138">在 Zulu Mission Control 視窗的左上方窗格中，按一下標示為「JVM 瀏覽器」  的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="82e08-138">In the upper left pane of the Zulu Mission Control window click on the tab labelled **JVM Browser**.</span></span>
    2.  <span data-ttu-id="82e08-139">針對執行應用程式的 JVM 執行個體，選取並展開左上方的清單項目。</span><span class="sxs-lookup"><span data-stu-id="82e08-139">Select and expand the list item in the upper left for your the JVM instance running your application.</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="82e08-140">![為您的 JVM 執行個體展開左上方的清單項目](../media/jdk/azul-jfr-2.png)</span><span class="sxs-lookup"><span data-stu-id="82e08-140">![Expand the list item in the upper-left for your JVM instance](../media/jdk/azul-jfr-2.png)</span></span>


8.  <span data-ttu-id="82e08-141">視需要啟動飛行記錄</span><span class="sxs-lookup"><span data-stu-id="82e08-141">Start a Flight Recording, if necessary</span></span>
    1.  <span data-ttu-id="82e08-142">如果 Flight Recorder 顯示「沒有記錄」，請以滑鼠右鍵按一下 [JVM 瀏覽器] 索引標籤中的 Flight Recorder 一行，然後選取 [啟動飛行記錄...] </span><span class="sxs-lookup"><span data-stu-id="82e08-142">If the Flight Recorder displays "No Recordings", start one by right-clicking on the Flight Recorder line in the JVM Browser tab and selecting **Start Flight Recording...**</span></span>
    2.  <span data-ttu-id="82e08-143">選取固定的持續時間記錄或連續記錄，以及分析組態 (細部) 或連續組態 (較低的額外負荷)，然後按一下 [完成]  。</span><span class="sxs-lookup"><span data-stu-id="82e08-143">Select either a fixed duration recording or a continuous recording, and either a Profiling configuration (fine-grained) or a Continuous configuration (lower overhead), then click **Finish**.</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="82e08-144">![啟動飛行記錄](../media/jdk/azul-jfr-3.png)</span><span class="sxs-lookup"><span data-stu-id="82e08-144">![Start a Flight Recording](../media/jdk/azul-jfr-3.png)</span></span>

9.  <span data-ttu-id="82e08-145">傾印飛行記錄</span><span class="sxs-lookup"><span data-stu-id="82e08-145">Dump the Flight Recording</span></span>
    1.  <span data-ttu-id="82e08-146">飛行記錄應該會出現在 JVM 瀏覽器中的 Flight Recorder 這一行下方。</span><span class="sxs-lookup"><span data-stu-id="82e08-146">A Flight Recording should appear below the Flight Recorder line in the JVM Browser.</span></span> <span data-ttu-id="82e08-147">以滑鼠右鍵按一下代表飛行記錄的這一行，然後選取 [傾印整個記錄]  。</span><span class="sxs-lookup"><span data-stu-id="82e08-147">Right-click on the line representing the Flight Recording and select **Dump whole recording**.</span></span>
    2.  <span data-ttu-id="82e08-148">Zulu Mission Control 視窗右側的大型窗格中會出現新的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="82e08-148">A new tab will appear in the large pane on the right side of the Zulu Mission Control window.</span></span> <span data-ttu-id="82e08-149">此窗格代表剛剛從執行應用程式的 JVM 傾印的飛行記錄。</span><span class="sxs-lookup"><span data-stu-id="82e08-149">This pane represents the Flight Recording just dumped from the JVM running your application.</span></span>

10. <span data-ttu-id="82e08-150">使用 Zulu Mission Control 檢查飛行記錄</span><span class="sxs-lookup"><span data-stu-id="82e08-150">Examine the Flight Recording using Zulu Mission Control</span></span>
    1.  <span data-ttu-id="82e08-151">在 Zulu Mission Control 視窗的左側窗格中，選取標示為「大綱」  的索引標籤 (如果尚未啟用)。</span><span class="sxs-lookup"><span data-stu-id="82e08-151">If not already activated, select the tab labelled **Outline** in the left pane of the Zulu Mission Control Window.</span></span> <span data-ttu-id="82e08-152">此索引標籤包含不同的檢視，分別顯示在飛行記錄中收集到的資料。</span><span class="sxs-lookup"><span data-stu-id="82e08-152">This tab contains different views of the data collected in the Flight Recording.</span></span>
 
    > [!div class="mx-imgBorder"]
    <span data-ttu-id="82e08-153">![檢閱飛行記錄](../media/jdk/azul-jfr-4.png)</span><span class="sxs-lookup"><span data-stu-id="82e08-153">![Review the Fliight Recording](../media/jdk/azul-jfr-4.png)</span></span>

## <a name="resources"></a><span data-ttu-id="82e08-154">資源</span><span class="sxs-lookup"><span data-stu-id="82e08-154">Resources</span></span>

<span data-ttu-id="82e08-155">我們也準備了[示範影片](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)，由 Azul Systems Deputy CTO Simon Ritter 負責講解。</span><span class="sxs-lookup"><span data-stu-id="82e08-155">We have also prepared a [demonstration video](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/) narrated by Azul Systems Deputy CTO Simon Ritter.</span></span> <span data-ttu-id="82e08-156">影片會引導您完成 Flight Recorder 和 Zulu Mission Control 的組態與設定。</span><span class="sxs-lookup"><span data-stu-id="82e08-156">The video walks you through the configuration and setup of both Flight Recorder and Zulu Mission Control.</span></span> <span data-ttu-id="82e08-157">Flight Recorder 的討論從 31:30 開始。</span><span class="sxs-lookup"><span data-stu-id="82e08-157">The Flight Recorder discussion starts at 31:30.</span></span>

