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
ms.openlocfilehash: fc246d499b2f5f20a7efbaed16c4b4193d8d8e6c
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="release-notes"></a><span data-ttu-id="8cfc1-104">版本資訊</span><span class="sxs-lookup"><span data-stu-id="8cfc1-104">Release Notes</span></span> 

## <a name="june-30-2017---110"></a><span data-ttu-id="8cfc1-105">2017 年 6 月 30 日 - 1.1.0</span><span class="sxs-lookup"><span data-stu-id="8cfc1-105">June 30, 2017 - 1.1.0</span></span> 

<span data-ttu-id="8cfc1-106">V1.1 可與 V1.0 中已達到正式運作 (穩定) 階段、且目的是要提供給大眾使用的 API 回溯相容。</span><span class="sxs-lookup"><span data-stu-id="8cfc1-106">V1.1 is backwards compatible with V1.0 in the APIs intended for public use that reached the general availability (stable) stage in V1.0.</span></span>

<span data-ttu-id="8cfc1-107">V.0 中標記了 @Beta 註釋的 API 已導入一些重大變更</span><span class="sxs-lookup"><span data-stu-id="8cfc1-107">Some breaking changes were introduced in APIs marked with the @Beta annotation in V.0</span></span>

<span data-ttu-id="8cfc1-108">如果您要將程式碼移轉至 1.1.0，您可以使用[這些附註](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)來讓程式碼做好從 1.0.0 移轉至 1.1.0 的準備。</span><span class="sxs-lookup"><span data-stu-id="8cfc1-108">If you are migrating your code to 1.1.0, you can use [these notes](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md) for preparing your code for 1.1.0 from 1.0.0.</span></span>

### <a name="generally-availabile-in-v11"></a><span data-ttu-id="8cfc1-109">在 V1.1 中正式運作</span><span class="sxs-lookup"><span data-stu-id="8cfc1-109">Generally availabile in V1.1</span></span>

<span data-ttu-id="8cfc1-110">某些在 V1.0 中仍屬搶鮮版的 API 現已在 V1.1 中正式運作，尤其是：</span><span class="sxs-lookup"><span data-stu-id="8cfc1-110">Some of the APIs that were still in Beta in V1.0 are now GA in V1.1, in particular:</span></span>

- <span data-ttu-id="8cfc1-111">非同步方法</span><span class="sxs-lookup"><span data-stu-id="8cfc1-111">async methods</span></span>
- <span data-ttu-id="8cfc1-112">CDN 中先前屬於搶鮮版的所有方法</span><span class="sxs-lookup"><span data-stu-id="8cfc1-112">all methods in CDN that were previously in Beta</span></span>
- <span data-ttu-id="8cfc1-113">應用程式閘道中先前屬於搶鮮版的所有方法和介面</span><span class="sxs-lookup"><span data-stu-id="8cfc1-113">all methods and interfaces in Application Gateways that were previously in Beta</span></span>

 <span data-ttu-id="8cfc1-114">程式庫的某些部分仍處於預覽階段。</span><span class="sxs-lookup"><span data-stu-id="8cfc1-114">Some parts of the library are still in Preview.</span></span> <span data-ttu-id="8cfc1-115">請參閱下表以了解程式庫的目前狀態：</span><span class="sxs-lookup"><span data-stu-id="8cfc1-115">Refer to the table below for the current state of the libraries:</span></span>

