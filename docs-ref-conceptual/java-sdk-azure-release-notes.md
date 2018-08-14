---
title: 適用於 Java 的 Azure 管理程式庫版本資訊 | Microsoft Docs
description: 了解適用於 Java 之 Azure 管理程式庫的最新消息和重大變更
keywords: Azure, Java, API, 參考, 附註, 更新, 取代
author: routlaw
manager: douge
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: 0aaa83ceb42192441decb5972baae56fed337fb2
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090681"
---
# <a name="release-notes"></a>版本資訊 

## <a name="october-5-2017---130"></a>2017 年 10 月 5 日 - 1.3.0 

版本 1.3.0 向下相容於舊版的服務及功能，可達到與舊版相同的可用性 (穩定性)。

所有針對舊版服務所進行的重大更新都會以 @Beta 標註。

如果您要將程式碼移轉至 1.3.0，可以使用[這些附註](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md)來讓現有程式碼做好移轉至版本 1.3 的準備。

### <a name="generally-availabile-in-v13"></a>在 V1.3 中正式運作

某些在舊版中仍屬搶鮮版 (Beta) 的 API 現已在 GA 中正式運作，尤其是：

- 非同步方法
- CDN 中先前屬於搶鮮版的所有方法
- 應用程式閘道中先前屬於搶鮮版的所有方法和介面

  程式庫的某些部分仍處於預覽階段。 請參閱下表以了解程式庫的目前狀態：

服務或功能 | 以正式運作版來提供 | 以預覽版來提供 
---------|---------|---------|-
計算  | 虛擬機器和 VM 擴充功能、虛擬機器擴展集、受控磁碟   | Azure Container Service、Azure Container Registry 
儲存體   |  儲存體帳戶       |    加密     
SQL Database  | 資料庫、防火牆、彈性集區              
網路功能    |  虛擬網路、網路介面、IP 位址、路由表、網路安全性群組、DNS、流量管理員、應用程式閘道  |    負載平衡器、對等互連、虛擬網路閘道、網路監看員 
更多服務    |  Resource Manager、Key Vault、Redis、CDN、Batch       |  Web 應用程式、函式應用程式、服務匯流排、Graph RBAC、Cosmos DB、搜尋服務  
基礎     |   驗證 - 核心、Async 方法、受控服務識別      |      |

> 預覽功能在會程式庫的類別、介面或方法層級中以 `@Beta` 標註。 這些功能可能會有所變更。 未來可能會以任何方式修改，或甚至移除。

### <a name="import-with-maven"></a>使用 Maven 來匯入

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a>獲得協助及提供意見

查看 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) 社群，以獲得在自己的程式碼中使用程式庫的說明。 如果您遇到任何錯誤，或是有可改善這些程式庫的建議，請透過 [GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 提出問題。


