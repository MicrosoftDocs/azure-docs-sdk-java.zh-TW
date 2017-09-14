---
title: "適用於 Java 的 Azure 管理程式庫版本資訊 | Microsoft Docs"
description: "了解適用於 Java 之 Azure 管理程式庫的最新消息和重大變更"
keywords: "Azure, Java, API, 參考, 附註, 更新, 取代"
author: routlaw
manager: douge
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: c0d5c4b3702d3bee4e93de51cec36e72aeaf598f
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2017
---
# <a name="release-notes"></a>版本資訊 

## <a name="june-30-2017---110"></a>2017 年 6 月 30 日 - 1.1.0 

V1.1 可與 V1.0 中已達到正式運作 (穩定) 階段、且目的是要提供給大眾使用的 API 回溯相容。

V.0 中標記了 @Beta 註釋的 API 已導入一些重大變更

如果您要將程式碼移轉至 1.1.0，您可以使用[這些附註](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)來讓程式碼做好從 1.0.0 移轉至 1.1.0 的準備。

### <a name="generally-availabile-in-v11"></a>在 V1.1 中正式運作

某些在 V1.0 中仍屬搶鮮版的 API 現已在 V1.1 中正式運作，尤其是：

- 非同步方法
- CDN 中先前屬於搶鮮版的所有方法
- 應用程式閘道中先前屬於搶鮮版的所有方法和介面

 程式庫的某些部分仍處於預覽階段。 請參閱下表以了解程式庫的目前狀態：

服務或功能 | 以正式運作版來提供 | 以預覽版來提供  | 敬請期待 |
---------|---------|---------|---------|
計算  | 虛擬機器和 VM 擴充功能、虛擬機器擴展集、受控磁碟   | Azure Container Service、Azure Container Registry |    |
儲存體   |  儲存體帳戶       |         |   加密      |
SQL Database  | 資料庫、防火牆、彈性集區        |         |   更多功能      |
網路    |  虛擬網路、網路介面、IP 位址、路由表、網路安全性群組、DNS、流量管理員、應用程式閘道  |    負載平衡器     |   VPN、網路監看員   |
更多服務    |  Resource Manager、Key Vault、Redis、CDN、Batch       |  Web 應用程式、函式應用程式、服務匯流排、Graph RBAC、DocumentDB   | 監視、排程器、函式管理、搜尋、更多 Graph RBAC 功能        |
基礎     |   驗證 - 核心、非同步方法       |      |         |

### <a name="import-with-maven"></a>使用 Maven 來匯入

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.2.1</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a>獲得協助及提供意見

查看 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) 社群，以獲得在自己的程式碼中使用程式庫的說明。 如果您遇到任何錯誤，或是有可改善這些程式庫的建議，請透過 [GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 提出問題。

### <a name="migrate-from-previous-releases"></a>從先前版本移轉

[從 1.0.0-beta5 移轉](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)  [從 1.1.0 移轉](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)


