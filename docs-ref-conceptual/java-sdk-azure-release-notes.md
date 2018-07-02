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
# <a name="release-notes"></a><span data-ttu-id="13097-104">版本資訊</span><span class="sxs-lookup"><span data-stu-id="13097-104">Release Notes</span></span> 

## <a name="october-5-2017---130"></a><span data-ttu-id="13097-105">2017 年 10 月 5 日 - 1.3.0</span><span class="sxs-lookup"><span data-stu-id="13097-105">October 5, 2017 - 1.3.0</span></span> 

<span data-ttu-id="13097-106">版本 1.3.0 向下相容於舊版的服務及功能，可達到與舊版相同的可用性 (穩定性)。</span><span class="sxs-lookup"><span data-stu-id="13097-106">Version 1.3.0 is backwards compatible with previous versions for services and features use that reached the general availability (stable) stage in previous releases.</span></span>

<span data-ttu-id="13097-107">所有針對舊版服務所進行的重大更新都會以 @Beta 標註。</span><span class="sxs-lookup"><span data-stu-id="13097-107">Any breaking changes from Preview versions for those services are marked with the @Beta annotation.</span></span>

<span data-ttu-id="13097-108">如果您要將程式碼移轉至 1.3.0，可以使用[這些附註](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md)來讓現有程式碼做好移轉至版本 1.3 的準備。</span><span class="sxs-lookup"><span data-stu-id="13097-108">If you are migrating your code to 1.3.0, you can use [these notes](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md) to prepare your existing code for the 1.3 version.</span></span>

### <a name="generally-availabile-in-v13"></a><span data-ttu-id="13097-109">在 V1.3 中正式運作</span><span class="sxs-lookup"><span data-stu-id="13097-109">Generally availabile in V1.3</span></span>

<span data-ttu-id="13097-110">某些在舊版中仍屬搶鮮版 (Beta) 的 API 現已在 GA 中正式運作，尤其是：</span><span class="sxs-lookup"><span data-stu-id="13097-110">Some of the APIs that were still in Beta in previous releases are now GA, in particular:</span></span>

- <span data-ttu-id="13097-111">非同步方法</span><span class="sxs-lookup"><span data-stu-id="13097-111">async methods</span></span>
- <span data-ttu-id="13097-112">CDN 中先前屬於搶鮮版的所有方法</span><span class="sxs-lookup"><span data-stu-id="13097-112">all methods in CDN that were previously in Beta</span></span>
- <span data-ttu-id="13097-113">應用程式閘道中先前屬於搶鮮版的所有方法和介面</span><span class="sxs-lookup"><span data-stu-id="13097-113">all methods and interfaces in Application Gateways that were previously in Beta</span></span>

  <span data-ttu-id="13097-114">程式庫的某些部分仍處於預覽階段。</span><span class="sxs-lookup"><span data-stu-id="13097-114">Some parts of the library are still in Preview.</span></span> <span data-ttu-id="13097-115">請參閱下表以了解程式庫的目前狀態：</span><span class="sxs-lookup"><span data-stu-id="13097-115">Refer to the table below for the current state of the libraries:</span></span>

