---
title: 如何對 Azure Active Directory 使用 Spring Boot Starter
description: 了解如何使用 Azure Active Directory Starter 來設定 Spring Boot Initializer 應用程式。
services: active-directory
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 06/20/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: adcbc78cc129daf589bf070741308e4024432e5d
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090831"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="f46c3-103">如何對 Azure Active Directory 使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="f46c3-103">How to use the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="f46c3-104">概觀</span><span class="sxs-lookup"><span data-stu-id="f46c3-104">Overview</span></span>

<span data-ttu-id="f46c3-105">本文示範如何使用 **[Spring Initializr]** (適用於 Azure Active Directory (Azure AD) 的 Spring Boot Starter) 建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="f46c3-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f46c3-106">先決條件</span><span class="sxs-lookup"><span data-stu-id="f46c3-106">Prerequisites</span></span>

<span data-ttu-id="f46c3-107">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="f46c3-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="f46c3-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="f46c3-109">[Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="f46c3-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="f46c3-110">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="f46c3-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="f46c3-111">使用 Spring Initializr 建立自訂應用程式</span><span class="sxs-lookup"><span data-stu-id="f46c3-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="f46c3-112">瀏覽至 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="f46c3-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="f46c3-113">指定您想要使用 [Java] 產生 [Maven 專案]、輸入應用程式的 [群組] 和 [成品] 名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。</span><span class="sxs-lookup"><span data-stu-id="f46c3-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定群組和成品名稱][security-01]

1. <span data-ttu-id="f46c3-115">向下捲動至 [核心] 區段，並核取 [安全性] 的方塊，然後在 [Web] 區段中核取 [Web] 的方塊。</span><span class="sxs-lookup"><span data-stu-id="f46c3-115">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![選取安全性和 Web Starter][security-02]

1. <span data-ttu-id="f46c3-117">向下捲動至 [Azure] 區段，然後核取 [Azure Active Directory] 的方塊。</span><span class="sxs-lookup"><span data-stu-id="f46c3-117">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![選取 Azure Active Directory Starter][security-03]

1. <span data-ttu-id="f46c3-119">捲動到頁面底部，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="f46c3-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![產生 Spring Boot 專案][security-04]

1. <span data-ttu-id="f46c3-121">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="f46c3-121">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="f46c3-122">建立和設定新的 Azure Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="f46c3-122">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="f46c3-123">建立 Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="f46c3-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="f46c3-124">登入 <https://portal.azure.com>。</span><span class="sxs-lookup"><span data-stu-id="f46c3-124">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="f46c3-125">依序按一下 [+新增]、[安全性 + 身分識別] 和 [Azure Active Directory]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-125">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![建立新的 Azure Active Directory 執行個體][directory-01]

1. <span data-ttu-id="f46c3-127">輸入您的 [組織名稱] 和您的 [初始網域名稱]，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-127">Enter your **Organization name** and your **Initial domain name**, and then click **Create**.</span></span>

   ![指定 Azure Active Directory 名稱][directory-02]

1. <span data-ttu-id="f46c3-129">從 Azure 入口網站頂端工具列上的下拉式功能表，選取您的新 Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="f46c3-129">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![選擇您的 Azure Active Directory][directory-03]

1. <span data-ttu-id="f46c3-131">從入口網站的功能表選取 [Azure Active Directory]，按一下 [屬性]，並複製**目錄識別碼** - 您將在本文稍後使用此識別碼。</span><span class="sxs-lookup"><span data-stu-id="f46c3-131">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID** - you will use that later in this article.</span></span>

   ![複製 Azure Active Directory 識別碼][directory-13]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="f46c3-133">為 Spring Boot 應用程式新增應用程式註冊</span><span class="sxs-lookup"><span data-stu-id="f46c3-133">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="f46c3-134">從入口網站的功能表選取 [Azure Active Directory]，按一下 [概觀]，然後按一下 [應用程式註冊]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-134">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![新增應用程式註冊][directory-04]

1. <span data-ttu-id="f46c3-136">按一下 [新增應用程式註冊]，指定您的應用程式 [名稱]，使用 http://localhost:8080 作為 [登入 URL]，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-136">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![建立新的應用程式註冊][directory-05]

1. <span data-ttu-id="f46c3-138">應用程式註冊建立好之後，請對它按一下。</span><span class="sxs-lookup"><span data-stu-id="f46c3-138">Click your application registration after it has been created.</span></span>

   ![選取您的應用程式註冊][directory-06]

1. <span data-ttu-id="f46c3-140">當應用程式註冊頁面出現時，複製您的 [應用程式識別碼] 以供稍後使用，按一下 [設定]，然後按一下 [金鑰]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-140">When the page for your app registration, copy your **Application ID** for later use, then click **Settings**, and then click **Keys**.</span></span>

   ![建立應用程式註冊金鑰][directory-07]

1. <span data-ttu-id="f46c3-142">新增 [說明]，並指定新金鑰的 [持續時間]，然後按一下 [儲存]；當您按一下 [儲存] 圖示時，系統就會自動填入金鑰的值，然後您必須複製金鑰值以供稍後使用。</span><span class="sxs-lookup"><span data-stu-id="f46c3-142">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key for later.</span></span> <span data-ttu-id="f46c3-143">(之後您就無法擷取此值)。</span><span class="sxs-lookup"><span data-stu-id="f46c3-143">(You will not be able to retrieve this value later.)</span></span>

   ![指定應用程式註冊金鑰參數][directory-08]

1. <span data-ttu-id="f46c3-145">從應用程式註冊的主要頁面，按一下 [設定]，然後按一下 [所需的權限]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-145">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![應用程式註冊所需的權限][directory-09]

1. <span data-ttu-id="f46c3-147">按一下 [Windows Azure Active Directory]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-147">Click **Windows Azure Active Directory**.</span></span>

   ![選取 [Windows Azure Active Directory]][directory-10]

1. <span data-ttu-id="f46c3-149">核取 [以登入使用者身分存取目錄] 和 [登入及讀取使用者個人檔案] 的方塊，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-149">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![啟用存取權限][directory-11]

1. <span data-ttu-id="f46c3-151">在 [所需的權限] 頁面上，按一下 [授與權限]，然後在出現提示時按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-151">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![授與存取權限][directory-12]

1. <span data-ttu-id="f46c3-153">從應用程式註冊的主要頁面，按一下 [設定]，然後按一下 [回覆 URL]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-153">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

   ![編輯 [回覆 URL]][directory-14]

1. <span data-ttu-id="f46c3-155">輸入 "http://localhost:8080/login/oauth2/code/azure" 作為新的回覆 URL，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-155">Enter "http://localhost:8080/login/oauth2/code/azure" as a new reply URL, and then click **Save**.</span></span>

   ![新增新回覆 URL][directory-15]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="f46c3-157">設定及編譯 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="f46c3-157">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="f46c3-158">從下載的專案封存檔將檔案解壓縮到某個目錄。</span><span class="sxs-lookup"><span data-stu-id="f46c3-158">Extract the files from the downloaded project archive into a directory.</span></span>

2. <span data-ttu-id="f46c3-159">瀏覽至專案中的父資料夾，然後在文字編輯器中開啟 pom.xml 檔案。</span><span class="sxs-lookup"><span data-stu-id="f46c3-159">Navigate to the parent folder in your project and open the *pom.xml* file in a text editor.</span></span>

3. <span data-ttu-id="f46c3-160">新增 Spring OAuth2 安全性的相依性；例如：</span><span class="sxs-lookup"><span data-stu-id="f46c3-160">Add the dependencies for Spring OAuth2 security; for example:</span></span>

   ```xml
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-client</artifactId>
   </dependency>
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-jose</artifactId>
   </dependency>
   ```

4. <span data-ttu-id="f46c3-161">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f46c3-161">Save and close the *pom.xml* file.</span></span>

5. <span data-ttu-id="f46c3-162">瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="f46c3-162">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

6. <span data-ttu-id="f46c3-163">使用稍早取得的值新增儲存體帳戶的金鑰；例如：</span><span class="sxs-lookup"><span data-stu-id="f46c3-163">Add the key for your storage account using the values from earlier; for example:</span></span>

   ```yaml
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222

   # Specifies your App Registration's Application ID:
   spring.security.oauth2.client.registration.azure.client-id=11111111-1111-1111-1111-1111111111111111

   # Specifies your App Registration's secret key:
   spring.security.oauth2.client.registration.azure.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authentication:
   azure.activedirectory.active-directory-groups=Users
   ```
   <span data-ttu-id="f46c3-164">其中：</span><span class="sxs-lookup"><span data-stu-id="f46c3-164">Where:</span></span>

   | <span data-ttu-id="f46c3-165">參數</span><span class="sxs-lookup"><span data-stu-id="f46c3-165">Parameter</span></span> | <span data-ttu-id="f46c3-166">說明</span><span class="sxs-lookup"><span data-stu-id="f46c3-166">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="f46c3-167">包含稍早 Active Directory 的**目錄識別碼**。</span><span class="sxs-lookup"><span data-stu-id="f46c3-167">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="f46c3-168">包含您稍早完成應用程式註冊所得到的**應用程式識別碼**。</span><span class="sxs-lookup"><span data-stu-id="f46c3-168">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="f46c3-169">包含您稍早完成應用程式註冊金鑰所得到的**值**。</span><span class="sxs-lookup"><span data-stu-id="f46c3-169">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="f46c3-170">包含要用於驗證的 Active Directory 群組清單。</span><span class="sxs-lookup"><span data-stu-id="f46c3-170">Contains a list of Active Directory groups to use for authentication.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="f46c3-171">如需 *application.properties* 檔案中可用值的完整清單，請參閱 GitHub 上的 [Azure Active Directory Spring Boot 範例][AAD Spring Boot Sample]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-171">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

7. <span data-ttu-id="f46c3-172">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f46c3-172">Save and close the *application.properties* file.</span></span>

8. <span data-ttu-id="f46c3-173">在應用程式的 Java 來源資料夾中，建立名為 controller 的資料夾；例如：src/main/java/com/wingtiptoys/security/controller。</span><span class="sxs-lookup"><span data-stu-id="f46c3-173">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

9. <span data-ttu-id="f46c3-174">在 controller 資料夾中建立名為 HelloController.java 的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="f46c3-174">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

10. <span data-ttu-id="f46c3-175">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="f46c3-175">Enter the following code, then save and close the file:</span></span>

   ```java
   package com.wingtiptoys.security;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.oauth2.client.OAuth2AuthorizedClient;
   import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
   import org.springframework.ui.Model;

   @RestController
   public class HelloController {
      @Autowired
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String helloWorld() {
         return "Hello World!";
      }
   }
   ```
   > [!NOTE]
   > 
   > <span data-ttu-id="f46c3-176">您為 `@PreAuthorize("hasRole('')")` 方法指定的群組名稱必須包含您在 *application.properties* 檔案 `azure.activedirectory.active-directory-groups` 欄位中所指定的其中一個群組。</span><span class="sxs-lookup"><span data-stu-id="f46c3-176">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   >

   > [!NOTE]
   > 
   > <span data-ttu-id="f46c3-177">您可以指定不同的授權設定，以用於不同的要求對應，例如：</span><span class="sxs-lookup"><span data-stu-id="f46c3-177">You can specify different authorization settings for different request mappings; for example:</span></span>
   >
   > ``` java
   > public class HelloController {
   >    @Autowired
   >    @PreAuthorize("hasRole('Users')")
   >    @RequestMapping("/")
   >    public String helloWorld() {
   >       return "Hello Users!";
   >    }
   >    @PreAuthorize("hasRole('Group1')")
   >    @RequestMapping("/Group1")
   >    public String groupOne() {
   >       return "Hello Group 1 Users!";
   >    }
   >    @PreAuthorize("hasRole('Group2')")
   >    @RequestMapping("/Group2")
   >    public String groupTwo() {
   >       return "Hello Group 2 Users!";
   >    }
   > }
   > ```
   >    

11. <span data-ttu-id="f46c3-178">在應用程式的 Java 來源資料夾中，建立名為 security 的資料夾；例如：src/main/java/com/wingtiptoys/security/security。</span><span class="sxs-lookup"><span data-stu-id="f46c3-178">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

12. <span data-ttu-id="f46c3-179">在 security 資料夾中建立名為 WebSecurityConfig.java 的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="f46c3-179">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

13. <span data-ttu-id="f46c3-180">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="f46c3-180">Enter the following code, then save and close the file:</span></span>

    ```java
    package com.wingtiptoys.security;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.security.oauth2.client.oidc.userinfo.OidcUserRequest;
    import org.springframework.security.oauth2.client.userinfo.OAuth2UserService;
    import org.springframework.security.oauth2.core.oidc.user.OidcUser;

    @EnableWebSecurity
    @EnableGlobalMethodSecurity(prePostEnabled = true)
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Autowired
        private OAuth2UserService<OidcUserRequest, OidcUser> oidcUserService;

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .oidcUserService(oidcUserService);
        }
    }
    ```

## <a name="build-and-test-your-app"></a><span data-ttu-id="f46c3-181">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="f46c3-181">Build and test your app</span></span>

1. <span data-ttu-id="f46c3-182">開啟命令提示字元，並將目錄切換到應用程式的 pom.xml 檔案所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f46c3-182">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="f46c3-183">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="f46c3-183">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![建置應用程式][build-application]

1. <span data-ttu-id="f46c3-185">由 Maven 建置並啟動應用程式之後，在網頁瀏覽器中開啟 <http://localhost:8080>；系統會提示您輸入使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="f46c3-185">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![登入您的應用程式][application-login]

1. <span data-ttu-id="f46c3-187">成功登入之後，您應該會從控制器看到範例文字 "Hello World"。</span><span class="sxs-lookup"><span data-stu-id="f46c3-187">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![成功登入][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="f46c3-189">未獲得授權的使用者帳戶將會收到 **HTTP 403 未獲授權**訊息。</span><span class="sxs-lookup"><span data-stu-id="f46c3-189">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="f46c3-190">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f46c3-190">Next steps</span></span>

<span data-ttu-id="f46c3-191">如需使用 Azure Active Directory 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="f46c3-191">For more information about using Azure Active Directory, see the following articles:</span></span>

* <span data-ttu-id="f46c3-192">[Azure Active Directory 文件]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-192">[Azure Active Directory Documentation].</span></span>

<span data-ttu-id="f46c3-193">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="f46c3-193">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="f46c3-194">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f46c3-194">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="f46c3-195">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="f46c3-195">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="f46c3-196">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-196">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="f46c3-197">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="f46c3-197">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="f46c3-198">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="f46c3-198">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="f46c3-199">為了協助開發人員開始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="f46c3-199">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="f46c3-200">除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f46c3-200">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="f46c3-201">如需更詳細的範例，請參閱 GitHub 上的 [Azure Active Directory Spring Boot 範例][AAD Spring Boot Sample]。</span><span class="sxs-lookup"><span data-stu-id="f46c3-201">For a more-detailed sample, see the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>

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
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

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
[directory-13]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-13.png
[directory-14]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-14.png
[directory-15]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-15.png

[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
