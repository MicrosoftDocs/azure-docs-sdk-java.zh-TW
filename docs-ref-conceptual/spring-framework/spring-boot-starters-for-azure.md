---
title: 適用於 Azure 的 Spring Boot Starter
description: 本文說明適用於 Azure 的各種 Spring Boot Starter。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 69c0381313994796af31d5301ceadb9f6f40dcb5
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991552"
---
# <a name="spring-boot-starters-for-azure"></a><span data-ttu-id="819c9-103">適用於 Azure 的 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="819c9-103">Spring Boot Starters for Azure</span></span>

<span data-ttu-id="819c9-104">本文說明 [Spring Initializr] 的各種 Spring Boot Starter，這些 Spring Boot Starter 可為 Java 開發人員提供整合功能來與 Microsoft Azure 搭配運作。</span><span class="sxs-lookup"><span data-stu-id="819c9-104">This article describes the various Spring Boot Starters for the [Spring Initializr] that provide Java developers with integration features for working with Microsoft Azure.</span></span>

![Azure Spring Boot Starter][spring-boot-starters]

<span data-ttu-id="819c9-106">下列是目前適用於 Azure 的 Spring Boot Starter：</span><span class="sxs-lookup"><span data-stu-id="819c9-106">The following Spring Boot Starters are currently available for Azure:</span></span>

