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
ms.date: 12/19/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: b3d97917deffa75bfab5f0d9ded64affd90139e1
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991392"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a>教學課程：使用適用於 Azure Active Directory 的 Spring Boot Starter 保護 Java Web 應用程式

## <a name="overview"></a>概觀

本文示範如何使用 **[Spring Initializr]** (適用於 Azure Active Directory (Azure AD) 的 Spring Boot Starter) 建立 Java 應用程式。

在本教學課程中，您了解如何：

> [!div class="checklist"]
> * 使用 Spring Initializr 建立 Java 應用程式
> * 設定 Azure Active Directory
> * 使用 Spring Boot 類別和註解保護應用程式
> * 建置與測試您的 Java 應用程式

如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

## <a name="prerequisites"></a>必要條件

請務必具備下列必要條件，以便本文中說明的步驟：

* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。

## <a name="create-an-app-using-spring-initializr"></a>使用 Spring Initialzr 建立應用程式

1. 瀏覽至 <https://start.spring.io/> 。

1. 指定您想要使用 **JAVA** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。

   ![指定群組和成品名稱][create-spring-app-01]

1. 向下捲動至**核心**區段並勾選**安全性**方塊，並在 **Web** 區段中勾選 **Web** 方塊，接著向下捲動至 **Azure** 區段並勾選 **Azure Active Directory**方塊。

   ![選取安全性、Web 以及 Azure Active Directory 入門版][create-spring-app-02]

1. 捲動到頁面頂端或底部，然後按一下按鈕以**產生專案**。

   ![產生 Spring Boot 專案][create-spring-app-03]

1. 出現提示時，將專案下載至本機電腦上的路徑。

## <a name="create-azure-active-directory-instance"></a>建立 Azure Active Directory 執行個體

### <a name="create-the-active-directory-instance"></a>建立 Active Directory 執行個體

1. 登入 <https://portal.azure.com>。

1. 依序按一下 [+新增資源]、[身分識別] 和 [Azure Active Directory]。

   ![建立新的 Azure Active Directory 執行個體][create-directory-01]

1. 輸入您的 [組織名稱] 和您的 [初始網域名稱]。 複製您目錄的完整 URL；您稍後在本教學課程中，將使用它來新增使用者帳戶。 (例如：`wingtiptoysdirectory.onmicrosoft.com`)。完成時，請按一下 [建立]。

   ![指定 Azure Active Directory 名稱][create-directory-02]

1. 在 Azure 入口網站工具列右上方選取您的帳戶名稱，接著按一下 [切換目錄]。

   ![選取您的 Azure 帳戶名稱][create-directory-03]

1. 從下拉式功能表，選取您的新 Azure Active Directory。

   ![選擇您的 Azure Active Directory][create-directory-04]

1. 從入口網站的功能表選取 [Azure Active Directory]，按一下 [屬性]，並複製**目錄識別碼**；您稍後在本教學課程中，將使用這個值來設定 application.properties 檔案。

   ![複製 Azure Active Directory 識別碼][create-directory-05]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>為 Spring Boot 應用程式新增應用程式註冊

1. 從入口網站的功能表選取 [Azure Active Directory]，按一下 [應用程式註冊]，然後按一下 [新增應用程式註冊]。

   ![新增應用程式註冊][create-app-registration-01]

2. 指定您的應用程式 [名稱]，使用 http://localhost:8080 作為 [登入 URL]，然後按一下 [建立]。

   ![建立新的應用程式註冊][create-app-registration-02]

4. 當您應用程式的註冊頁面顯示時，請複製您的**應用程式識別碼**；您稍後在本教學課程中，將使用此值來設定您的 application.properties 檔案。 按一下 [設定]，然後按一下 [金鑰]。

   ![建立應用程式註冊金鑰][create-app-registration-03]

5. 新增 [說明]，並指定新金鑰的 [持續時間]，然後按一下 [儲存]；當您按一下 [儲存] 圖示時，系統就會自動填入金鑰的值，您必須複製金鑰值，以供稍後在本教學課程中設定您的 application.properties 檔案。 (之後您就無法擷取此值)。

   ![指定應用程式註冊金鑰參數][create-app-registration-04]

6. 從應用程式註冊的主要頁面，按一下 [設定]，然後按一下 [所需的權限]。

   ![應用程式註冊所需的權限][create-app-registration-05]

7. 按一下 [Windows Azure Active Directory]。

   ![選取 [Windows Azure Active Directory]][create-app-registration-06]

8. 核取 [以登入使用者身分存取目錄] 和 [登入及讀取使用者個人檔案] 的方塊，然後按一下 [儲存]。

   ![啟用存取權限][create-app-registration-07]

9. 在 [所需的權限] 頁面上，按一下 [授與權限]，然後在出現提示時按一下 [是]。

   ![授與存取權限][create-app-registration-08]

10. 從應用程式註冊的主要頁面，按一下 [設定]，然後按一下 [回覆 URL]。

    ![編輯 [回覆 URL]][create-app-registration-09]

11. 輸入 "<http://localhost:8080/login/oauth2/code/azure>" 作為新的回覆 URL，然後按一下 [儲存]。

    ![新增新回覆 URL][create-app-registration-10]

12. 從您應用程式註冊的主頁面中，按一下 [資訊清單]，然後將 `oauth2AllowImplicitFlow` 參數的值設定為 `true`，然後按一下 [儲存]。

    ![設定應用程式資訊清單][create-app-registration-11]

    > [!NOTE]
    > 
    > 如需 `oauth2AllowImplicitFlow` 參數和其他應用程式設定的詳細資訊，請參閱 [Azure Active Directory 應用程式資訊清單][AAD app manifest]。 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a>將使用者帳戶新增至您的目錄，然後將該帳戶新增至群組

