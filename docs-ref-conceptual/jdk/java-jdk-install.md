---
title: 安裝適用於 Azure 和 Azure Stack 的 Azul Zulu JDK
description: 如何安裝 Windows、Linux 與 Mac 的 Azure 開發所適用的 Azul Zulu Java Development Kit (JDK)
author: erickson-doug
manager: douge
ms.author: douge
ms.date: 4/19/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: 33d2206f9a0a1cc9fadc60b17c1a93f66c592fe3
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568591"
---
# <a name="install-the-jdk-for-azure-and-azure-stack"></a><span data-ttu-id="4c0d2-103">安裝適用於 Azure 和 Azure Stack 的 JDK</span><span class="sxs-lookup"><span data-stu-id="4c0d2-103">Install the JDK for Azure and Azure Stack</span></span>

<span data-ttu-id="4c0d2-104">OpenJDK 的 Azul Zulu Enterprise 組建是免費、多平台、可實際執行的 OpenJDK 散發套件，適用於 Azure 和 Azure Stack，由 Microsoft 與 Azul Systems 提供支援。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-104">Azul Zulu Enterprise builds of OpenJDK are a no-cost, multi-platform, production-ready distribution of the OpenJDK for Azure and Azure Stack backed by Microsoft and Azul Systems.</span></span> <span data-ttu-id="4c0d2-105">其中包含建置及執行 Java SE 應用程式所需的所有元件。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-105">They contain all the components for building and running Java SE applications.</span></span>

