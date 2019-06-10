---
title: 搭配使用 Docker 映像與適用於 Azure Java 開發的 JDK
description: ''
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: ee8df2a08b23d090965cb42e2c15b934d4785e7c
ms.sourcegitcommit: 03379369346974c6e80f86e7129b885112b5c1a9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2019
ms.locfileid: "64910234"
---
# <a name="use-docker-with-a-jdk-for-azure"></a><span data-ttu-id="d0edd-102">搭配使用 Docker 與適用於 Azure 的 JDK</span><span class="sxs-lookup"><span data-stu-id="d0edd-102">Use Docker with a JDK for Azure</span></span> 

<span data-ttu-id="d0edd-103">針對 Java 7、8 和 11 預先建置的 Docker 映像可透過 [Docker Hub](https://hub.docker.com/_/microsoft-java-se) 取得。</span><span class="sxs-lookup"><span data-stu-id="d0edd-103">Pre-built Docker images for Java 7, 8, and 11 are available through [Docker Hub](https://hub.docker.com/_/microsoft-java-se).</span></span>

<span data-ttu-id="d0edd-104">多基底 OS 映像上已針對 Zulu JDK、JRE 和無周邊 JRE 通過認證的 Docker 容器映像，可從 Docker Hub 取得：</span><span class="sxs-lookup"><span data-stu-id="d0edd-104">Certified Docker container images for Zulu JDK, JRE, and JRE-headless on multiple base OS images are available at Docker Hub:</span></span>

* [<span data-ttu-id="d0edd-105">JDK</span><span class="sxs-lookup"><span data-stu-id="d0edd-105">JDK</span></span>](https://hub.docker.com/_/microsoft-java-jdk)
* [<span data-ttu-id="d0edd-106">JRE</span><span class="sxs-lookup"><span data-stu-id="d0edd-106">JRE</span></span>](https://hub.docker.com/_/microsoft-java-jre)
* [<span data-ttu-id="d0edd-107">無周邊 JRE</span><span class="sxs-lookup"><span data-stu-id="d0edd-107">JRE-headless</span></span>](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a><span data-ttu-id="d0edd-108">執行 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="d0edd-108">Running a Docker image</span></span>

<span data-ttu-id="d0edd-109">Docker 映像可以使用 `$ docker run mcr.microsoft.com/java/jdk:tag java` 語法來執行，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="d0edd-109">Docker images can be run using the syntax `$ docker run mcr.microsoft.com/java/jdk:tag java` as shown in the following example.</span></span>

```cli
docker run mcr.microsoft.com/java/jdk:8u212-zulu-alpine java -version 
```

## <a name="creating-a-docker-image"></a><span data-ttu-id="d0edd-110">建立 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="d0edd-110">Creating a Docker image</span></span>

<span data-ttu-id="d0edd-111">您可以使用 Microsoft 的官方 Docker Hub 映像來建立映像，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="d0edd-111">You can create an image using Microsoft's official Docker Hub images as shown in the following examples.</span></span>

### <a name="create-a-docker-file"></a><span data-ttu-id="d0edd-112">建立 Docker 檔案</span><span class="sxs-lookup"><span data-stu-id="d0edd-112">Create a Docker file</span></span>

```cli
FROM mcr.microsoft.com/java/jdk:8u212-zulu-alpine 
  
RUN echo $' \
  
public class HelloWorld { \
   public static void main(String[] args) { \
      // Prints "Hello, World" in the terminal window. \
      System.out.println("Hello, World - From Microsoft Azure !!!"); \
   } \
}' > HelloWorld.java
  
RUN javac HelloWorld.java
  
CMD ["java", "HelloWorld"]
```

### <a name="build-a-docker-image"></a><span data-ttu-id="d0edd-113">建置 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="d0edd-113">Build a Docker image</span></span>

```cli
docker build -t hello-world
```

### <a name="run-the-new-image"></a><span data-ttu-id="d0edd-114">執行新的映像</span><span class="sxs-lookup"><span data-stu-id="d0edd-114">Run the new image</span></span>

```cli
docker run hello-world
```

<span data-ttu-id="d0edd-115">您會看到下列輸出︰</span><span class="sxs-lookup"><span data-stu-id="d0edd-115">You will see the following output:</span></span>

```output
Hello World - From Microsoft Azure !!!
```
