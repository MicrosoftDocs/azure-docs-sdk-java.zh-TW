---
title: "如何對 Azure Active Directory 使用 Spring Boot Starter"
description: "了解如何使用 Azure Active Directory Starter 來設定 Spring Boot Initializer 應用程式。"
services: active-directory
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: robmcm
ms.openlocfilehash: a999e33674ea01e776db10186e8af83ce157ef20
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a>如何對 Azure Active Directory 使用 Spring Boot Starter

## <a name="overview"></a>概觀

本文示範如何使用 **[Spring Initializr]** (適用於 Azure Active Directory (Azure AD) 的 Spring Boot Starter) 建立應用程式。

## <a name="prerequisites"></a>必要條件

若要遵循本文中的步驟，需要具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* [Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>使用 Spring Initializr 建立自訂應用程式

1. 瀏覽至 <https://start.spring.io/>。

1. 指定您想要使用 [Java] 產生 [Maven 專案]、輸入應用程式的 [群組] 和 [成品] 名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。

   ![指定群組和成品名稱][security-01]

1. 向下捲動至 [核心] 區段，並核取 [安全性] 的方塊，然後在 [Web] 區段中核取 [Web] 的方塊。

   ![選取安全性和 Web Starter][security-02]

1. 向下捲動至 [Azure] 區段，然後核取 [Azure Active Directory] 的方塊。

   ![選取 Azure Active Directory Starter][security-03]

1. 捲動到頁面底部，然後按一下按鈕以**產生專案**。

   ![產生 Spring Boot 專案][security-04]

1. 出現提示時，將專案下載至本機電腦上的路徑。

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a>建立和設定新的 Azure Active Directory 執行個體

### <a name="create-the-active-directory-instance"></a>建立 Active Directory 執行個體

1. 登入 <https://portal.azure.com>。

1. 依序按一下 [+新增]、[安全性 + 身分識別] 和 [Azure Active Directory]。

   ![建立新的 Azure Active Directory 執行個體][directory-01]

1. 輸入您的 [組織名稱] 和您的 [初始網域名稱]，然後按一下 [建立]。

   ![指定 Azure Active Directory 名稱][directory-02]

1. 從 Azure 入口網站頂端工具列上的下拉式功能表，選取您的新 Azure Active Directory。

   ![選擇您的 Azure Active Directory][directory-03]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>為 Spring Boot 應用程式新增應用程式註冊

1. 從入口網站的功能表選取 [Azure Active Directory]，按一下 [概觀]，然後按一下 [應用程式註冊]。

   ![新增應用程式註冊][directory-04]

1. 按一下 [新增應用程式註冊]，指定您的應用程式 [名稱]，使用 http://localhost:8080 作為 [登入 URL]，然後按一下 [建立].

   ![建立新的應用程式註冊][directory-05]

1. 應用程式註冊建立好之後，請對它按一下。

   ![選取您的應用程式註冊][directory-06]

1. 當應用程式註冊的頁面出現時，複製您的 [應用程式識別碼] 以供稍後使用，然後按一下 [金鑰]。

   ![建立應用程式註冊金鑰][directory-07]

1. 新增 [說明]，並指定新金鑰的 [持續時間]，然後按一下 [儲存]；當您按一下 [儲存] 圖示時，系統就會自動填入金鑰的值，然後您必須複製金鑰值以供稍後使用。 (之後您就無法擷取此值)。

   ![指定應用程式註冊金鑰參數][directory-08]

1. 從應用程式註冊的主要頁面，按一下 [所需的權限]。

   ![應用程式註冊所需的權限][directory-09]

1. 按一下 [Windows Azure Active Directory]。

   ![選取 [Windows Azure Active Directory]][directory-10]

1. 核取 [以登入使用者身分存取目錄] 和 [登入及讀取使用者個人檔案] 的方塊，然後按一下 [儲存]。

   ![啟用存取權限][directory-11]

1. 在 [所需的權限] 頁面上，按一下 [授與權限]，然後在出現提示時按一下 [是]。

   ![授與存取權限][directory-12]

## <a name="configure-and-compile-your-spring-boot-application"></a>設定及編譯 Spring Boot 應用程式

1. 從下載的專案封存檔將檔案解壓縮到某個目錄。

1. 瀏覽至專案中的父資料夾，然後在文字編輯器中開啟 pom.xml 檔案。

1. 新增 Spring OAuth2 安全性的相依性；例如：

   ```xml
   <dependency>
      <groupId>org.springframework.security.oauth</groupId>
      <artifactId>spring-security-oauth2</artifactId>
   </dependency>
   ```

1. 儲存並關閉 pom.xml 檔案。

1. 瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。

1. 使用稍早取得的值新增儲存體帳戶的金鑰；例如：

   ```yaml
   # Specifies your Active Directory Application ID:
   azure.activedirectory.clientId=11111111-1111-1111-1111-1111111111111111

   # Specifies your secret key:
   azure.activedirectory.clientSecret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authentication:
   azure.activedirectory.activeDirectoryGroups=Users
   ```
   其中：
   參數 | 說明
   ---|---|---
   `azure.activedirectory.clientId` | 包含您稍早取得的**應用程式識別碼**。
   `azure.activedirectory.clientSecret` | 包含您稍早完成之應用程式註冊所得到的金鑰值。
   `azure.activedirectory.activeDirectoryGroups` | 包含要用於驗證的 Active Directory 群組清單。


1. 儲存並關閉 application.properties 檔案。

1. 在應用程式的 Java 來源資料夾中，建立名為 controller 的資料夾；例如：src/main/java/com/wingtiptoys/security/controller。

1. 在 controller 資料夾中建立名為 HelloController.java 的新 Java 檔案，然後在文字編輯器中加以開啟。

1. 輸入下列程式碼，然後儲存並關閉檔案：

   ```java
   package com.wingtiptoys.security;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.web.authentication.preauth.PreAuthenticatedAuthenticationToken;
   
   @RestController
   public class HelloController {
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String hello() {
         return "Hello World!";
      }
   }
   ```

1. 在應用程式的 Java 來源資料夾中，建立名為 security 的資料夾；例如：src/main/java/com/wingtiptoys/security/security。

1. 在 security 資料夾中建立名為 WebSecurityConfig.java 的新 Java 檔案，然後在文字編輯器中加以開啟。

1. 輸入下列程式碼，然後儲存並關閉檔案：

   ```java
   package com.wingtiptoys.security;

   import com.microsoft.azure.spring.autoconfigure.aad.AADAuthenticationFilter;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.autoconfigure.security.oauth2.client.EnableOAuth2Sso;
   import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
   import org.springframework.security.config.annotation.web.builders.HttpSecurity;
   import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
   import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
   import org.springframework.security.web.csrf.CookieCsrfTokenRepository;
   
   @EnableOAuth2Sso
   @EnableGlobalMethodSecurity(securedEnabled = true, prePostEnabled = true)
   
   public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
      @Autowired
      private AADAuthenticationFilter aadAuthFilter;
      @Override
      protected void configure(HttpSecurity http) throws Exception {
         http.authorizeRequests().anyRequest().permitAll();
         http.addFilterBefore(aadAuthFilter, UsernamePasswordAuthenticationFilter.class);
      }
   }
   ```

## <a name="build-and-test-your-app"></a>建置及測試您的應用程式

1. 開啟命令提示字元，並將目錄切換到應用程式的 pom.xml 檔案所在的資料夾。

1. 使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：

   ```shell
   mvn clean package
   ```

   ![][build-application]

1. 使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```



1. 在 Maven 建置並啟動您的應用程式之後，請在網頁瀏覽器中開啟 <http://localhost:8080>。

## <a name="next-steps"></a>後續步驟

如需使用 Azure Active Directory 的詳細資訊，請參閱下列文章：

* [Azure Active Directory 文件]。

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式](deploy-spring-boot-java-app-on-kubernetes.md)

如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。

**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。 [Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。 為了協助開發人員開始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。 除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。

<!-- URL List -->

[Azure Active Directory 文件]: /azure/active-directory/
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[security-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-01.png
[security-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-02.png
[security-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-03.png
[security-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-04.png

[directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-01.png
[directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-02.png
[directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-03.png
[directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-04.png
[directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-05.png
[directory-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-06.png
[directory-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-07.png
[directory-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-08.png
[directory-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-09.png
[directory-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-10.png
[directory-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-11.png
[directory-12]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-12.png

[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
