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
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a>教學課程：使用適用於 Azure Active Directory B2C 的 Spring Boot Starter 保護 Java Web 應用程式。

## <a name="overview"></a>概觀

本文示範如何透過 [Spring Initializr](https://start.spring.io/) (使用適用於 Azure Active Directory (Azure AD) 的 Spring Boot Starter) 建立 Java 應用程式。

在本教學課程中，您了解如何：

> [!div class="checklist"]
> * 使用 Spring Initializr 建立 Java 應用程式
> * 設定 Azure Active Directory B2C
> * 使用 Spring Boot 類別和註解保護應用程式
> * 建置與測試您的 Java 應用程式

如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

## <a name="prerequisites"></a>必要條件

請務必具備下列必要條件，以便本文中說明的步驟：

* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。

## <a name="create-an-app-using-spring-initializr"></a>使用 Spring Initialzr 建立應用程式

1. 瀏覽至 <https://start.spring.io/> 。

2. 指定您想要使用 **Java** 產生 **Maven** 專案、為您的應用程式輸入**群組**和**成品**名稱，然後選取 Spring Initializr 的 **Web** 和**安全性**模組。

   ![指定群組和成品名稱](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/SI.png)


3. 按一下 `Generate Project`，並在出現提示時，將專案下載至本機電腦上的路徑。

## <a name="create-azure-active-directory-instance"></a>建立 Azure Active Directory 執行個體

### <a name="create-the-active-directory-instance"></a>建立 Active Directory 執行個體

1. 登入 <https://portal.azure.com>。

2. 依序按一下 [+新增資源]  、[身分識別]  和 [Azure Active Directory B2C]  。

   ![建立新的 Azure Active Directory B2C 執行個體](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ1.png)

3. 輸入您的 [組織名稱]  和您的 [初始網域名稱]  ，並將 [網域名稱]  記錄為您的 `${your-tenant-name}`，然後按一下 [建立]  。

   ![取得您的 B2C 租用戶名稱](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ5.png)

4. 在 Azure 入口網站工具列右上方選取您的帳戶名稱，接著按一下 [切換目錄]  。

   ![選擇您的 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ2.png)

5. 從下拉式功能表，選取您的新 Azure Active Directory。

   ![選擇您的 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ3.png)

6. 搜尋 `b2c`，然後按一下 `Azure AD B2C` 服務。

   ![尋找 Azure Active Directory B2C 執行個體](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ4.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>為 Spring Boot 應用程式新增應用程式註冊

1. 從入口網站功能表中選取 [Azure AD B2C]  ，按一下 [應用程式]  ，然後按一下 [新增]  。

   ![新增應用程式註冊](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C1.png)

2. 指定應用程式的 [名稱]  、新增 `http://localhost:8080/home` 作為 [回覆 URL]  ，將 [應用程式識別碼]  記錄為您的 `${your-client-id}`，然後按一下 [儲存]  。

   ![新增應用程式回覆 URL](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C2.png)

3. 從您的應用程式中選取 [金鑰]  ，按一下 [產生金鑰]  以產生 `${your-client-secret}`，然後按一下 [儲存]  。

4. 選取左側的 [使用者流程]  ，然後**按一下** **新增使用者流程**。

   ![建立使用者流程](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C3.png)

5. 選擇 [註冊或登入]  、[設定檔編輯]  和 [密碼重設]  以分別建立使用者流程。 指定使用者流程的 [名稱]  和 [使用者屬性與宣告]  ，然後按一下 [建立]  。

   ![設定使用者流程](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C4.png)

## <a name="configure-and-compile-your-app"></a>設定及編譯您的應用程式

1. 從您稍早在本教學課程中建立並下載的專案封存，將檔案解壓縮到目錄中。

2. 瀏覽至專案中的父資料夾，然後在文字編輯器中開啟 `pom.xml`Maven 專案檔。

3. 將適用於 Spring OAth2 安全性的相依性新增至 `pom.xml`：

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

4. 儲存並關閉 *pom.xml* 檔案。

5. 瀏覽至專案中的 src/main/resources  資料夾，然後在文字編輯器中開啟 application.yml  檔案。

6. 使用您稍早建立的值來指定應用程式註冊的設定；例如：

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
   其中：

   | 參數 | 說明 |
   |---|---|
   | `azure.activedirectory.b2c.tenant` | 包含您先前取得的 AD B2C `${your-tenant-name`。 |
   | `azure.activedirectory.b2c.client-id` | 包含您先前完成的應用程式的 `${your-client-id}`。 |
   | `azure.activedirectory.b2c.client-secret` | 包含您先前完成的應用程式的 `${your-client-secret}`。 |
   | `azure.activedirectory.b2c.reply-url` | 包含您先前完成的應用程式的其中一個 [回覆 URL]  。 |
   | `azure.activedirectory.b2c.logout-success-url` | 指定您的應用程式成功登出時的 URL。 |
   | `azure.activedirectory.b2c.user-flows` | 包含您先前完成的使用者流程名稱。

   > [!NOTE]
   > 
   > 如需 *application.yml* 檔案中可用值的完整清單，請參閱 GitHub 上的 [Azure Active Directory B2C Spring Boot 範例][AAD B2C Spring Boot 範例]。
   >

7. 儲存並關閉 application.yml  檔案。

8. 在應用程式的 Java 來源資料夾中建立名為 controller  的資料夾。

9. 在 controller  資料夾中建立名為 HelloController.java  的新 Java 檔案，然後在文字編輯器中加以開啟。

10. 輸入下列程式碼，然後儲存並關閉檔案：

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

11. 在應用程式的 Java 來源資料夾中建立名為 security  的資料夾。

12. 在 security  資料夾中建立名為 WebSecurityConfig.java  的新 Java 檔案，然後在文字編輯器中加以開啟。

13. 輸入下列程式碼，然後儲存並關閉檔案：

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
14. 從 [Azure AD B2C Spring Boot 範例](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates)中複製 `greeting.html` 和 `home.html`，並將 `${your-profile-edit-user-flow}` 和 `${your-password-reset-user-flow}` 分別取代為您先前完成的使用者流程名稱。

## <a name="build-and-test-your-app"></a>建置及測試您的應用程式

1. 開啟命令提示字元，並將目錄切換到應用程式的 pom.xml  檔案所在的資料夾。

2. 使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

3. 由 Maven 建置並啟動應用程式之後，在網頁瀏覽器中開啟 <http://localhost:8080/>；您應該會重新導向至登入頁面。

   ![登入頁面](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO1.png)

4. 按一下具有 `${your-sign-up-or-in}` 使用者流程名稱的連結，您應該會重新導向 Azure AD B2C 以啟動驗證程序。

   ![Azure AD B2C 登入](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO2.png)

4. 成功登入之後，您應該會在瀏覽器中看到範例 `home page`。

   ![成功登入](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO3.png)

## <a name="summary"></a>總結

在本教學課程中，您已使用 Azure Active Direcotry B2C Starter 建立了新的 Java Web 應用程式、設定了新的 Azure AD B2C 租用戶並在其中註冊了新的應用程式，並接著將您的應用程式設定為使用 Spring 註解和類別來保護 Web 應用程式。

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)
