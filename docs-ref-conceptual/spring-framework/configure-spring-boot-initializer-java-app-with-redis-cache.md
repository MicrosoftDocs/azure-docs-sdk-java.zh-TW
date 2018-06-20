---
title: 將 Spring Boot Initializer 應用程式設定為使用 Azure Redis 快取
description: 將使用 Spring Initializr 所建立的 Spring Boot 應用程式設定為透過 Azure Redis Cache 在雲端使用 Redis。
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm;zhijzhao;yidon
ms.date: 02/01/2018
ms.devlang: java
ms.service: cache
ms.tgt_pltfrm: cache-redis
ms.topic: article
ms.workload: na
ms.openlocfilehash: 8bfe7c2ddd238e0e5a259de9078b831a97b1b1a4
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954579"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-redis-in-the-cloud-with-azure-redis-cache"></a><span data-ttu-id="59ed4-103">將 Spring Boot Initializer 應用程式設定為透過 Azure Redis Cache 在雲端使用 Redis</span><span class="sxs-lookup"><span data-stu-id="59ed4-103">Configure a Spring Boot Initializer app to use Redis in the cloud with Azure Redis Cache</span></span>

<span data-ttu-id="59ed4-104">本文將引導您使用 Azure 入口網站在雲端建立 Redis 快取、使用 **[Spring Initializr]** 建立自訂應用程式，及建立 Java Web 應用程式以使用 Redis 快取來儲存和擷取資料。</span><span class="sxs-lookup"><span data-stu-id="59ed4-104">This article walks you through creating a Redis cache in the cloud using the Azure portal, then using the **[Spring Initializr]** to create a custom application, and then creating a Java web application that stores and retrieves data using your Redis cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59ed4-105">先決條件</span><span class="sxs-lookup"><span data-stu-id="59ed4-105">Prerequisites</span></span>