1. 從 Active Directory 的 [概觀] 頁面中，按一下 [所有使用者]，再按一下 [新增使用者]。

   ![新增新的使用者帳戶][create-user-01]

1. 當 [使用者] 面板顯示時，輸入 [名稱] 和 [使用者名稱]。

   ![輸入使用者帳戶資訊][create-user-02]

   > [!NOTE]
   > 
   > 當您輸入使用者名稱時，必須指定本教學課程中稍早的目錄 URL；例如：
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. 按一下 [群組]，然後選取您要在應用程式中用來進行授權的群組，然後按一下 [選取]。 (基於本教學課程的目的，將帳戶新增至_使用者_群組。)

   ![選取使用者的群組][create-user-03]

1. 按一下 [顯示密碼]，並複製密碼；當您登入本教學課程中稍後的應用程式時，將使用此密碼。 複製好密碼之後，按一下 [建立] 將新的使用者帳戶新增至您的目錄。

   ![顯示密碼][create-user-04]

## <a name="configure-and-compile-your-app"></a>設定及編譯您的應用程式

1. 從您稍早在本教學課程中建立並下載的專案封存，將檔案解壓縮到目錄中。

1. 瀏覽至專案中的父資料夾，然後在文字編輯器中開啟 `pom.xml`Maven 專案檔。

1. 將適用於 Spring OAth2 安全性的相依性新增至 `pom.xml`：

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

1. 儲存並關閉 *pom.xml* 檔案。

1. 瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。

1. 使用您稍早建立的值來指定應用程式註冊的設定；例如：

   ```yaml
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222

   # Specifies your App Registration's Application ID:
   spring.security.oauth2.client.registration.azure.client-id=11111111-1111-1111-1111-1111111111111111

   # Specifies your App Registration's secret key:
   spring.security.oauth2.client.registration.azure.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authorization:
   azure.activedirectory.active-directory-groups=Users
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `azure.activedirectory.tenant-id` | 包含稍早 Active Directory 的**目錄識別碼**。 |
   | `spring.security.oauth2.client.registration.azure.client-id` | 包含您稍早完成應用程式註冊所得到的**應用程式識別碼**。 |
   | `spring.security.oauth2.client.registration.azure.client-secret` | 包含您稍早完成應用程式註冊金鑰所得到的**值**。 |
   | `azure.activedirectory.active-directory-groups` | 包含要用於授權的 Active Directory 群組清單。 |

   > [!NOTE]
   > 
   > 如需 *application.properties* 檔案中可用值的完整清單，請參閱 GitHub 上的 [Azure Active Directory Spring Boot 範例][AAD Spring Boot Sample]。
   >

1. 儲存並關閉 *application.properties* 檔案。

1. 在應用程式的 Java 來源資料夾中，建立名為 controller 的資料夾；例如：src/main/java/com/wingtiptoys/security/controller。

1. 在 controller 資料夾中建立名為 HelloController.java 的新 Java 檔案，然後在文字編輯器中加以開啟。

1. 輸入下列程式碼，然後儲存並關閉檔案：

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
   > 您為 `@PreAuthorize("hasRole('')")` 方法指定的群組名稱必須包含您在 *application.properties* 檔案 `azure.activedirectory.active-directory-groups` 欄位中所指定的其中一個群組。
   > 
   > 您也可以指定不同的授權設定，以用於不同的要求對應，例如：
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

1. 在應用程式的 Java 來源資料夾中，建立名為 security 的資料夾；例如：src/main/java/com/wingtiptoys/security/security。

1. 在 security 資料夾中建立名為 WebSecurityConfig.java 的新 Java 檔案，然後在文字編輯器中加以開啟。

1. 輸入下列程式碼，然後儲存並關閉檔案：

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

## <a name="build-and-test-your-app"></a>建置及測試您的應用程式

1. 開啟命令提示字元，並將目錄切換到應用程式的 pom.xml 檔案所在的資料夾。

1. 使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![建置應用程式][build-application]

1. 由 Maven 建置並啟動應用程式之後，在網頁瀏覽器中開啟 <http://localhost:8080>；系統會提示您輸入使用者名稱和密碼。

   ![登入您的應用程式][application-login]

   > [!NOTE]
   > 
   > 如果這是新使用者帳戶的第一次登入，系統可能會提示您變更密碼。
   > 
   > ![變更您的密碼][update-password]
   > 

1. 成功登入之後，您應該會從控制器看到範例文字 "Hello World"。

   ![成功登入][hello-world]

   > [!NOTE]
   > 
   > 未獲得授權的使用者帳戶將會收到 **HTTP 403 未獲授權**訊息。
   >

## <a name="summary"></a>總結

在本教學課程中，您已使用 Azure Active Direcotry 入門版建立了新的 Java Web 應用程式、設定了新的 Azure AD 租用戶並在其中註冊了新的應用程式，並接著將您的應用程式設定為使用 Spring 註解和類別來保護 Web 應用程式。

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: /java/azure/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

<!-- IMG List -->

[create-spring-app-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-01.png
[create-spring-app-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-02.png
[create-spring-app-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-03.png

[create-directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-01.png
[create-directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-02.png
[create-directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-03.png
[create-directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-04.png
[create-directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-05.png

[create-app-registration-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-01.png
[create-app-registration-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-02.png
[create-app-registration-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03.png
[create-app-registration-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04.png
[create-app-registration-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-05.png
[create-app-registration-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-06.png
[create-app-registration-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-07.png
[create-app-registration-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-08.png
[create-app-registration-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-09.png
[create-app-registration-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-10.png
[create-app-registration-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-11.png

[create-user-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-01.png
[create-user-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-02.png
[create-user-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-03.png
[create-user-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-04.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
