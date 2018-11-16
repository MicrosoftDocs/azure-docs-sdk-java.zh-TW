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
ms.date: 07/02/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: da44a40b7b52e75bb0a946b46ddfc033bfef54e9
ms.sourcegitcommit: 473c3aec55f3e9b131dc87c62e2eac218ce9564e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "51571715"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="95737-103">教學課程：使用適用於 Azure Active Directory 的 Spring Boot Starter 保護 Java Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="95737-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="95737-104">概觀</span><span class="sxs-lookup"><span data-stu-id="95737-104">Overview</span></span>

<span data-ttu-id="95737-105">本文示範如何使用 **[Spring Initializr]** (適用於 Azure Active Directory (Azure AD) 的 Spring Boot Starter) 建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="95737-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="95737-106">在本教學課程中，您了解如何：</span><span class="sxs-lookup"><span data-stu-id="95737-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="95737-107">使用 Spring Initializr 建立 Java 應用程式</span><span class="sxs-lookup"><span data-stu-id="95737-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="95737-108">設定 Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95737-108">Configure Azure Active Directory</span></span>
> * <span data-ttu-id="95737-109">使用 Spring Boot 類別和註解保護應用程式</span><span class="sxs-lookup"><span data-stu-id="95737-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="95737-110">建置與測試您的 Java 應用程式</span><span class="sxs-lookup"><span data-stu-id="95737-110">Build and test your Java application</span></span>

<span data-ttu-id="95737-111">如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。</span><span class="sxs-lookup"><span data-stu-id="95737-111">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95737-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="95737-112">Prerequisites</span></span>

<span data-ttu-id="95737-113">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="95737-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="95737-114">[Java 開發套件 (JDK)](https://aka.ms/azure-jdks) \(英文\) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="95737-114">A [Java Development Kit (JDK)](https://aka.ms/azure-jdks), version 1.7 or later.</span></span>
* <span data-ttu-id="95737-115">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="95737-115">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-application-using-the-spring-initializr"></a><span data-ttu-id="95737-116">使用 Spring Initializr 建立應用程式</span><span class="sxs-lookup"><span data-stu-id="95737-116">Create an application using the Spring Initializr</span></span>

1. <span data-ttu-id="95737-117">瀏覽至 <https://start.spring.io/> 。</span><span class="sxs-lookup"><span data-stu-id="95737-117">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="95737-118">指定您想要使用 **JAVA** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。</span><span class="sxs-lookup"><span data-stu-id="95737-118">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定群組和成品名稱][security-01]

1. <span data-ttu-id="95737-120">向下捲動至 [核心] 區段，並核取 [安全性] 的方塊，然後在 [Web] 區段中核取 [Web] 的方塊。</span><span class="sxs-lookup"><span data-stu-id="95737-120">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![選取安全性和 Web Starter][security-02]

1. <span data-ttu-id="95737-122">向下捲動至 [Azure] 區段，然後核取 [Azure Active Directory] 的方塊。</span><span class="sxs-lookup"><span data-stu-id="95737-122">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![選取 Azure Active Directory Starter][security-03]

1. <span data-ttu-id="95737-124">捲動到頁面底部，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="95737-124">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![產生 Spring Boot 專案][security-04]

1. <span data-ttu-id="95737-126">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="95737-126">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="95737-127">建立和設定新的 Azure Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="95737-127">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="95737-128">建立 Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="95737-128">Create the Active Directory instance</span></span>

1. <span data-ttu-id="95737-129">登入 <https://portal.azure.com>。</span><span class="sxs-lookup"><span data-stu-id="95737-129">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="95737-130">依序按一下 [+新增]、[安全性 + 身分識別] 和 [Azure Active Directory]。</span><span class="sxs-lookup"><span data-stu-id="95737-130">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![建立新的 Azure Active Directory 執行個體][directory-01]

1. <span data-ttu-id="95737-132">輸入您的 [組織名稱] 和您的 [初始網域名稱]。</span><span class="sxs-lookup"><span data-stu-id="95737-132">Enter your **Organization name** and your **Initial domain name**.</span></span> <span data-ttu-id="95737-133">複製您目錄的完整 URL；您稍後在本教學課程中，將使用它來新增使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="95737-133">Copy the full URL of your directory; you will use that to add user accounts later in this tutorial.</span></span> <span data-ttu-id="95737-134">(例如：`wingtiptoysdirectory.onmicrosoft.com`)。完成時，請按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="95737-134">(For example: `wingtiptoysdirectory.onmicrosoft.com`.) When you have finished, click **Create**.</span></span>

   ![指定 Azure Active Directory 名稱][directory-02]

