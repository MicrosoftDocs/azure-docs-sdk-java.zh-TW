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
# <a name="install-the-jdk-for-azure-and-azure-stack"></a>安裝適用於 Azure 和 Azure Stack 的 JDK

OpenJDK 的 Azul Zulu Enterprise 組建是免費、多平台、可實際執行的 OpenJDK 散發套件，適用於 Azure 和 Azure Stack，由 Microsoft 與 Azul Systems 提供支援。 其中包含建置及執行 Java SE 應用程式所需的所有元件。

目前有[多種下載套件分別支援各類用戶端作業系統](https://www.azul.com/downloads/azure-only/zulu/)。 您也可以從 Azure Marketplace 資源庫取得適用於下列平台的虛擬機器映像：

  * [Azul Zulu：Ubuntu 18.04 上的 Java 8](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-ubuntu-1804)
  * [Azul Zulu：Windows Server 2019 上的 Java 8](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-windows-2019)
  
  * [Azul Zulu：Ubuntu 18.04 上的 Java 11](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-ubuntu-1804)
  * [Azul Zulu：Windows Server 2019 上的 Java 11](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-windows-2019)


> [!NOTE]
> 以下指示以 64 位元 Java 8 版的 JDK 為目標。 Azul 也提供獨立安裝形式的 Java Runtime Environment (JRE)。 JRE 隨附於 JDK 安裝中。
>
>  [Azul 的 Azure 下載頁面](https://www.azul.com/downloads/azure-only/zulu/)也會提供 Java 11 套件。

## <a name="download-and-install-the-azul-zulu-jdks-for-windows"></a>下載並安裝適用於 Windows 的 Azul Zulu JDK 

1. [將 64 位元 Azul Zulu JDK 8 以 MSI 的形式下載](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi)到您用戶端上的位置，例如 `C:\Users\<your_login>\Downloads`。 (Azul 的 Azure 下載頁面[也會提供 .ZIP 套件](https://www.azul.com/downloads/azure-only/zulu/)。)

2. 瀏覽至該目錄，然後按兩下下載的 MSI 檔案，以開始安裝。

## <a name="download-and-install-the-azul-zulu-jdks-for-mac"></a>下載並安裝適用於 Mac 的 Azul Zulu JDK 

下列步驟會將 ZIP 檔案下載到您的 Mac。 另外也有 DMG 版本可供使用。

1. [將 64 位元 Azul Zulu JDK 8 以 ZIP 檔案的形式下載](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip)到您用戶端上的位置，例如 `/Library/Java/JavaVirtualMachines/`。 (Azul 的 Azure 下載頁面[也會提供 .DMG 套件](https://www.azul.com/downloads/azure-only/zulu/)。)

2. 啟動搜尋工具、瀏覽至下載目錄中，然後按兩下 ZIP 檔案。 或者，您可以啟動終端機命令視窗，然後瀏覽至該目錄並執行：

```cli
unzip <name_of_zulu_package>.zip
```

## <a name="download-and-install-the-azul-zulu-jdks-for-alpine-linux"></a>下載並安裝適用於 Alpine Linux 的 Azul Zulu JDK

1. [將 64 位元 Azul Zulu JDK 8 以 TAR 檔案的形式下載](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz)到您用戶端上的位置，例如 `/usr/lib/jvm`。 (Azul 的 Azure 下載頁面[也會提供 .RPM 和 .DEB 套件](https://www.azul.com/downloads/azure-only/zulu/)。)

2. 移至您的目錄，然後執行下列命令以解壓縮檔案，並將其展開：

    ```cli
    tar -xvf <name_of_zulu_package>.tar
    ```

## <a name="confirm-your-installation"></a>確認安裝

若要確認您的安裝，請移至命令列並執行 `java -version`。

命令的輸出應為：

```cli
$ java -version

openjdk version "1.8.0_212"
OpenJDK Runtime Environment (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 1.8.0_212-b04)
OpenJDK 64-Bit Server VM (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 25.212-b04, mixed mode)

```

## <a name="download-and-install-the-azul-zulu-jdks-from-a-yum-repository"></a>從 Yum 存放庫下載並安裝 Azul Zulu JDK

Azul 會將 Azul Zulu JDK 提供於 [Yum 存放庫](http://repos.azul.com/azure-only/zulu-azure.repo)中。

**若要安裝適用於 Java 8 的 Azul Zulu JDK，請從您的 CLI 執行下列命令：**

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-8-azure-jdk
```

針對 Java 11，請執行：

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-11-azure-jdk
```

針對 Java 12 (預覽)，請執行：

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-12-azure-jdk
```

**若要從 Yum 存放庫更新 Zulu JDK 8 套件：**

```cli
sudo yum -q -y install zulu-8-azure-jdk
```

(如果您使用 11 或 12 版，請變更上述命令中的版本號碼。)

**若要從 Yum 存放庫中移除 Zulu JDK 8 套件：**

```cli
sudo yum -y erase zulu-8-azure-jdk
```
(如果您使用 11 或 12 版，請變更上述命令中的版本號碼。)

## <a name="download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository"></a>從 apt-get 存放庫下載並安裝 Azul Zulu JDK

Azul 也會將 Azul Zulu JDK 提供於 [apt-get 存放庫](http://repos.azul.com/azure-only/zulu/apt)中。

**若要透過 apt-get 安裝適用於 Java 8 的 Azul Zulu JDK，請從您的 CLI 執行下列命令：**

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

針對 Java 11，請執行：

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-11-azure-jdk
```

針對 Java 12 (預覽)，請執行：

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-12-azure-jdk
```

**若要從 apt-get 存放庫更新 Zulu JDK 8 套件：**

```cli
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

舊有版本會自動移除。
(如果您使用 11 或 12 版，請變更上述命令中的版本號碼。)

**若要從 apt-get 存放庫中移除 Zulu JDK 8 套件：**

```cli
sudo apt-get -y purge zulu-8-azure-jdk
```

(如果您使用 11 或 12 版，請變更上述命令中的版本號碼。)

如需準備、安裝和管理 Azul Zulu JDK 以進行 Azure 開發的詳細指引，請參閱[官方 Zulu 文件](https://docs.azul.com/zulu/zuludocs/index.htm)。