<span data-ttu-id="8cfc1-116">服務或功能</span><span class="sxs-lookup"><span data-stu-id="8cfc1-116">Service or feature</span></span> | <span data-ttu-id="8cfc1-117">以正式運作版來提供</span><span class="sxs-lookup"><span data-stu-id="8cfc1-117">Available as GA</span></span> | <span data-ttu-id="8cfc1-118">以預覽版來提供</span><span class="sxs-lookup"><span data-stu-id="8cfc1-118">Available as Preview</span></span>  | <span data-ttu-id="8cfc1-119">敬請期待</span><span class="sxs-lookup"><span data-stu-id="8cfc1-119">Coming soon</span></span> |
---------|---------|---------|---------|
<span data-ttu-id="8cfc1-120">計算</span><span class="sxs-lookup"><span data-stu-id="8cfc1-120">Compute</span></span>  | <span data-ttu-id="8cfc1-121">虛擬機器和 VM 擴充功能、虛擬機器擴展集、受控磁碟</span><span class="sxs-lookup"><span data-stu-id="8cfc1-121">Virtual machines and VM extensions, Virtual machine scale sets, managed disks</span></span>   | <span data-ttu-id="8cfc1-122">Azure Container Service、Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="8cfc1-122">Azure container service, Azure container registry</span></span> |    |
<span data-ttu-id="8cfc1-123">儲存體</span><span class="sxs-lookup"><span data-stu-id="8cfc1-123">Storage</span></span>   |  <span data-ttu-id="8cfc1-124">儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="8cfc1-124">Storage accounts</span></span>       |         |   <span data-ttu-id="8cfc1-125">加密</span><span class="sxs-lookup"><span data-stu-id="8cfc1-125">Encryption</span></span>      |
<span data-ttu-id="8cfc1-126">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8cfc1-126">SQL Database</span></span>  | <span data-ttu-id="8cfc1-127">資料庫、防火牆、彈性集區</span><span class="sxs-lookup"><span data-stu-id="8cfc1-127">Databases, firewalls, elastic pools</span></span>        |         |   <span data-ttu-id="8cfc1-128">更多功能</span><span class="sxs-lookup"><span data-stu-id="8cfc1-128">More features</span></span>      |
<span data-ttu-id="8cfc1-129">網路</span><span class="sxs-lookup"><span data-stu-id="8cfc1-129">Networking</span></span>    |  <span data-ttu-id="8cfc1-130">虛擬網路、網路介面、IP 位址、路由表、網路安全性群組、DNS、流量管理員、應用程式閘道</span><span class="sxs-lookup"><span data-stu-id="8cfc1-130">Virtual networks , network interfaces , IP addresses ,routing tables, network security groups , DNS, Traffic managers, Application gateways</span></span>  |    <span data-ttu-id="8cfc1-131">負載平衡器</span><span class="sxs-lookup"><span data-stu-id="8cfc1-131">Load balancers</span></span>     |   <span data-ttu-id="8cfc1-132">VPN、網路監看員</span><span class="sxs-lookup"><span data-stu-id="8cfc1-132">VPN, Network watchers</span></span>   |
<span data-ttu-id="8cfc1-133">更多服務</span><span class="sxs-lookup"><span data-stu-id="8cfc1-133">More services</span></span>    |  <span data-ttu-id="8cfc1-134">Resource Manager、Key Vault、Redis、CDN、Batch</span><span class="sxs-lookup"><span data-stu-id="8cfc1-134">Resource Manager, Key Vault, Redis,  CDN, Batch</span></span>       |  <span data-ttu-id="8cfc1-135">Web 應用程式、函式應用程式、服務匯流排、Graph RBAC、DocumentDB</span><span class="sxs-lookup"><span data-stu-id="8cfc1-135">Web apps, Function apps, Service Bus, Graph RBAC, DocumentDB</span></span>   | <span data-ttu-id="8cfc1-136">監視、排程器、函式管理、搜尋、更多 Graph RBAC 功能</span><span class="sxs-lookup"><span data-stu-id="8cfc1-136">Monitor ,Scheduler, Functions management, Search, more Graph RBAC features</span></span>        |
<span data-ttu-id="8cfc1-137">基礎</span><span class="sxs-lookup"><span data-stu-id="8cfc1-137">Fundamentals</span></span>     |   <span data-ttu-id="8cfc1-138">驗證 - 核心、非同步方法</span><span class="sxs-lookup"><span data-stu-id="8cfc1-138">Authentication - core , Async methods</span></span>       |      |         |

### <a name="import-with-maven"></a><span data-ttu-id="8cfc1-139">使用 Maven 來匯入</span><span class="sxs-lookup"><span data-stu-id="8cfc1-139">Import with Maven</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a><span data-ttu-id="8cfc1-140">獲得協助及提供意見</span><span class="sxs-lookup"><span data-stu-id="8cfc1-140">Get help and give feedback</span></span>

<span data-ttu-id="8cfc1-141">查看 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) 社群，以獲得在自己的程式碼中使用程式庫的說明。</span><span class="sxs-lookup"><span data-stu-id="8cfc1-141">Check out the [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) community for help using the libraries in your own code.</span></span> <span data-ttu-id="8cfc1-142">如果您遇到任何錯誤，或是有可改善這些程式庫的建議，請透過 [GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 提出問題。</span><span class="sxs-lookup"><span data-stu-id="8cfc1-142">If you encounter any bugs or have suggestions to improve these libraries, please file issues via [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span></span>

### <a name="migrate-from-previous-releases"></a><span data-ttu-id="8cfc1-143">從先前版本移轉</span><span class="sxs-lookup"><span data-stu-id="8cfc1-143">Migrate from previous releases</span></span>

[<span data-ttu-id="8cfc1-144">從 1.0.0-beta5 移轉](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)  [從 1.1.0 移轉</span><span class="sxs-lookup"><span data-stu-id="8cfc1-144">Migrate from 1.0.0-beta5](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)  [Migrate from 1.1.0</span></span>](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)