<span data-ttu-id="4c0d2-106">目前有[多種下載套件分別支援各類用戶端作業系統](https://www.azul.com/downloads/azure-only/zulu/)。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-106">There are [multiple download package types supported for each client OS](https://www.azul.com/downloads/azure-only/zulu/).</span></span> <span data-ttu-id="4c0d2-107">您也可以從 Azure Marketplace 資源庫取得適用於下列平台的虛擬機器映像：</span><span class="sxs-lookup"><span data-stu-id="4c0d2-107">You can also get a virtual machine image from the Azure Marketplace Gallery for the following platforms:</span></span>

  * [<span data-ttu-id="4c0d2-108">Azul Zulu：Ubuntu 18.04 上的 Java 8</span><span class="sxs-lookup"><span data-stu-id="4c0d2-108">Azul Zulu: Java 8 on Ubuntu 18.04</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-ubuntu-1804)
  * [<span data-ttu-id="4c0d2-109">Azul Zulu：Windows Server 2019 上的 Java 8</span><span class="sxs-lookup"><span data-stu-id="4c0d2-109">Azul Zulu: Java 8 on Windows Server 2019</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-windows-2019)
  
  * [<span data-ttu-id="4c0d2-110">Azul Zulu：Ubuntu 18.04 上的 Java 11</span><span class="sxs-lookup"><span data-stu-id="4c0d2-110">Azul Zulu: Java 11 on Ubuntu 18.04</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-ubuntu-1804)
  * [<span data-ttu-id="4c0d2-111">Azul Zulu：Windows Server 2019 上的 Java 11</span><span class="sxs-lookup"><span data-stu-id="4c0d2-111">Azul Zulu: Java 11 on Windows Server 2019</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-windows-2019)


> [!NOTE]
> <span data-ttu-id="4c0d2-112">以下指示以 64 位元 Java 8 版的 JDK 為目標。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-112">These instructions target the 64-bit Java 8 version of the JDK.</span></span> <span data-ttu-id="4c0d2-113">Azul 也提供獨立安裝形式的 Java Runtime Environment (JRE)。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-113">Azul also provides the Java Run-time Environment (JRE) as a stand-alone installation.</span></span> <span data-ttu-id="4c0d2-114">JRE 隨附於 JDK 安裝中。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-114">The JRE is included with the JDK install.</span></span>
>
>  <span data-ttu-id="4c0d2-115">[Azul 的 Azure 下載頁面](https://www.azul.com/downloads/azure-only/zulu/)也會提供 Java 11 套件。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-115">Java 11 packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-for-windows"></a><span data-ttu-id="4c0d2-116">下載並安裝適用於 Windows 的 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="4c0d2-116">Download and install the Azul Zulu JDKs for Windows</span></span> 

1. <span data-ttu-id="4c0d2-117">[將 64 位元 Azul Zulu JDK 8 以 MSI 的形式下載](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi)到您用戶端上的位置，例如 `C:\Users\<your_login>\Downloads`。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-117">[Download the 64-bit Azul Zulu JDK 8 as an MSI](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi) to a location on your client, such as `C:\Users\<your_login>\Downloads`.</span></span> <span data-ttu-id="4c0d2-118">(Azul 的 Azure 下載頁面[也會提供 .ZIP 套件](https://www.azul.com/downloads/azure-only/zulu/)。)</span><span class="sxs-lookup"><span data-stu-id="4c0d2-118">(.ZIP packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="4c0d2-119">瀏覽至該目錄，然後按兩下下載的 MSI 檔案，以開始安裝。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-119">Navigate to the directory and double-click the downloaded MSI file to begin installation.</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-for-mac"></a><span data-ttu-id="4c0d2-120">下載並安裝適用於 Mac 的 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="4c0d2-120">Download and install the Azul Zulu JDKs for Mac</span></span> 

<span data-ttu-id="4c0d2-121">下列步驟會將 ZIP 檔案下載到您的 Mac。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-121">These steps download a ZIP file to your Mac.</span></span> <span data-ttu-id="4c0d2-122">另外也有 DMG 版本可供使用。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-122">There is also a DMG version available.</span></span>

1. <span data-ttu-id="4c0d2-123">[將 64 位元 Azul Zulu JDK 8 以 ZIP 檔案的形式下載](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip)到您用戶端上的位置，例如 `/Library/Java/JavaVirtualMachines/`。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-123">[Download the 64-bit Azul Zulu JDK 8 as a ZIP file](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip) to a location on your client, such as `/Library/Java/JavaVirtualMachines/`.</span></span> <span data-ttu-id="4c0d2-124">(Azul 的 Azure 下載頁面[也會提供 .DMG 套件](https://www.azul.com/downloads/azure-only/zulu/)。)</span><span class="sxs-lookup"><span data-stu-id="4c0d2-124">(.DMG packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="4c0d2-125">啟動搜尋工具、瀏覽至下載目錄中，然後按兩下 ZIP 檔案。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-125">Launch Finder, navigate to the download directory, and double-click the ZIP file.</span></span> <span data-ttu-id="4c0d2-126">或者，您可以啟動終端機命令視窗，然後瀏覽至該目錄並執行：</span><span class="sxs-lookup"><span data-stu-id="4c0d2-126">Alternatively, you can launch a terminal command window, navigate to the directory, and run:</span></span>

```cli
unzip <name_of_zulu_package>.zip
```

## <a name="download-and-install-the-azul-zulu-jdks-for-alpine-linux"></a><span data-ttu-id="4c0d2-127">下載並安裝適用於 Alpine Linux 的 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="4c0d2-127">Download and install the Azul Zulu JDKs for Alpine Linux</span></span>

1. <span data-ttu-id="4c0d2-128">[將 64 位元 Azul Zulu JDK 8 以 TAR 檔案的形式下載](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz)到您用戶端上的位置，例如 `/usr/lib/jvm`。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-128">[Download the 64-bit Azul Zulu JDK 8 as a TAR file](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz) to a location on your client, such as `/usr/lib/jvm`.</span></span> <span data-ttu-id="4c0d2-129">(Azul 的 Azure 下載頁面[也會提供 .RPM 和 .DEB 套件](https://www.azul.com/downloads/azure-only/zulu/)。)</span><span class="sxs-lookup"><span data-stu-id="4c0d2-129">(.RPM and .DEB packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="4c0d2-130">移至您的目錄，然後執行下列命令以解壓縮檔案，並將其展開：</span><span class="sxs-lookup"><span data-stu-id="4c0d2-130">Go to your directory and run the following command to unzip and expand the file:</span></span>

    ```cli
    tar -xvf <name_of_zulu_package>.tar
    ```

## <a name="confirm-your-installation"></a><span data-ttu-id="4c0d2-131">確認安裝</span><span class="sxs-lookup"><span data-stu-id="4c0d2-131">Confirm your installation</span></span>

<span data-ttu-id="4c0d2-132">若要確認您的安裝，請移至命令列並執行 `java -version`。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-132">To confirm your installation, go to the command-line and run `java -version`.</span></span>

<span data-ttu-id="4c0d2-133">命令的輸出應為：</span><span class="sxs-lookup"><span data-stu-id="4c0d2-133">The output of the command should be:</span></span>

```cli
$ java -version

openjdk version "1.8.0_212"
OpenJDK Runtime Environment (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 1.8.0_212-b04)
OpenJDK 64-Bit Server VM (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 25.212-b04, mixed mode)

```

## <a name="download-and-install-the-azul-zulu-jdks-from-a-yum-repository"></a><span data-ttu-id="4c0d2-134">從 Yum 存放庫下載並安裝 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="4c0d2-134">Download and install the Azul Zulu JDKs from a Yum repository</span></span>

<span data-ttu-id="4c0d2-135">Azul 會將 Azul Zulu JDK 提供於 [Yum 存放庫](http://repos.azul.com/azure-only/zulu-azure.repo)中。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-135">The Azul Zulu JDKs are provided in a [Yum repository](http://repos.azul.com/azure-only/zulu-azure.repo) by Azul.</span></span>

<span data-ttu-id="4c0d2-136">**若要安裝適用於 Java 8 的 Azul Zulu JDK，請從您的 CLI 執行下列命令：**</span><span class="sxs-lookup"><span data-stu-id="4c0d2-136">**To install the Azul Zulu JDK for Java 8, run the following commands from your CLI:**</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-8-azure-jdk
```

<span data-ttu-id="4c0d2-137">針對 Java 11，請執行：</span><span class="sxs-lookup"><span data-stu-id="4c0d2-137">For Java 11, run:</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-11-azure-jdk
```

<span data-ttu-id="4c0d2-138">針對 Java 12 (預覽)，請執行：</span><span class="sxs-lookup"><span data-stu-id="4c0d2-138">For Java 12 (Preview), run:</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-12-azure-jdk
```

<span data-ttu-id="4c0d2-139">**若要從 Yum 存放庫更新 Zulu JDK 8 套件：**</span><span class="sxs-lookup"><span data-stu-id="4c0d2-139">**To update a Zulu JDK 8 package from a Yum repository:**</span></span>

```cli
sudo yum -q -y install zulu-8-azure-jdk
```

<span data-ttu-id="4c0d2-140">(如果您使用 11 或 12 版，請變更上述命令中的版本號碼。)</span><span class="sxs-lookup"><span data-stu-id="4c0d2-140">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="4c0d2-141">**若要從 Yum 存放庫中移除 Zulu JDK 8 套件：**</span><span class="sxs-lookup"><span data-stu-id="4c0d2-141">**To remove a Zulu JDK 8 package from a Yum repository:**</span></span>

```cli
sudo yum -y erase zulu-8-azure-jdk
```
<span data-ttu-id="4c0d2-142">(如果您使用 11 或 12 版，請變更上述命令中的版本號碼。)</span><span class="sxs-lookup"><span data-stu-id="4c0d2-142">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository"></a><span data-ttu-id="4c0d2-143">從 apt-get 存放庫下載並安裝 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="4c0d2-143">Download and install the Azul Zulu JDKs from an apt-get repository</span></span>

<span data-ttu-id="4c0d2-144">Azul 也會將 Azul Zulu JDK 提供於 [apt-get 存放庫](http://repos.azul.com/azure-only/zulu/apt)中。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-144">The Azul Zulu JDKs are also provided in an [apt-get repository](http://repos.azul.com/azure-only/zulu/apt) by Azul.</span></span>

<span data-ttu-id="4c0d2-145">**若要透過 apt-get 安裝適用於 Java 8 的 Azul Zulu JDK，請從您的 CLI 執行下列命令：**</span><span class="sxs-lookup"><span data-stu-id="4c0d2-145">**To install the Azul Zulu JDK for Java 8 with apt-get, run the following commands from your CLI:**</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

<span data-ttu-id="4c0d2-146">針對 Java 11，請執行：</span><span class="sxs-lookup"><span data-stu-id="4c0d2-146">For Java 11, run:</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-11-azure-jdk
```

<span data-ttu-id="4c0d2-147">針對 Java 12 (預覽)，請執行：</span><span class="sxs-lookup"><span data-stu-id="4c0d2-147">For Java 12 (Preview), run:</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-12-azure-jdk
```

<span data-ttu-id="4c0d2-148">**若要從 apt-get 存放庫更新 Zulu JDK 8 套件：**</span><span class="sxs-lookup"><span data-stu-id="4c0d2-148">**To update a Zulu JDK 8 package from an apt-get repository:**</span></span>

```cli
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

<span data-ttu-id="4c0d2-149">舊有版本會自動移除。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-149">The previous release will be automatically removed.</span></span>
<span data-ttu-id="4c0d2-150">(如果您使用 11 或 12 版，請變更上述命令中的版本號碼。)</span><span class="sxs-lookup"><span data-stu-id="4c0d2-150">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="4c0d2-151">**若要從 apt-get 存放庫中移除 Zulu JDK 8 套件：**</span><span class="sxs-lookup"><span data-stu-id="4c0d2-151">**To remove a Zulu JDK 8 package from an apt-get repository:**</span></span>

```cli
sudo apt-get -y purge zulu-8-azure-jdk
```

<span data-ttu-id="4c0d2-152">(如果您使用 11 或 12 版，請變更上述命令中的版本號碼。)</span><span class="sxs-lookup"><span data-stu-id="4c0d2-152">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="4c0d2-153">如需準備、安裝和管理 Azul Zulu JDK 以進行 Azure 開發的詳細指引，請參閱[官方 Zulu 文件](https://docs.azul.com/zulu/zuludocs/index.htm)。</span><span class="sxs-lookup"><span data-stu-id="4c0d2-153">For more detailed guidance on preparing, installing, and managing your Azul Zulu JDKs for Azure development, read [the official Zulu docs](https://docs.azul.com/zulu/zuludocs/index.htm).</span></span>