1. <span data-ttu-id="95737-136">從 Azure 入口網站頂端工具列上的下拉式功能表，選取您的新 Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="95737-136">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![選擇您的 Azure Active Directory][directory-03]

1. <span data-ttu-id="95737-138">從入口網站的功能表選取 [Azure Active Directory]，按一下 [屬性]，並複製**目錄識別碼**；您稍後在本教學課程中，將使用這個值來設定 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="95737-138">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID**; you will use that value to configure your *application.properties* file later in this tutorial.</span></span>

   ![複製 Azure Active Directory 識別碼][directory-13]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="95737-140">為 Spring Boot 應用程式新增應用程式註冊</span><span class="sxs-lookup"><span data-stu-id="95737-140">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="95737-141">從入口網站的功能表選取 [Azure Active Directory]，按一下 [概觀]，然後按一下 [應用程式註冊]。</span><span class="sxs-lookup"><span data-stu-id="95737-141">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![新增應用程式註冊][directory-04]

2. <span data-ttu-id="95737-143">按一下 [新增應用程式註冊]，指定您的應用程式 [名稱]，使用 http://localhost:8080 作為 [登入 URL]，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="95737-143">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![建立新的應用程式註冊][directory-05]

3. <span data-ttu-id="95737-145">應用程式註冊建立好之後，請對它按一下。</span><span class="sxs-lookup"><span data-stu-id="95737-145">Click your application registration after it has been created.</span></span>

   ![選取您的應用程式註冊][directory-06]

4. <span data-ttu-id="95737-147">當您應用程式的註冊頁面顯示時，請複製您的**應用程式識別碼**；您稍後在本教學課程中，將使用此值來設定您的 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="95737-147">When the page for your app registration appears, copy your **Application ID**; you will use this value to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="95737-148">按一下 [設定]，然後按一下 [金鑰]。</span><span class="sxs-lookup"><span data-stu-id="95737-148">Click **Settings**, and then click **Keys**.</span></span>

   ![建立應用程式註冊金鑰][directory-07]

5. <span data-ttu-id="95737-150">新增 [說明]，並指定新金鑰的 [持續時間]，然後按一下 [儲存]；當您按一下 [儲存] 圖示時，系統就會自動填入金鑰的值，您必須複製金鑰值，以供稍後在本教學課程中設定您的 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="95737-150">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="95737-151">(之後您就無法擷取此值)。</span><span class="sxs-lookup"><span data-stu-id="95737-151">(You will not be able to retrieve this value later.)</span></span>

   ![指定應用程式註冊金鑰參數][directory-08]

6. <span data-ttu-id="95737-153">從應用程式註冊的主要頁面，按一下 [設定]，然後按一下 [所需的權限]。</span><span class="sxs-lookup"><span data-stu-id="95737-153">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![應用程式註冊所需的權限][directory-09]

7. <span data-ttu-id="95737-155">按一下 [Windows Azure Active Directory]。</span><span class="sxs-lookup"><span data-stu-id="95737-155">Click **Windows Azure Active Directory**.</span></span>

   ![選取 [Windows Azure Active Directory]][directory-10]

8. <span data-ttu-id="95737-157">核取 [以登入使用者身分存取目錄] 和 [登入及讀取使用者個人檔案] 的方塊，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="95737-157">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![啟用存取權限][directory-11]

9. <span data-ttu-id="95737-159">在 [所需的權限] 頁面上，按一下 [授與權限]，然後在出現提示時按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="95737-159">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![授與存取權限][directory-12]

10. <span data-ttu-id="95737-161">從應用程式註冊的主要頁面，按一下 [設定]，然後按一下 [回覆 URL]。</span><span class="sxs-lookup"><span data-stu-id="95737-161">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

    ![編輯 [回覆 URL]][directory-14]

11. <span data-ttu-id="95737-163">輸入 "<http://localhost:8080/login/oauth2/code/azure>" 作為新的回覆 URL，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="95737-163">Enter "<http://localhost:8080/login/oauth2/code/azure>" as a new reply URL, and then click **Save**.</span></span>

    ![新增新回覆 URL][directory-15]

