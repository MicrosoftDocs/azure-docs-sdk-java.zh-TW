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
ms.openlocfilehash: 2bba290e16cac638685aa91b435876433fc1ea91
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2017
---
# <a name="redis-cache-libraries-for-java"></a>適用於 Java 的 Redis 快取程式庫

## <a name="overview"></a>概觀

Azure Redis 快取是安全、分散式的金鑰-值存放區，並以熱門的開放原始碼 [Redis](https://redis.io/) 快取作為基礎。 

若要開始使用 Azure Redis 快取，請參閱[如何搭配使用 Azure Redis 快取與 Java](/azure/redis-cache/cache-java-get-started)。

## <a name="client-library"></a>用戶端程式庫

使用開放原始碼 [Jedis](https://github.com/xetorthio/jedis) 用戶端連線至 Azure Redis 快取並透過快取來儲存和擷取值。  

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。   

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
</dependency>
```

## <a name="example"></a>範例

連線到 Azure Redis，並將字串插入快取中。

```java
JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */
    Jedis jedis = new Jedis(shardInfo);
    jedis.set("foo", "bar");
```

## <a name="management-api"></a>管理 API

使用管理 API 建立及調整 Azure Redis 資源和管理存取金鑰。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-redis</artifactId>
    <version>1.2.1</version>
</dependency>
```

## <a name="example"></a>範例

在[雙節點的標準層](https://azure.microsoft.com/services/cache/)中建立新的 Azure Redis 快取。 

```java
RedisCache cache = azure.redisCaches().define(redisCacheName1)
    .withRegion(Region.US_CENTRAL)
    .withNewResourceGroup(rgName)
    .withStandardSku();
```

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/rediscache/managementapi)

## <a name="samples"></a>範例

[管理 Azure Redis 快取](https://github.com/Azure-Samples/redis-java-manage-cache)   

深入探索可在應用程式中使用的 [Azure Redis 快取 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=redis)。
