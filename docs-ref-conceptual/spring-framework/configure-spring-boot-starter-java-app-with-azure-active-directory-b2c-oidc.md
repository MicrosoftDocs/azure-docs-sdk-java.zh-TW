---
title: 如何對 Azure Active Directory B2C 使用 Spring Boot Starter
description: 了解如何使用 Azure Active Directory B2C Starter 來設定 Spring Boot Initializer 應用程式。
services: active-directory-b2c
documentationcenter: java
author: panli
manager: kevinzha
editor: panli
ms.assetid: ''
ms.author: panli
ms.date: 02/28/2019
ms.devlang: java
ms.service: active-directory-b2c
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 21aa6c812a519d686356851a57f00688d9a24fa4
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625714"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a><span data-ttu-id="e002d-103">教學課程：使用適用於 Azure Active Directory B2C 的 Spring Boot Starter 保護 Java Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e002d-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory B2C.</span></span>

## <a name="overview"></a><span data-ttu-id="e002d-104">概觀</span><span class="sxs-lookup"><span data-stu-id="e002d-104">Overview</span></span>

<span data-ttu-id="e002d-105">本文示範如何透過 [Spring Initializr](https://start.spring.io/) (使用適用於 Azure Active Directory (Azure AD) 的 Spring Boot Starter) 建立 Java 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e002d-105">This article demonstrates creating a Java app with the [Spring Initializr](https://start.spring.io/) that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e002d-106">在本教學課程中，您了解如何：</span><span class="sxs-lookup"><span data-stu-id="e002d-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e002d-107">使用 Spring Initializr 建立 Java 應用程式</span><span class="sxs-lookup"><span data-stu-id="e002d-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="e002d-108">設定 Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="e002d-108">Configure Azure Active Directory B2C</span></span>
> * <span data-ttu-id="e002d-109">使用 Spring Boot 類別和註解保護應用程式</span><span class="sxs-lookup"><span data-stu-id="e002d-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="e002d-110">建置與測試您的 Java 應用程式</span><span class="sxs-lookup"><span data-stu-id="e002d-110">Build and test your Java application</span></span>

<span data-ttu-id="e002d-111">如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。</span><span class="sxs-lookup"><span data-stu-id="e002d-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e002d-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="e002d-112">Prerequisites</span></span>

<span data-ttu-id="e002d-113">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="e002d-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="e002d-114">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="e002d-114">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="e002d-115">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="e002d-115">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="e002d-116">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e002d-116">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-spring-initializr"></a><span data-ttu-id="e002d-117">使用 Spring Initialzr 建立應用程式</span><span class="sxs-lookup"><span data-stu-id="e002d-117">Create an app using Spring Initializr</span></span>

1. <span data-ttu-id="e002d-118">瀏覽至 <https://start.spring.io/> 。</span><span class="sxs-lookup"><span data-stu-id="e002d-118">Browse to <https://start.spring.io/>.</span></span>

2. <span data-ttu-id="e002d-119">指定您想要使用 **Java** 產生 **Maven** 專案、為您的應用程式輸入**群組**和**成品**名稱，然後選取 Spring Initializr 的 **Web** 和**安全性**模組。</span><span class="sxs-lookup"><span data-stu-id="e002d-119">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then select the **Web** and **Security** module of the Spring Initializr.</span></span>

   ![指定群組和成品名稱](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/SI.png)


3. <span data-ttu-id="e002d-121">按一下 `Generate Project`，並在出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="e002d-121">Click `Generate Project`, download the project to a path on your local computer when prompted.</span></span>

## <a name="create-azure-active-directory-instance"></a><span data-ttu-id="e002d-122">建立 Azure Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="e002d-122">Create Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="e002d-123">建立 Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="e002d-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="e002d-124">登入 <https://portal.azure.com>。</span><span class="sxs-lookup"><span data-stu-id="e002d-124">Log into <https://portal.azure.com>.</span></span>

2. <span data-ttu-id="e002d-125">依序按一下 [+新增資源]  、[身分識別]  和 [Azure Active Directory B2C]  。</span><span class="sxs-lookup"><span data-stu-id="e002d-125">Click **+Create a resource**, then **Identity**, and then **Azure Active Directory B2C**.</span></span>

   ![建立新的 Azure Active Directory B2C 執行個體](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ1.png)

3. <span data-ttu-id="e002d-127">輸入您的 [組織名稱]  和您的 [初始網域名稱]  ，並將 [網域名稱]  記錄為您的 `${your-tenant-name}`，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="e002d-127">Enter your **Organization name** and your **Initial domain name**, record the **domain name** as your `${your-tenant-name}` and click **Create**.</span></span>

   ![取得您的 B2C 租用戶名稱](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ5.png)

4. <span data-ttu-id="e002d-129">在 Azure 入口網站工具列右上方選取您的帳戶名稱，接著按一下 [切換目錄]  。</span><span class="sxs-lookup"><span data-stu-id="e002d-129">Select your account name on the top-right of the Azure portal toolbar, then click **Switch directory**.</span></span>

   ![選擇您的 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ2.png)

5. <span data-ttu-id="e002d-131">從下拉式功能表，選取您的新 Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="e002d-131">Select your new Azure Active Directory from the drop-down menu.</span></span>

   ![選擇您的 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ3.png)

6. <span data-ttu-id="e002d-133">搜尋 `b2c`，然後按一下 `Azure AD B2C` 服務。</span><span class="sxs-lookup"><span data-stu-id="e002d-133">Search `b2c` and click `Azure AD B2C` service.</span></span>

   ![尋找 Azure Active Directory B2C 執行個體](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ4.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="e002d-135">為 Spring Boot 應用程式新增應用程式註冊</span><span class="sxs-lookup"><span data-stu-id="e002d-135">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="e002d-136">從入口網站功能表中選取 [Azure AD B2C]  ，按一下 [應用程式]  ，然後按一下 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="e002d-136">Select **Azure AD B2C** from the portal menu, click **Applications**, and then click **Add**.</span></span>

   ![新增應用程式註冊](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C1.png)

2. <span data-ttu-id="e002d-138">指定應用程式的 [名稱]  、新增 `http://localhost:8080/home` 作為 [回覆 URL]  ，將 [應用程式識別碼]  記錄為您的 `${your-client-id}`，然後按一下 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="e002d-138">Specify your application **Name**, add `http://localhost:8080/home` for the **Reply URL**, record the **Application ID** as your `${your-client-id}` and then click **Save**.</span></span>

   ![新增應用程式回覆 URL](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C2.png)

3. <span data-ttu-id="e002d-140">從您的應用程式中選取 [金鑰]  ，按一下 [產生金鑰]  以產生 `${your-client-secret}`，然後按一下 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="e002d-140">Select **Keys** from your application, click **Generate key** to generate `${your-client-secret}` and then **Save**.</span></span>

4. <span data-ttu-id="e002d-141">選取左側的 [使用者流程]  ，然後**按一下** **新增使用者流程**。</span><span class="sxs-lookup"><span data-stu-id="e002d-141">Select **User flows** on your left, and then **Click** \*\*New user flow \*\*.</span></span>

   ![建立使用者流程](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C3.png)

5. <span data-ttu-id="e002d-143">選擇 [註冊或登入]  、[設定檔編輯]  和 [密碼重設]  以分別建立使用者流程。</span><span class="sxs-lookup"><span data-stu-id="e002d-143">Choose **Sign up or in**, **Profile editing** and **Password reset** to create user flows respectively.</span></span> <span data-ttu-id="e002d-144">指定使用者流程的 [名稱]  和 [使用者屬性與宣告]  ，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="e002d-144">Specify your user flow **Name** and **User attributes and claims**, click **Create**.</span></span>

   ![設定使用者流程](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C4.png)

## <a name="configure-and-compile-your-app"></a><span data-ttu-id="e002d-146">設定及編譯您的應用程式</span><span class="sxs-lookup"><span data-stu-id="e002d-146">Configure and compile your app</span></span>

1. <span data-ttu-id="e002d-147">從您稍早在本教學課程中建立並下載的專案封存，將檔案解壓縮到目錄中。</span><span class="sxs-lookup"><span data-stu-id="e002d-147">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

2. <span data-ttu-id="e002d-148">瀏覽至專案中的父資料夾，然後在文字編輯器中開啟 `pom.xml`Maven 專案檔。</span><span class="sxs-lookup"><span data-stu-id="e002d-148">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

3. <span data-ttu-id="e002d-149">將適用於 Spring OAth2 安全性的相依性新增至 `pom.xml`：</span><span class="sxs-lookup"><span data-stu-id="e002d-149">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

   ```xml
   <dependency>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-active-directory-b2c-spring-boot-starter</artifactId>
       <version>2.1.6.M2</version>
   </dependency>
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-thymeleaf</artifactId>
   </dependency>
   <dependency>
       <groupId>org.thymeleaf.extras</groupId>
       <artifactId>thymeleaf-extras-springsecurity5</artifactId>
   </dependency>
   ```

4. <span data-ttu-id="e002d-150">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="e002d-150">Save and close the *pom.xml* file.</span></span>

5. <span data-ttu-id="e002d-151">瀏覽至專案中的 src/main/resources  資料夾，然後在文字編輯器中開啟 application.yml  檔案。</span><span class="sxs-lookup"><span data-stu-id="e002d-151">Navigate to the *src/main/resources* folder in your project and open the *application.yml* file in a text editor.</span></span>

6. <span data-ttu-id="e002d-152">使用您稍早建立的值來指定應用程式註冊的設定；例如：</span><span class="sxs-lookup"><span data-stu-id="e002d-152">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

   ```yaml
   azure:
     activedirectory:
       b2c:
         tenant: ${your-tenant-name}
         client-id: ${your-client-id}
         client-secret: ${your-client-secret}
         reply-url: ${your-reply-url-from-aad} # should be absolute url.
         logout-success-url: ${you-logout-success-url}
         user-flows:
           sign-up-or-sign-in: ${your-sign-up-or-in-user-flow}
           profile-edit: ${your-profile-edit-user-flow}     # optional
           password-reset: ${your-password-reset-user-flow} # optional
   ```
   <span data-ttu-id="e002d-153">其中：</span><span class="sxs-lookup"><span data-stu-id="e002d-153">Where:</span></span>

   | <span data-ttu-id="e002d-154">參數</span><span class="sxs-lookup"><span data-stu-id="e002d-154">Parameter</span></span> | <span data-ttu-id="e002d-155">說明</span><span class="sxs-lookup"><span data-stu-id="e002d-155">Description</span></span> |
   |---|---|
   | `azure.activedirectory.b2c.tenant` | <span data-ttu-id="e002d-156">包含您先前取得的 AD B2C `${your-tenant-name`。</span><span class="sxs-lookup"><span data-stu-id="e002d-156">Contains your AD B2C's `${your-tenant-name` from earlier.</span></span> |
   | `azure.activedirectory.b2c.client-id` | <span data-ttu-id="e002d-157">包含您先前完成的應用程式的 `${your-client-id}`。</span><span class="sxs-lookup"><span data-stu-id="e002d-157">Contains the `${your-client-id}` from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.client-secret` | <span data-ttu-id="e002d-158">包含您先前完成的應用程式的 `${your-client-secret}`。</span><span class="sxs-lookup"><span data-stu-id="e002d-158">Contains the `${your-client-secret}` from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.reply-url` | <span data-ttu-id="e002d-159">包含您先前完成的應用程式的其中一個 [回覆 URL]  。</span><span class="sxs-lookup"><span data-stu-id="e002d-159">Contains one of the **Reply URL** from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.logout-success-url` | <span data-ttu-id="e002d-160">指定您的應用程式成功登出時的 URL。</span><span class="sxs-lookup"><span data-stu-id="e002d-160">Specify the URL when your application logout successfully.</span></span> |
   | `azure.activedirectory.b2c.user-flows` | <span data-ttu-id="e002d-161">包含您先前完成的使用者流程名稱。</span><span class="sxs-lookup"><span data-stu-id="e002d-161">Contains the name of the user flows that you completed earlier.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="e002d-162">如需 *application.yml* 檔案中可用值的完整清單，請參閱 GitHub 上的 [Azure Active Directory B2C Spring Boot 範例][AAD B2C Spring Boot 範例]。</span><span class="sxs-lookup"><span data-stu-id="e002d-162">For a full list of values that are available in your *application.yml* file, see the [Azure Active Directory B2C Spring Boot Sample][AAD B2C Spring Boot Sample] on GitHub.</span></span>
   >

7. <span data-ttu-id="e002d-163">儲存並關閉 application.yml  檔案。</span><span class="sxs-lookup"><span data-stu-id="e002d-163">Save and close the *application.yml* file.</span></span>

8. <span data-ttu-id="e002d-164">在應用程式的 Java 來源資料夾中建立名為 controller  的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e002d-164">Create a folder named *controller* in the Java source folder for your application.</span></span>

9. <span data-ttu-id="e002d-165">在 controller  資料夾中建立名為 HelloController.java  的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="e002d-165">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

10. <span data-ttu-id="e002d-166">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="e002d-166">Enter the following code, then save and close the file:</span></span>

    ```java
    package sample.aad.controller;
    
    import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
    import org.springframework.security.oauth2.core.user.OAuth2User;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;
    
    @Controller
    public class WebController {
    
        private void initializeModel(Model model, OAuth2AuthenticationToken token) {
            if (token != null) {
                final OAuth2User user = token.getPrincipal();
    
                model.addAttribute("grant_type", user.getAuthorities());
                model.addAllAttributes(user.getAttributes());
            }
        }
    
        @GetMapping(value = "/")
        public String index(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);
    
            return "home";
        }
    
        @GetMapping(value = "/greeting")
        public String greeting(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);
    
            return "greeting";
        }
    
        @GetMapping(value = "/home")
        public String home(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);
    
            return "home";
        }
    }
    ```

11. <span data-ttu-id="e002d-167">在應用程式的 Java 來源資料夾中建立名為 security  的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e002d-167">Create a folder named *security* in the Java source folder for your application.</span></span>

12. <span data-ttu-id="e002d-168">在 security  資料夾中建立名為 WebSecurityConfig.java  的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="e002d-168">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

13. <span data-ttu-id="e002d-169">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="e002d-169">Enter the following code, then save and close the file:</span></span>

    ```java
    package sample.aad.security;
    
    import com.microsoft.azure.spring.autoconfigure.b2c.AADB2COidcLoginConfigurer;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    
    @EnableWebSecurity
    public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {
    
        private final AADB2COidcLoginConfigurer configurer;
    
        public WebSecurityConfiguration(AADB2COidcLoginConfigurer configurer) {
            this.configurer = configurer;
        }
    
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                    .authorizeRequests()
                    .anyRequest()
                    .authenticated()
                    .and()
                    .apply(configurer)
            ;
        }
    }
    ```
14. <span data-ttu-id="e002d-170">從 [Azure AD B2C Spring Boot 範例](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates)中複製 `greeting.html` 和 `home.html`，並將 `${your-profile-edit-user-flow}` 和 `${your-password-reset-user-flow}` 分別取代為您先前完成的使用者流程名稱。</span><span class="sxs-lookup"><span data-stu-id="e002d-170">Copy the `greeting.html` and `home.html` from [Azure AD B2C Spring Boot Sample](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates), and replace the `${your-profile-edit-user-flow}` and `${your-password-reset-user-flow}` with your user flow name respectively that completed earlier.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="e002d-171">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="e002d-171">Build and test your app</span></span>

1. <span data-ttu-id="e002d-172">開啟命令提示字元，並將目錄切換到應用程式的 pom.xml  檔案所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e002d-172">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

2. <span data-ttu-id="e002d-173">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="e002d-173">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

3. <span data-ttu-id="e002d-174">由 Maven 建置並啟動應用程式之後，在網頁瀏覽器中開啟 <http://localhost:8080/>；您應該會重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e002d-174">After your application is built and started by Maven, open <http://localhost:8080/> in a web browser; you should be redirected to login page.</span></span>

   ![登入頁面](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO1.png)

4. <span data-ttu-id="e002d-176">按一下具有 `${your-sign-up-or-in}` 使用者流程名稱的連結，您應該會重新導向 Azure AD B2C 以啟動驗證程序。</span><span class="sxs-lookup"><span data-stu-id="e002d-176">Click linke with name of `${your-sign-up-or-in}` user flow, you should be rediected Azure AD B2C to start the authentication process.</span></span>

   ![Azure AD B2C 登入](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO2.png)

4. <span data-ttu-id="e002d-178">成功登入之後，您應該會在瀏覽器中看到範例 `home page`。</span><span class="sxs-lookup"><span data-stu-id="e002d-178">After you have logged in successfully, you should see the sample `home page` from the browser,</span></span>

   ![成功登入](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO3.png)

## <a name="summary"></a><span data-ttu-id="e002d-180">總結</span><span class="sxs-lookup"><span data-stu-id="e002d-180">Summary</span></span>

<span data-ttu-id="e002d-181">在本教學課程中，您已使用 Azure Active Direcotry B2C Starter 建立了新的 Java Web 應用程式、設定了新的 Azure AD B2C 租用戶並在其中註冊了新的應用程式，並接著將您的應用程式設定為使用 Spring 註解和類別來保護 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e002d-181">In this tutorial, you created a new Java web application using the Azure Active Directory B2C starter, configured a new Azure AD B2C tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e002d-182">後續步驟</span><span class="sxs-lookup"><span data-stu-id="e002d-182">Next steps</span></span>

<span data-ttu-id="e002d-183">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="e002d-183">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e002d-184">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="e002d-184">Spring on Azure</span></span>](/java/azure/spring-framework)
