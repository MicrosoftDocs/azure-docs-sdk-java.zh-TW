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
ms.date: 02/01/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 678d4b279cecb83c95b3bf0f6bcdf1581924aa62
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954439"
---
# <a name="spring-boot-starters-for-azure"></a>適用於 Azure 的 Spring Boot Starter

本文說明 [Spring Initializr] 的各種 Spring Boot Starter，這些 Spring Boot Starter 可為 Java 開發人員提供整合功能來與 Microsoft Azure 搭配運作。

![Azure Spring Boot Starter][spring-boot-starters]

下列是目前適用於 Azure 的 Spring Boot Starter：

* **[Azure 支援](#azure-support)**

   為 Azure 服務 (例如，服務匯流排、儲存體、Active Directory 等) 提供自動設定支援。

* **[Azure Active Directory](#azure-active-directory)**

   提供整合支援，讓 Spring Security 可與 Azure Active Directory 整合以獲得驗證功能。

* **[Azure Key Vault](#azure-key-vault)**

   提供 Spring 值註釋支援以便與 Azure Key Vault 祕密整合。

* **[Azure 儲存體](#azure-storage)**

   為 Azure 儲存體服務提供 Spring Boot 支援。

<a name="azure-support"></a>
## <a name="azure-support"></a>Azure 支援

此 Spring Boot Starter 可為 Azure 服務 (例如，服務匯流排、儲存體、Active Directory、Cosmos DB、金鑰保存庫等) 提供自動設定支援。

如需如何使用此 Starter 所提供之各種 Azure 功能的範例，請參閱下列內容：

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples>

當您對 Spring Boot 專案新增此 Starter 時，pom.xml 檔案會發生下列變更：

* 下列屬性會新增至 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* 預設的 `spring-boot-starter` 相依性會由下列相依性取代：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* 檔案中會新增下列區段：

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
## <a name="azure-active-directory"></a>Azure Active Directory

此 Spring Boot Starter 可為 Spring Security 提供自動設定支援，以便與 Azure Active Directory 進行整合以提供驗證功能。

如需如何使用此 Starter 所提供之 Azure Active Directory 功能的範例，請參閱下列內容：

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample>

當您對 Spring Boot 專案新增此 Starter 時，pom.xml 檔案會發生下列變更：

* 下列屬性會新增至 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* 預設的 `spring-boot-starter` 相依性會由下列相依性取代：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* 檔案中會新增下列區段：

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
## <a name="azure-key-vault"></a>Azure 金鑰保存庫

此 Spring Boot Starter 提供 Spring 值註釋支援以便與 Azure Key Vault 祕密整合。

如需如何使用此 Starter 所提供之 Azure Key Vault 功能的範例，請參閱下列內容：

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample>

當您對 Spring Boot 專案新增此 Starter 時，pom.xml 檔案會發生下列變更：

* 下列屬性會新增至 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* 預設的 `spring-boot-starter` 相依性會由下列相依性取代：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* 檔案中會新增下列區段：

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
## <a name="azure-storage"></a>Azure 儲存體

此 Spring Boot Starter 可為 Azure 儲存體服務提供 Spring Boot 整合支援。

如需如何使用此 Starter 所提供之 Azure 儲存體功能的範例，請參閱下列內容：

* [如何對 Azure 儲存體使用 Spring Boot Starter](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample>

當您對 Spring Boot 專案新增此 Starter 時，pom.xml 檔案會發生下列變更：

* 下列屬性會新增至 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* 預設的 `spring-boot-starter` 相依性會由下列相依性取代：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* 檔案中會新增下列區段：

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

## <a name="next-steps"></a>後續步驟

如需在 Azure 上使用 [Spring Boot] 應用程式的詳細資訊，請參閱 [Azure 上的 Spring]。

如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。

如需開始使用您自己的 Spring Boot 應用程式的說明，請參閱 **Spring Initializr** (網址為 https://start.spring.io/)。

<!-- URL List -->

[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Azure 上的 Spring]: https://docs.microsoft.com/java/azure/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