12. <span data-ttu-id="95737-165">從您應用程式註冊的主頁面中，按一下 [資訊清單]，然後將 `oauth2AllowImplicitFlow` 參數的值設定為 `true`，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="95737-165">From the main page for your app registration, click **Manifest**, then set the value of the `oauth2AllowImplicitFlow` parameter to `true`, and then click **Save**.</span></span>

    ![設定應用程式資訊清單][directory-16]

    > [!NOTE]
    > 
    > <span data-ttu-id="95737-167">如需 `oauth2AllowImplicitFlow` 參數和其他應用程式設定的詳細資訊，請參閱 [Azure Active Directory 應用程式資訊清單][AAD app manifest]。</span><span class="sxs-lookup"><span data-stu-id="95737-167">For more information about the `oauth2AllowImplicitFlow` parameter and other application settings, see [Azure Active Directory application manifest][AAD app manifest].</span></span> 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a><span data-ttu-id="95737-168">將使用者帳戶新增至您的目錄，然後將該帳戶新增至群組</span><span class="sxs-lookup"><span data-stu-id="95737-168">Add a user account to your directory, and add that account to a group</span></span>

1. <span data-ttu-id="95737-169">從 Active Directory 的 [概觀] 頁面中，按一下 [使用者]。</span><span class="sxs-lookup"><span data-stu-id="95737-169">From the **Overview** page of your Active Directory, click **Users**.</span></span>

   ![開啟 [使用者] 面板][directory-17]

1. <span data-ttu-id="95737-171">當 [使用者] 面板顯示時，請按一下 [新增使用者]。</span><span class="sxs-lookup"><span data-stu-id="95737-171">When the **Users** panel is displayed, click **New user**.</span></span>

   ![新增新的使用者帳戶][directory-18]

1. <span data-ttu-id="95737-173">當 [使用者] 面板顯示時，輸入 [名稱] 和 [使用者名稱]。</span><span class="sxs-lookup"><span data-stu-id="95737-173">When the **User** panel is displayed, enter the **Name** and **User name**.</span></span>

   ![輸入使用者帳戶資訊][directory-19]

   > [!NOTE]
   > 
   > <span data-ttu-id="95737-175">當您輸入使用者名稱時，必須指定本教學課程中稍早的目錄 URL；例如：</span><span class="sxs-lookup"><span data-stu-id="95737-175">You need to specify your directory URL from earlier in this tutorial when you enter the user name; for example:</span></span>
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. <span data-ttu-id="95737-176">按一下 [群組]，然後選取您要在應用程式中用來進行授權的群組，然後按一下 [選取]。</span><span class="sxs-lookup"><span data-stu-id="95737-176">Click **Groups**, then select the groups that you will use for authorization in your application, and then click **Select**.</span></span> <span data-ttu-id="95737-177">(基於本教學課程的目的，將帳戶新增至_使用者_群組。)</span><span class="sxs-lookup"><span data-stu-id="95737-177">(For the purposes of this tutorial, add the account to the _Users_ group.)</span></span>

   ![選取使用者的群組][directory-20]

1. <span data-ttu-id="95737-179">按一下 [顯示密碼]，並複製密碼；當您登入本教學課程中稍後的應用程式時，將使用此密碼。</span><span class="sxs-lookup"><span data-stu-id="95737-179">Click **Show password**, and copy the password; you will use this when you log into your application later in this tutorial.</span></span>

   ![顯示密碼][directory-21]

1. <span data-ttu-id="95737-181">按一下 [建立] 將新的使用者帳戶新增至您的目錄。</span><span class="sxs-lookup"><span data-stu-id="95737-181">Click **Create** to add the new user account to your directory.</span></span>

   ![建立新的使用者帳戶][directory-22]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="95737-183">設定及編譯 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="95737-183">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="95737-184">從您稍早在本教學課程中建立並下載的專案封存，將檔案解壓縮到目錄中。</span><span class="sxs-lookup"><span data-stu-id="95737-184">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

1. <span data-ttu-id="95737-185">瀏覽至專案中的父資料夾，然後在文字編輯器中開啟 `pom.xml`Maven 專案檔。</span><span class="sxs-lookup"><span data-stu-id="95737-185">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

1. <span data-ttu-id="95737-186">將適用於 Spring OAth2 安全性的相依性新增至 `pom.xml`：</span><span class="sxs-lookup"><span data-stu-id="95737-186">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

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

1. <span data-ttu-id="95737-187">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="95737-187">Save and close the *pom.xml* file.</span></span>

