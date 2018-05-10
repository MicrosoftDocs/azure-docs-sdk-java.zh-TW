---
title: 如何對 Azure Active Directory 使用 Spring Boot Starter
description: 了解如何使用 Azure Active Directory Starter 來設定 Spring Boot Initializer 應用程式。
services: active-directory
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: cf1cad0b87626058f7204a6565d09fb8901b7ce4
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="8a4ea-103">如何對 Azure Active Directory 使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="8a4ea-103">How to use the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="8a4ea-104">概觀</span><span class="sxs-lookup"><span data-stu-id="8a4ea-104">Overview</span></span>

<span data-ttu-id="8a4ea-105">本文示範如何使用 **[Spring Initializr]** (適用於 Azure Active Directory (Azure AD) 的 Spring Boot Starter) 建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a4ea-106">先決條件</span><span class="sxs-lookup"><span data-stu-id="8a4ea-106">Prerequisites</span></span>

<span data-ttu-id="8a4ea-107">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="8a4ea-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="8a4ea-109">[Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="8a4ea-110">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="8a4ea-111">使用 Spring Initializr 建立自訂應用程式</span><span class="sxs-lookup"><span data-stu-id="8a4ea-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="8a4ea-112">瀏覽至 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="8a4ea-113">指定您想要使用 [Java] 產生 [Maven 專案]、輸入應用程式的 [群組] 和 [成品] 名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定群組和成品名稱][security-01]

1. <span data-ttu-id="8a4ea-115">向下捲動至 [核心] 區段，並核取 [安全性] 的方塊，然後在 [Web] 區段中核取 [Web] 的方塊。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-115">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![選取安全性和 Web Starter][security-02]

1. <span data-ttu-id="8a4ea-117">向下捲動至 [Azure] 區段，然後核取 [Azure Active Directory] 的方塊。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-117">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![選取 Azure Active Directory Starter][security-03]

1. <span data-ttu-id="8a4ea-119">捲動到頁面底部，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![產生 Spring Boot 專案][security-04]

1. <span data-ttu-id="8a4ea-121">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-121">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="8a4ea-122">建立和設定新的 Azure Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="8a4ea-122">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="8a4ea-123">建立 Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="8a4ea-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="8a4ea-124">登入 <https://portal.azure.com>。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-124">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="8a4ea-125">依序按一下 [+新增]、[安全性 + 身分識別] 和 [Azure Active Directory]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-125">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![建立新的 Azure Active Directory 執行個體][directory-01]

1. <span data-ttu-id="8a4ea-127">輸入您的 [組織名稱] 和您的 [初始網域名稱]，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-127">Enter your **Organization name** and your **Initial domain name**, and then click **Create**.</span></span>

   ![指定 Azure Active Directory 名稱][directory-02]

1. <span data-ttu-id="8a4ea-129">從 Azure 入口網站頂端工具列上的下拉式功能表，選取您的新 Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-129">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![選擇您的 Azure Active Directory][directory-03]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="8a4ea-131">為 Spring Boot 應用程式新增應用程式註冊</span><span class="sxs-lookup"><span data-stu-id="8a4ea-131">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="8a4ea-132">從入口網站的功能表選取 [Azure Active Directory]，按一下 [概觀]，然後按一下 [應用程式註冊]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-132">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![新增應用程式註冊][directory-04]

1. <span data-ttu-id="8a4ea-134">按一下 [新增應用程式註冊]，指定您的應用程式 [名稱]，使用 http://localhost:8080 作為 [登入 URL]，然後按一下 [建立].</span><span class="sxs-lookup"><span data-stu-id="8a4ea-134">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![建立新的應用程式註冊][directory-05]

1. <span data-ttu-id="8a4ea-136">應用程式註冊建立好之後，請對它按一下。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-136">Click your application registration after it has been created.</span></span>

   ![選取您的應用程式註冊][directory-06]

1. <span data-ttu-id="8a4ea-138">當應用程式註冊的頁面出現時，複製您的 [應用程式識別碼] 以供稍後使用，然後按一下 [金鑰]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-138">When the page for your app registration, copy your **Application ID** for later, then click **Keys**.</span></span>

   ![建立應用程式註冊金鑰][directory-07]

1. <span data-ttu-id="8a4ea-140">新增 [說明]，並指定新金鑰的 [持續時間]，然後按一下 [儲存]；當您按一下 [儲存] 圖示時，系統就會自動填入金鑰的值，然後您必須複製金鑰值以供稍後使用。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-140">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key for later.</span></span> <span data-ttu-id="8a4ea-141">(之後您就無法擷取此值)。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-141">(You will not be able to retrieve this value later.)</span></span>

   ![指定應用程式註冊金鑰參數][directory-08]

1. <span data-ttu-id="8a4ea-143">從應用程式註冊的主要頁面，按一下 [所需的權限]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-143">From the main page for your app registration, click **Required permissions**.</span></span>

   ![應用程式註冊所需的權限][directory-09]

1. <span data-ttu-id="8a4ea-145">按一下 [Windows Azure Active Directory]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-145">Click **Windows Azure Active Directory**.</span></span>

   ![選取 [Windows Azure Active Directory]][directory-10]

1. <span data-ttu-id="8a4ea-147">核取 [以登入使用者身分存取目錄] 和 [登入及讀取使用者個人檔案] 的方塊，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-147">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![啟用存取權限][directory-11]