<span data-ttu-id="13097-116">服務或功能</span><span class="sxs-lookup"><span data-stu-id="13097-116">Service or feature</span></span> | <span data-ttu-id="13097-117">以正式運作版來提供</span><span class="sxs-lookup"><span data-stu-id="13097-117">Available as GA</span></span> | <span data-ttu-id="13097-118">以預覽版來提供</span><span class="sxs-lookup"><span data-stu-id="13097-118">Available as Preview</span></span> 
---------|---------|---------|-
<span data-ttu-id="13097-119">計算</span><span class="sxs-lookup"><span data-stu-id="13097-119">Compute</span></span>  | <span data-ttu-id="13097-120">虛擬機器和 VM 擴充功能、虛擬機器擴展集、受控磁碟</span><span class="sxs-lookup"><span data-stu-id="13097-120">Virtual machines and VM extensions, Virtual machine scale sets, managed disks</span></span>   | <span data-ttu-id="13097-121">Azure Container Service、Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="13097-121">Azure container service, Azure container registry</span></span> 
<span data-ttu-id="13097-122">儲存體</span><span class="sxs-lookup"><span data-stu-id="13097-122">Storage</span></span>   |  <span data-ttu-id="13097-123">儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="13097-123">Storage accounts</span></span>       |    <span data-ttu-id="13097-124">加密</span><span class="sxs-lookup"><span data-stu-id="13097-124">Encryption</span></span>     
<span data-ttu-id="13097-125">SQL Database</span><span class="sxs-lookup"><span data-stu-id="13097-125">SQL Database</span></span>  | <span data-ttu-id="13097-126">資料庫、防火牆、彈性集區</span><span class="sxs-lookup"><span data-stu-id="13097-126">Databases, firewalls, elastic pools</span></span>              
<span data-ttu-id="13097-127">網路功能</span><span class="sxs-lookup"><span data-stu-id="13097-127">Networking</span></span>    |  <span data-ttu-id="13097-128">虛擬網路、網路介面、IP 位址、路由表、網路安全性群組、DNS、流量管理員、應用程式閘道</span><span class="sxs-lookup"><span data-stu-id="13097-128">Virtual networks , network interfaces , IP addresses ,routing tables, network security groups , DNS, Traffic managers, Application gateways</span></span>  |    <span data-ttu-id="13097-129">負載平衡器、對等互連、虛擬網路閘道、網路監看員</span><span class="sxs-lookup"><span data-stu-id="13097-129">Load balancers, Network peering, Virtual Network Gateway, Network watchers</span></span> 
<span data-ttu-id="13097-130">更多服務</span><span class="sxs-lookup"><span data-stu-id="13097-130">More services</span></span>    |  <span data-ttu-id="13097-131">Resource Manager、Key Vault、Redis、CDN、Batch</span><span class="sxs-lookup"><span data-stu-id="13097-131">Resource Manager, Key Vault, Redis,  CDN, Batch</span></span>       |  <span data-ttu-id="13097-132">Web 應用程式、函式應用程式、服務匯流排、Graph RBAC、Cosmos DB、搜尋服務</span><span class="sxs-lookup"><span data-stu-id="13097-132">Web apps, Function apps, Service Bus, Graph RBAC, Cosmos DB, Search</span></span>  
<span data-ttu-id="13097-133">基礎</span><span class="sxs-lookup"><span data-stu-id="13097-133">Fundamentals</span></span>     |   <span data-ttu-id="13097-134">驗證 - 核心、Async 方法、受控服務識別</span><span class="sxs-lookup"><span data-stu-id="13097-134">Authentication - core , Async methods , Managed Service Identity</span></span>      |      |

> <span data-ttu-id="13097-135">預覽功能在會程式庫的類別、介面或方法層級中以 `@Beta` 標註。</span><span class="sxs-lookup"><span data-stu-id="13097-135">Preview features are marked with a `@Beta` annotation at the class or interface or method level in libraries.</span></span> <span data-ttu-id="13097-136">這些功能可能會有所變更。</span><span class="sxs-lookup"><span data-stu-id="13097-136">These features are subject to change.</span></span> <span data-ttu-id="13097-137">未來可能會以任何方式修改，或甚至移除。</span><span class="sxs-lookup"><span data-stu-id="13097-137">They can be modified in any way, or even removed, in the future.</span></span>

### <a name="import-with-maven"></a><span data-ttu-id="13097-138">使用 Maven 來匯入</span><span class="sxs-lookup"><span data-stu-id="13097-138">Import with Maven</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a><span data-ttu-id="13097-139">獲得協助及提供意見</span><span class="sxs-lookup"><span data-stu-id="13097-139">Get help and give feedback</span></span>

<span data-ttu-id="13097-140">查看 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) 社群，以獲得在自己的程式碼中使用程式庫的說明。</span><span class="sxs-lookup"><span data-stu-id="13097-140">Check out the [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) community for help using the libraries in your own code.</span></span> <span data-ttu-id="13097-141">如果您遇到任何錯誤，或是有可改善這些程式庫的建議，請透過 [GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 提出問題。</span><span class="sxs-lookup"><span data-stu-id="13097-141">If you encounter any bugs or have suggestions to improve these libraries, please file issues via [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span></span>