<span data-ttu-id="59ed4-106">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="59ed4-106">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="59ed4-107">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="59ed4-108">[Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="59ed4-108">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="59ed4-109">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="59ed4-109">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="59ed4-110">使用 Spring Initializr 建立自訂應用程式</span><span class="sxs-lookup"><span data-stu-id="59ed4-110">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="59ed4-111">瀏覽至 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="59ed4-111">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="59ed4-112">指定您想要使用 [Java] 產生 [Maven 專案]、輸入應用程式的 [群組] 和 [成品] 名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。</span><span class="sxs-lookup"><span data-stu-id="59ed4-112">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="59ed4-114">Spring Initializr 會使用 [群組] 和 [成品] 名稱來建立套件名稱；例如：com.contoso.myazuredemo。</span><span class="sxs-lookup"><span data-stu-id="59ed4-114">The Spring Initializr will use the **Group** and **Aritifact** names to create the package name; for example: *com.contoso.myazuredemo*.</span></span>
   >

1. <span data-ttu-id="59ed4-115">向下捲動至 [Web] 區段，並核取 [Web] 方塊，然後向下捲動至 [NoSQL] 區段，並核取 [Redis] 方塊，然後捲動至頁面底部，並按一下按鈕以 [產生專案]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-115">Scroll down to the **Web** section and check the box for **Web**, then scroll down to the **NoSQL** section and check the box for **Redis**, then scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Spring Initializr 的完整選項][SI02]

1. <span data-ttu-id="59ed4-117">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="59ed4-117">When prompted, download the project to a path on your local computer.</span></span>

   ![下載自訂的 Spring Boot 專案][SI03]

1. <span data-ttu-id="59ed4-119">當您在本機系統上擷取檔案之後，就可以開始編輯自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="59ed4-119">After you have extracted the files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![自訂的 Spring Boot 專案檔][SI04]

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="59ed4-121">在 Azure 上建立 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="59ed4-121">Create a Redis cache on Azure</span></span>

1. <span data-ttu-id="59ed4-122">瀏覽至 <https://portal.azure.com/> 上的 Azure 入口網站，然後按一下 [+新增]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-122">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure 入口網站][AZ01]

1. <span data-ttu-id="59ed4-124">按一下 [資料庫]，然後按一下 [Redis 快取]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-124">Click **Database**, and then click **Redis Cache**.</span></span>

   ![Azure 入口網站][AZ02]

1. <span data-ttu-id="59ed4-126">在 [新的 Redis 快取] 頁面上，指定下列資訊：</span><span class="sxs-lookup"><span data-stu-id="59ed4-126">On the **New Redis Cache** page, specify the following information:</span></span>

   * <span data-ttu-id="59ed4-127">為快取輸入 [DNS 名稱]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-127">Enter the **DNS name** for your cache.</span></span>
   * <span data-ttu-id="59ed4-128">指定 [訂閱]、[資源群組]、[位置] 和 [定價層]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-128">Specify your **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span>
   * <span data-ttu-id="59ed4-129">針對本教學課程的案例，我們選擇 [解除封鎖連接埠 6379]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-129">For this tutorial, choose **Unblock port 6379**.</span></span>

   > [!NOTE]
   >
   > <span data-ttu-id="59ed4-130">您可以透過 Redis 快取使用 SSL，但需要透過 Jedis 等其他的 Redis 用戶端。</span><span class="sxs-lookup"><span data-stu-id="59ed4-130">You can use SSL with Redis caches, but you would need to use a different Redis client like Jedis.</span></span> <span data-ttu-id="59ed4-131">如需詳細資訊，請參閱[如何搭配使用 Azure Redis 快取與 Java][Redis Cache with Java]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-131">For more information, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span>
   >

   <span data-ttu-id="59ed4-132">當您指定這些選項之後，按一下 [建立] 以建立您的快取。</span><span class="sxs-lookup"><span data-stu-id="59ed4-132">When you have specified these options, click **Create** to create your cache.</span></span>

   ![Azure 入口網站][AZ03]

1. <span data-ttu-id="59ed4-134">在您的快取完成之後，您會看到它列在您的 Azure [儀表板] 上，以及 [所有資源] 和 [Redis 快取] 頁面下。</span><span class="sxs-lookup"><span data-stu-id="59ed4-134">Once your cache has been completed, you will see it listed on your Azure **Dashboard**, as well as under the **All Resources**, and **Redis Caches** pages.</span></span> <span data-ttu-id="59ed4-135">您可以按一下這些任何位置上的快取，來開啟快取的屬性頁面。</span><span class="sxs-lookup"><span data-stu-id="59ed4-135">You can click on your cache on any of those locations to open the properties page for your cache.</span></span>

   ![Azure 入口網站][AZ04]

1. <span data-ttu-id="59ed4-137">顯示列出快取屬性清單的頁面時，請按一下 [存取金鑰]，並複製快取的存取金鑰。</span><span class="sxs-lookup"><span data-stu-id="59ed4-137">When the page that contains the list of properties for your cache is displayed, click **Access keys** and copy your access keys for your cache.</span></span>

   ![Azure 入口網站][AZ05]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a><span data-ttu-id="59ed4-139">將自訂 Spring Boot 設定為使用 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="59ed4-139">Configure your custom Spring Boot to use your Redis Cache</span></span>

1. <span data-ttu-id="59ed4-140">在應用程式的 [資源] 目錄中尋找 application.properties 檔案，如果該檔案不存在，則加以建立。</span><span class="sxs-lookup"><span data-stu-id="59ed4-140">Locate the *application.properties* file in the *resources* directory of your app, or create the file if it does not already exist.</span></span>

   ![尋找 application.properties 檔案][RE01]

1. <span data-ttu-id="59ed4-142">在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用快取中的適當屬性來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="59ed4-142">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties from your cache:</span></span>

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![編輯 application.properties 檔案][RE02]

   > [!NOTE] 
   > 
   > <span data-ttu-id="59ed4-144">如果您使用 Jedis 等具備 SSL 的其他 Redis 用戶端，可以在 application.properties 檔案中使用 SSL 並使用連接埠 6380。</span><span class="sxs-lookup"><span data-stu-id="59ed4-144">If you were using a different Redis client like Jedis that enables SSL, you would specify that you want to use SSL in your *application.properties* file and use port 6380.</span></span> <span data-ttu-id="59ed4-145">例如︰</span><span class="sxs-lookup"><span data-stu-id="59ed4-145">For example:</span></span>
   > 
   > ```yaml
   > # Specify the DNS URI of your Redis cache.
   > spring.redis.host=myspringbootcache.redis.cache.windows.net
   > # Specify the access key for your Redis cache.
   > spring.redis.password=57686f6120447564652c2049495320526f636b73=
   > # Specify that you want to use SSL.
   > spring.redis.ssl=true
   > # Specify the SSL port for your Redis cache.
   > spring.redis.port=6380
   > ```
   > 
   > <span data-ttu-id="59ed4-146">如需詳細資訊，請參閱[如何搭配使用 Azure Redis 快取與 Java][Redis Cache with Java]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-146">For more information, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span> 
   > 

1. <span data-ttu-id="59ed4-147">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="59ed4-147">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="59ed4-148">在套件的來源資料夾下建立名為 controller 的資料夾，例如：</span><span class="sxs-lookup"><span data-stu-id="59ed4-148">Create a folder named *controller* under the source folder for your package; for example:</span></span>

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   <span data-ttu-id="59ed4-149">-或-</span><span class="sxs-lookup"><span data-stu-id="59ed4-149">-or-</span></span>

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. <span data-ttu-id="59ed4-150">在 *controller* 資料夾中建立名稱為 *HelloController.java* 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="59ed4-150">Create a new file named *HelloController.java* in the *controller* folder.</span></span> <span data-ttu-id="59ed4-151">在文字編輯器中開啟檔案，並加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="59ed4-151">Open the file in a text editor and add the following code to it:</span></span>

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   <span data-ttu-id="59ed4-152">其中，您必須使用專案的套件名稱來取代 `com.contoso.myazuredemo`。</span><span class="sxs-lookup"><span data-stu-id="59ed4-152">Where you will need to replace `com.contoso.myazuredemo` with the package name for your project.</span></span>

1. <span data-ttu-id="59ed4-153">儲存並關閉 HelloController.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="59ed4-153">Save and close the *HelloController.java* file.</span></span>

1. <span data-ttu-id="59ed4-154">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="59ed4-154">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="59ed4-155">使用網頁瀏覽器瀏覽至 http://localhost:8080 來測試 Web 應用程式；如果有可用的 curl，請使用類似下列範例的語法：</span><span class="sxs-lookup"><span data-stu-id="59ed4-155">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="59ed4-156">您應該會從顯示的範例控制器中看到 "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="59ed4-156">You should see the "Hello World!"</span></span> <span data-ttu-id="59ed4-157">訊息，系統將會從您的 Redis 快取動態擷取此訊息。</span><span class="sxs-lookup"><span data-stu-id="59ed4-157">message from your sample controller displayed, which is being retrieved dynamically from your Redis cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59ed4-158">後續步驟</span><span class="sxs-lookup"><span data-stu-id="59ed4-158">Next steps</span></span>

<span data-ttu-id="59ed4-159">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="59ed4-159">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="59ed4-160">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="59ed4-160">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="59ed4-161">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="59ed4-161">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="59ed4-162">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-162">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="59ed4-163">如需有關在 Azure 上開始搭配使用 Redis 快取與 Java 的詳細資訊，請參閱[如何搭配使用 Azure Redis 快取與 Java][Redis Cache with Java]。</span><span class="sxs-lookup"><span data-stu-id="59ed4-163">For more information about getting started using Redis Cache with Java on Azure, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span>

<span data-ttu-id="59ed4-164">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="59ed4-164">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="59ed4-165">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="59ed4-165">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="59ed4-166">為了協助開發人員開始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="59ed4-166">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="59ed4-167">除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="59ed4-167">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Redis Cache with Java]: /azure/redis-cache/cache-java-get-started

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ01.png
[AZ02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ02.png
[AZ03]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ03.png
[AZ04]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ04.png
[AZ05]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ05.png

[SI01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI01.png
[SI02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI02.png
[SI03]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI03.png
[SI04]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI04.png

[RE01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/RE01.png
[RE02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/RE02.png