1. <span data-ttu-id="8a4ea-149">在 [所需的權限] 頁面上，按一下 [授與權限]，然後在出現提示時按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-149">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![授與存取權限][directory-12]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="8a4ea-151">設定及編譯 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="8a4ea-151">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="8a4ea-152">從下載的專案封存檔將檔案解壓縮到某個目錄。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-152">Extract the files from the downloaded project archive into a directory.</span></span>

1. <span data-ttu-id="8a4ea-153">瀏覽至專案中的父資料夾，然後在文字編輯器中開啟 pom.xml 檔案。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-153">Navigate to the parent folder in your project and open the *pom.xml* file in a text editor.</span></span>

1. <span data-ttu-id="8a4ea-154">新增 Spring OAuth2 安全性的相依性；例如：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-154">Add the dependency for Spring OAuth2 security; for example:</span></span>

   ```xml
   <dependency>
      <groupId>org.springframework.security.oauth</groupId>
      <artifactId>spring-security-oauth2</artifactId>
   </dependency>
   ```

1. <span data-ttu-id="8a4ea-155">儲存並關閉 pom.xml 檔案。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-155">Save and close the  the *pom.xml* file.</span></span>

1. <span data-ttu-id="8a4ea-156">瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-156">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="8a4ea-157">使用稍早取得的值新增儲存體帳戶的金鑰；例如：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-157">Add the key for your storage account using the values from earlier; for example:</span></span>

   ```yaml
   # Specifies your Active Directory Application ID:
   azure.activedirectory.clientId=11111111-1111-1111-1111-1111111111111111

   # Specifies your secret key:
   azure.activedirectory.clientSecret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authentication:
   azure.activedirectory.activeDirectoryGroups=Users
   ```
   <span data-ttu-id="8a4ea-158">其中：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-158">Where:</span></span>
   | <span data-ttu-id="8a4ea-159">參數</span><span class="sxs-lookup"><span data-stu-id="8a4ea-159">Parameter</span></span> | <span data-ttu-id="8a4ea-160">說明</span><span class="sxs-lookup"><span data-stu-id="8a4ea-160">Description</span></span> |
   |---|---|
   | `azure.activedirectory.clientId` | <span data-ttu-id="8a4ea-161">包含您稍早取得的**應用程式識別碼**。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-161">Contains your **Application ID** from earlier.</span></span> |
   | `azure.activedirectory.clientSecret` | <span data-ttu-id="8a4ea-162">包含您稍早完成之應用程式註冊所得到的金鑰值。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-162">Contains the key value from your app registration which you completed earlier.</span></span> |
   | `azure.activedirectory.activeDirectoryGroups` | <span data-ttu-id="8a4ea-163">包含要用於驗證的 Active Directory 群組清單。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-163">Contains a list of Active Directory groups to use for authentication.</span></span> |


1. <span data-ttu-id="8a4ea-164">儲存並關閉 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-164">Save and close the  the *application.properties* file.</span></span>

1. <span data-ttu-id="8a4ea-165">在應用程式的 Java 來源資料夾中，建立名為 controller 的資料夾；例如：src/main/java/com/wingtiptoys/security/controller。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-165">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="8a4ea-166">在 controller 資料夾中建立名為 HelloController.java 的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-166">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="8a4ea-167">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-167">Enter the following code, then save and close the file:</span></span>

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

1. <span data-ttu-id="8a4ea-168">在應用程式的 Java 來源資料夾中，建立名為 security 的資料夾；例如：src/main/java/com/wingtiptoys/security/security。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-168">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="8a4ea-169">在 security 資料夾中建立名為 WebSecurityConfig.java 的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-169">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="8a4ea-170">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-170">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="8a4ea-171">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="8a4ea-171">Build and test your app</span></span>

1. <span data-ttu-id="8a4ea-172">開啟命令提示字元，並將目錄切換到應用程式的 pom.xml 檔案所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-172">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="8a4ea-173">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-173">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   ```

   ![建置應用程式][build-application]

1. <span data-ttu-id="8a4ea-175">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-175">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="8a4ea-176">在 Maven 建置並啟動您的應用程式之後，請在網頁瀏覽器中開啟 <http://localhost:8080>。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-176">After your application is built and started by Maven, open <http://localhost:8080> in a web browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a4ea-177">後續步驟</span><span class="sxs-lookup"><span data-stu-id="8a4ea-177">Next steps</span></span>

<span data-ttu-id="8a4ea-178">如需使用 Azure Active Directory 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-178">For more information about using Azure Active Directory, see the following articles:</span></span>

* <span data-ttu-id="8a4ea-179">[Azure Active Directory 文件]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-179">[Azure Active Directory Documentation].</span></span>

<span data-ttu-id="8a4ea-180">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="8a4ea-180">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="8a4ea-181">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8a4ea-181">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="8a4ea-182">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="8a4ea-182">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="8a4ea-183">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-183">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="8a4ea-184">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-184">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="8a4ea-185">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-185">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="8a4ea-186">為了協助開發人員開始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-186">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="8a4ea-187">除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a4ea-187">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Active Directory 文件]: /azure/active-directory/
[Azure Active Directory Documentation]: /azure/active-directory/
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
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