1. <span data-ttu-id="95737-188">瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="95737-188">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="95737-189">使用您稍早建立的值來指定應用程式註冊的設定；例如：</span><span class="sxs-lookup"><span data-stu-id="95737-189">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

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
   <span data-ttu-id="95737-190">其中：</span><span class="sxs-lookup"><span data-stu-id="95737-190">Where:</span></span>

   | <span data-ttu-id="95737-191">參數</span><span class="sxs-lookup"><span data-stu-id="95737-191">Parameter</span></span> | <span data-ttu-id="95737-192">說明</span><span class="sxs-lookup"><span data-stu-id="95737-192">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="95737-193">包含稍早 Active Directory 的**目錄識別碼**。</span><span class="sxs-lookup"><span data-stu-id="95737-193">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="95737-194">包含您稍早完成應用程式註冊所得到的**應用程式識別碼**。</span><span class="sxs-lookup"><span data-stu-id="95737-194">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="95737-195">包含您稍早完成應用程式註冊金鑰所得到的**值**。</span><span class="sxs-lookup"><span data-stu-id="95737-195">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="95737-196">包含要用於授權的 Active Directory 群組清單。</span><span class="sxs-lookup"><span data-stu-id="95737-196">Contains a list of Active Directory groups to use for authorization.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="95737-197">如需 *application.properties* 檔案中可用值的完整清單，請參閱 GitHub 上的 [Azure Active Directory Spring Boot 範例][AAD Spring Boot Sample]。</span><span class="sxs-lookup"><span data-stu-id="95737-197">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

1. <span data-ttu-id="95737-198">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="95737-198">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="95737-199">在應用程式的 Java 來源資料夾中，建立名為 controller 的資料夾；例如：src/main/java/com/wingtiptoys/security/controller。</span><span class="sxs-lookup"><span data-stu-id="95737-199">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="95737-200">在 controller 資料夾中建立名為 HelloController.java 的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="95737-200">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="95737-201">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="95737-201">Enter the following code, then save and close the file:</span></span>

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
   > <span data-ttu-id="95737-202">您為 `@PreAuthorize("hasRole('')")` 方法指定的群組名稱必須包含您在 *application.properties* 檔案 `azure.activedirectory.active-directory-groups` 欄位中所指定的其中一個群組。</span><span class="sxs-lookup"><span data-stu-id="95737-202">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   >

   > [!NOTE]
   > 
   > <span data-ttu-id="95737-203">您可以指定不同的授權設定，以用於不同的要求對應，例如：</span><span class="sxs-lookup"><span data-stu-id="95737-203">You can specify different authorization settings for different request mappings; for example:</span></span>
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

1. <span data-ttu-id="95737-204">在應用程式的 Java 來源資料夾中，建立名為 security 的資料夾；例如：src/main/java/com/wingtiptoys/security/security。</span><span class="sxs-lookup"><span data-stu-id="95737-204">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="95737-205">在 security 資料夾中建立名為 WebSecurityConfig.java 的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="95737-205">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="95737-206">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="95737-206">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="95737-207">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="95737-207">Build and test your app</span></span>

1. <span data-ttu-id="95737-208">開啟命令提示字元，並將目錄切換到應用程式的 pom.xml 檔案所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="95737-208">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="95737-209">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="95737-209">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![建置應用程式][build-application]

1. <span data-ttu-id="95737-211">由 Maven 建置並啟動應用程式之後，在網頁瀏覽器中開啟 <http://localhost:8080>；系統會提示您輸入使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="95737-211">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![登入您的應用程式][application-login]

   > [!NOTE]
   > 
   > <span data-ttu-id="95737-213">如果這是新使用者帳戶的第一次登入，系統可能會提示您變更密碼。</span><span class="sxs-lookup"><span data-stu-id="95737-213">You may be prompted to change your password if this is the first login for a new user account.</span></span>
   > 
   > ![變更您的密碼][update-password]
   > 

1. <span data-ttu-id="95737-215">成功登入之後，您應該會從控制器看到範例文字 "Hello World"。</span><span class="sxs-lookup"><span data-stu-id="95737-215">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![成功登入][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="95737-217">未獲得授權的使用者帳戶將會收到 **HTTP 403 未獲授權**訊息。</span><span class="sxs-lookup"><span data-stu-id="95737-217">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="95737-218">後續步驟</span><span class="sxs-lookup"><span data-stu-id="95737-218">Next steps</span></span>

<span data-ttu-id="95737-219">在本教學課程中，您已使用 Azure Active Direcotry 入門版建立了新的 Java Web 應用程式、設定了新的 Azure AD 租用戶並在其中註冊了新的應用程式，並接著將您的應用程式設定為使用 Spring 註解和類別來保護 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="95737-219">In this tutorial, you created a new Java web application using the Azure Active Directory starter, configured a new Azure AD tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span> <span data-ttu-id="95737-220">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="95737-220">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="95737-221">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="95737-221">Spring on Azure</span></span>](/java/azure/spring-framework)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: /java/azure/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
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
[directory-16]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-16.png
[directory-17]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-17.png
[directory-18]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-18.png
[directory-19]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-19.png
[directory-20]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-20.png
[directory-21]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-21.png
[directory-22]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-22.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