* <span data-ttu-id="819c9-107">**[Azure 支援](#azure-support)**</span><span class="sxs-lookup"><span data-stu-id="819c9-107">**[Azure Support](#azure-support)**</span></span>

   <span data-ttu-id="819c9-108">為 Azure 服務 (例如，服務匯流排、儲存體、Active Directory 等) 提供自動設定支援。</span><span class="sxs-lookup"><span data-stu-id="819c9-108">Provides auto-configuration support for Azure Services; e.g. Service Bus, Storage, Active Directory, etc.</span></span>

* <span data-ttu-id="819c9-109">**[Azure Active Directory](#azure-active-directory)**</span><span class="sxs-lookup"><span data-stu-id="819c9-109">**[Azure Active Directory](#azure-active-directory)**</span></span>

   <span data-ttu-id="819c9-110">提供整合支援，讓 Spring Security 可與 Azure Active Directory 整合以獲得驗證功能。</span><span class="sxs-lookup"><span data-stu-id="819c9-110">Provides integration support for Spring Security with Azure Active Directory for authentication.</span></span>

* <span data-ttu-id="819c9-111">**[Azure Key Vault](#azure-key-vault)**</span><span class="sxs-lookup"><span data-stu-id="819c9-111">**[Azure Key Vault](#azure-key-vault)**</span></span>

   <span data-ttu-id="819c9-112">提供 Spring 值註釋支援以便與 Azure Key Vault 祕密整合。</span><span class="sxs-lookup"><span data-stu-id="819c9-112">Provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

* <span data-ttu-id="819c9-113">**[Azure 儲存體](#azure-storage)**</span><span class="sxs-lookup"><span data-stu-id="819c9-113">**[Azure Storage](#azure-storage)**</span></span>

   <span data-ttu-id="819c9-114">為 Azure 儲存體服務提供 Spring Boot 支援。</span><span class="sxs-lookup"><span data-stu-id="819c9-114">Provides Spring Boot support for Azure Storage services.</span></span>

<a name="azure-support"></a>
## <a name="azure-support"></a><span data-ttu-id="819c9-115">Azure 支援</span><span class="sxs-lookup"><span data-stu-id="819c9-115">Azure Support</span></span>

<span data-ttu-id="819c9-116">此 Spring Boot Starter 可為 Azure 服務 (例如，服務匯流排、儲存體、Active Directory、Cosmos DB、金鑰保存庫等) 提供自動設定支援。</span><span class="sxs-lookup"><span data-stu-id="819c9-116">This Spring Boot Starter provides auto-configuration support for Azure Services; for example: Service Bus, Storage, Active Directory, Cosmos DB, Key Vault, etc.</span></span>

<span data-ttu-id="819c9-117">如需如何使用此 Starter 所提供之各種 Azure 功能的範例，請參閱下列內容：</span><span class="sxs-lookup"><span data-stu-id="819c9-117">For examples of how to use the various Azure features that are provided by this starter, see the following:</span></span>

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples>

<span data-ttu-id="819c9-118">當您對 Spring Boot 專案新增此 Starter 時，pom.xml 檔案會發生下列變更：</span><span class="sxs-lookup"><span data-stu-id="819c9-118">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="819c9-119">下列屬性會新增至 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="819c9-119">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="819c9-120">預設的 `spring-boot-starter` 相依性會由下列相依性取代：</span><span class="sxs-lookup"><span data-stu-id="819c9-120">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* <span data-ttu-id="819c9-121">檔案中會新增下列區段：</span><span class="sxs-lookup"><span data-stu-id="819c9-121">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-active-directory"></a>
## <a name="azure-active-directory"></a><span data-ttu-id="819c9-122">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="819c9-122">Azure Active Directory</span></span>

<span data-ttu-id="819c9-123">此 Spring Boot Starter 可為 Spring Security 提供自動設定支援，以便與 Azure Active Directory 進行整合以提供驗證功能。</span><span class="sxs-lookup"><span data-stu-id="819c9-123">This Spring Boot Starter provides auto-configuration support for Spring Security in order to provide integration with Azure Active Directory for authentication.</span></span>

<span data-ttu-id="819c9-124">如需如何使用此 Starter 所提供之 Azure Active Directory 功能的範例，請參閱下列內容：</span><span class="sxs-lookup"><span data-stu-id="819c9-124">For examples of how to use the Azure Active Directory features that are provided by this starter, see the following:</span></span>

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample>

<span data-ttu-id="819c9-125">當您對 Spring Boot 專案新增此 Starter 時，pom.xml 檔案會發生下列變更：</span><span class="sxs-lookup"><span data-stu-id="819c9-125">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="819c9-126">下列屬性會新增至 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="819c9-126">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="819c9-127">預設的 `spring-boot-starter` 相依性會由下列相依性取代：</span><span class="sxs-lookup"><span data-stu-id="819c9-127">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="819c9-128">檔案中會新增下列區段：</span><span class="sxs-lookup"><span data-stu-id="819c9-128">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-key-vault"></a>
## <a name="azure-key-vault"></a><span data-ttu-id="819c9-129">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="819c9-129">Azure Key Vault</span></span>

<span data-ttu-id="819c9-130">此 Spring Boot Starter 提供 Spring 值註釋支援以便與 Azure Key Vault 祕密整合。</span><span class="sxs-lookup"><span data-stu-id="819c9-130">This Spring Boot Starter provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

<span data-ttu-id="819c9-131">如需如何使用此 Starter 所提供之 Azure Key Vault 功能的範例，請參閱下列內容：</span><span class="sxs-lookup"><span data-stu-id="819c9-131">For examples of how to use the Azure Key Vault features that are provided by this starter, see the following:</span></span>

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample>

<span data-ttu-id="819c9-132">當您對 Spring Boot 專案新增此 Starter 時，pom.xml 檔案會發生下列變更：</span><span class="sxs-lookup"><span data-stu-id="819c9-132">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="819c9-133">下列屬性會新增至 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="819c9-133">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="819c9-134">預設的 `spring-boot-starter` 相依性會由下列相依性取代：</span><span class="sxs-lookup"><span data-stu-id="819c9-134">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="819c9-135">檔案中會新增下列區段：</span><span class="sxs-lookup"><span data-stu-id="819c9-135">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-storage"></a>
## <a name="azure-storage"></a><span data-ttu-id="819c9-136">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="819c9-136">Azure Storage</span></span>

<span data-ttu-id="819c9-137">此 Spring Boot Starter 可為 Azure 儲存體服務提供 Spring Boot 整合支援。</span><span class="sxs-lookup"><span data-stu-id="819c9-137">This Spring Boot Starter provides Spring Boot integration support for Azure Storage services.</span></span>

<span data-ttu-id="819c9-138">如需如何使用此 Starter 所提供之 Azure 儲存體功能的範例，請參閱下列內容：</span><span class="sxs-lookup"><span data-stu-id="819c9-138">For examples of how to use the Azure Storage features that are provided by this starter, see the following:</span></span>

* [<span data-ttu-id="819c9-139">如何對 Azure 儲存體使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="819c9-139">How to use the Spring Boot Starter for Azure Storage</span></span>](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample>

<span data-ttu-id="819c9-140">當您對 Spring Boot 專案新增此 Starter 時，pom.xml 檔案會發生下列變更：</span><span class="sxs-lookup"><span data-stu-id="819c9-140">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="819c9-141">下列屬性會新增至 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="819c9-141">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="819c9-142">預設的 `spring-boot-starter` 相依性會由下列相依性取代：</span><span class="sxs-lookup"><span data-stu-id="819c9-142">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="819c9-143">檔案中會新增下列區段：</span><span class="sxs-lookup"><span data-stu-id="819c9-143">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

## <a name="next-steps"></a><span data-ttu-id="819c9-144">後續步驟</span><span class="sxs-lookup"><span data-stu-id="819c9-144">Next steps</span></span>

<span data-ttu-id="819c9-145">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="819c9-145">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="819c9-146">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="819c9-146">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="819c9-147">其他資源</span><span class="sxs-lookup"><span data-stu-id="819c9-147">Additional Resources</span></span>

<span data-ttu-id="819c9-148">如需在 Azure 上使用 [Spring Boot] 應用程式的詳細資訊，請參閱 [Azure 上的 Spring]。</span><span class="sxs-lookup"><span data-stu-id="819c9-148">For more information about using [Spring Boot] applications on Azure, see [Spring on Azure].</span></span>

<span data-ttu-id="819c9-149">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="819c9-149">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="819c9-150">如需開始使用您自己的 Spring Boot 應用程式的說明，請參閱 **Spring Initializr**，網址為 https://start.spring.io/。</span><span class="sxs-lookup"><span data-stu-id="819c9-150">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<!-- URL List -->

[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Azure 上的 Spring]: /java/azure/spring-framework/
[Spring on Azure]: /java/azure/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
