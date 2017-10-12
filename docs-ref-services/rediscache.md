---
title: "適用於 Java 的 Redis 快取程式庫"
description: "適用於 Redis 快取之 Java 用戶端和管理程式庫的參考文件"
keywords: "Azure, Java, SDK, API, 快取, redis, web 快取, key-value, 記憶體內部"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: redis-cache
ms.openlocfilehash: 6d436c49124fd0a406486e0c7bac4d1605de5d32
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2017
---
# <a name="redis-cache-libraries-for-java"></a><span data-ttu-id="eb8a5-104">適用於 Java 的 Redis 快取程式庫</span><span class="sxs-lookup"><span data-stu-id="eb8a5-104">Redis Cache libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="eb8a5-105">概觀</span><span class="sxs-lookup"><span data-stu-id="eb8a5-105">Overview</span></span>

<span data-ttu-id="eb8a5-106">Azure Redis 快取是安全、分散式的金鑰-值存放區，並以熱門的開放原始碼 [Redis](https://redis.io/) 快取作為基礎。</span><span class="sxs-lookup"><span data-stu-id="eb8a5-106">Azure Redis Cache is a secure, distributed key-value store based on the popular open source [Redis](https://redis.io/) cache.</span></span> 

<span data-ttu-id="eb8a5-107">若要開始使用 Azure Redis 快取，請參閱[如何搭配使用 Azure Redis 快取與 Java](/azure/redis-cache/cache-java-get-started)。</span><span class="sxs-lookup"><span data-stu-id="eb8a5-107">To get started with Azure Redis Cache, see [How to use Azure Redis Cache with Java](/azure/redis-cache/cache-java-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="eb8a5-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="eb8a5-108">Client library</span></span>

<span data-ttu-id="eb8a5-109">使用開放原始碼 [Jedis](https://github.com/xetorthio/jedis) 用戶端連線至 Azure Redis 快取並透過快取來儲存和擷取值。</span><span class="sxs-lookup"><span data-stu-id="eb8a5-109">Connect to Azure Redis Cache and store and retrieve values from the cache using the open-source [Jedis](https://github.com/xetorthio/jedis) client.</span></span>  

<span data-ttu-id="eb8a5-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="eb8a5-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
</dependency>
```

## <a name="example"></a><span data-ttu-id="eb8a5-111">範例</span><span class="sxs-lookup"><span data-stu-id="eb8a5-111">Example</span></span>

<span data-ttu-id="eb8a5-112">連線到 Azure Redis，並將字串插入快取中。</span><span class="sxs-lookup"><span data-stu-id="eb8a5-112">Connect to Azure Redis and insert a string into the cache.</span></span>

```java
JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */
    Jedis jedis = new Jedis(shardInfo);
    jedis.set("foo", "bar");
```

## <a name="management-api"></a><span data-ttu-id="eb8a5-113">管理 API</span><span class="sxs-lookup"><span data-stu-id="eb8a5-113">Management API</span></span>

<span data-ttu-id="eb8a5-114">使用管理 API 建立及調整 Azure Redis 資源和管理存取金鑰。</span><span class="sxs-lookup"><span data-stu-id="eb8a5-114">Create and scale Azure Redis resources and manage access keys to with the management API.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-redis</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="eb8a5-115">範例</span><span class="sxs-lookup"><span data-stu-id="eb8a5-115">Example</span></span>

<span data-ttu-id="eb8a5-116">在[雙節點的標準層](https://azure.microsoft.com/services/cache/)中建立新的 Azure Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="eb8a5-116">Create a new Azure Redis Cache in the [two-node standard tier](https://azure.microsoft.com/services/cache/).</span></span> 

```java
RedisCache cache = azure.redisCaches().define(redisCacheName1)
    .withRegion(Region.US_CENTRAL)
    .withNewResourceGroup(rgName)
    .withStandardSku();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="eb8a5-117">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="eb8a5-117">Explore the Management APIs</span></span>](/java/api/overview/azure/rediscache/managementapi)

## <a name="samples"></a><span data-ttu-id="eb8a5-118">範例</span><span class="sxs-lookup"><span data-stu-id="eb8a5-118">Samples</span></span>

[<span data-ttu-id="eb8a5-119">管理 Azure Redis 快取</span><span class="sxs-lookup"><span data-stu-id="eb8a5-119">Manage Azure Redis Cache</span></span>](https://github.com/Azure-Samples/redis-java-manage-cache)   

<span data-ttu-id="eb8a5-120">深入探索可在應用程式中使用的 [Azure Redis 快取 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=redis)。</span><span class="sxs-lookup"><span data-stu-id="eb8a5-120">Explore more [sample Java code for Azure Redis Cache](https://azure.microsoft.com/resources/samples/?platform=java&term=redis) you can use in your apps.</span></span>
