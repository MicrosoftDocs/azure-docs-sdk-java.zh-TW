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
ms.date: 11/21/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 89e294fa739b59f87667f901e914fd5f050b5b8c
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338772"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="452a6-103">教學課程：使用適用於 Azure Active Directory 的 Spring Boot Starter 保護 Java Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="452a6-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="452a6-104">概觀</span><span class="sxs-lookup"><span data-stu-id="452a6-104">Overview</span></span>

<span data-ttu-id="452a6-105">本文示範如何使用 **[Spring Initializr]** (適用於 Azure Active Directory (Azure AD) 的 Spring Boot Starter) 建立 Java 應用程式。</span><span class="sxs-lookup"><span data-stu-id="452a6-105">This article demonstrates creating a Java app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="452a6-106">在本教學課程中，您了解如何：</span><span class="sxs-lookup"><span data-stu-id="452a6-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="452a6-107">使用 Spring Initializr 建立 Java 應用程式</span><span class="sxs-lookup"><span data-stu-id="452a6-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="452a6-108">設定 Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="452a6-108">Configure Azure Active Directory</span></span>
> * <span data-ttu-id="452a6-109">使用 Spring Boot 類別和註解保護應用程式</span><span class="sxs-lookup"><span data-stu-id="452a6-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="452a6-110">建置與測試您的 Java 應用程式</span><span class="sxs-lookup"><span data-stu-id="452a6-110">Build and test your Java application</span></span>

<span data-ttu-id="452a6-111">如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。</span><span class="sxs-lookup"><span data-stu-id="452a6-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="452a6-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="452a6-112">Prerequisites</span></span>

<span data-ttu-id="452a6-113">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="452a6-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="452a6-114">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="452a6-114">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="452a6-115">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="452a6-115">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="452a6-116">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="452a6-116">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-spring-initializr"></a><span data-ttu-id="452a6-117">使用 Spring Initialzr 建立應用程式</span><span class="sxs-lookup"><span data-stu-id="452a6-117">Create an app using Spring Initializr</span></span>

1. <span data-ttu-id="452a6-118">瀏覽至 <https://start.spring.io/> 。</span><span class="sxs-lookup"><span data-stu-id="452a6-118">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="452a6-119">指定您想要使用 **JAVA** 產生 **Maven** 專案、輸入應用程式的**群組**和**成品**名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。</span><span class="sxs-lookup"><span data-stu-id="452a6-119">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定群組和成品名稱][create-spring-app-01]

1. <span data-ttu-id="452a6-121">向下捲動至**核心**區段並勾選**安全性**方塊，並在 **Web** 區段中勾選 **Web** 方塊，接著向下捲動至 **Azure** 區段並勾選 **Azure Active Directory**方塊。</span><span class="sxs-lookup"><span data-stu-id="452a6-121">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**, then scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![選取安全性、Web 以及 Azure Active Directory 入門版][create-spring-app-02]

1. <span data-ttu-id="452a6-123">捲動到頁面頂端或底部，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="452a6-123">Scroll to the top or bottom of the page and click the button to **Generate Project**.</span></span>

   ![產生 Spring Boot 專案][create-spring-app-03]

1. <span data-ttu-id="452a6-125">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="452a6-125">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-azure-active-directory-instance"></a><span data-ttu-id="452a6-126">建立 Azure Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="452a6-126">Create Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="452a6-127">建立 Active Directory 執行個體</span><span class="sxs-lookup"><span data-stu-id="452a6-127">Create the Active Directory instance</span></span>

1. <span data-ttu-id="452a6-128">登入 <https://portal.azure.com>。</span><span class="sxs-lookup"><span data-stu-id="452a6-128">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="452a6-129">依序按一下 [+新增資源]、[身分識別] 和 [Azure Active Directory]。</span><span class="sxs-lookup"><span data-stu-id="452a6-129">Click **+Create a resource**, then **Identity**, and then **Azure Active Directory**.</span></span>

   ![建立新的 Azure Active Directory 執行個體][create-directory-01]

1. <span data-ttu-id="452a6-131">輸入您的 [組織名稱] 和您的 [初始網域名稱]。</span><span class="sxs-lookup"><span data-stu-id="452a6-131">Enter your **Organization name** and your **Initial domain name**.</span></span> <span data-ttu-id="452a6-132">複製您目錄的完整 URL；您稍後在本教學課程中，將使用它來新增使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="452a6-132">Copy the full URL of your directory; you will use that to add user accounts later in this tutorial.</span></span> <span data-ttu-id="452a6-133">(例如：`wingtiptoysdirectory.onmicrosoft.com`)。完成時，請按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="452a6-133">(For example: `wingtiptoysdirectory.onmicrosoft.com`.) When you have finished, click **Create**.</span></span>

   ![指定 Azure Active Directory 名稱][create-directory-02]

1. <span data-ttu-id="452a6-135">在 Azure 入口網站工具列右上方選取您的帳戶名稱，接著按一下 [切換目錄]。</span><span class="sxs-lookup"><span data-stu-id="452a6-135">Select your account name on the top-right of the Azure portal toolbar, then click **Switch directory**.</span></span>

   ![選取您的 Azure 帳戶名稱][create-directory-03]

1. <span data-ttu-id="452a6-137">從下拉式功能表，選取您的新 Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="452a6-137">Select your new Azure Active Directory from the drop-down menu.</span></span>

   ![選擇您的 Azure Active Directory][create-directory-04]

1. <span data-ttu-id="452a6-139">從入口網站的功能表選取 [Azure Active Directory]，按一下 [屬性]，並複製**目錄識別碼**；您稍後在本教學課程中，將使用這個值來設定 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="452a6-139">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID**; you will use that value to configure your *application.properties* file later in this tutorial.</span></span>

   ![複製 Azure Active Directory 識別碼][create-directory-05]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="452a6-141">為 Spring Boot 應用程式新增應用程式註冊</span><span class="sxs-lookup"><span data-stu-id="452a6-141">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="452a6-142">從入口網站的功能表選取 [Azure Active Directory]，按一下 [應用程式註冊]，然後按一下 [新增應用程式註冊]。</span><span class="sxs-lookup"><span data-stu-id="452a6-142">Select **Azure Active Directory** from the portal menu, click **App registrations**, and then click **New application registration**, .</span></span>

   ![新增應用程式註冊][create-app-registration-01]

2. <span data-ttu-id="452a6-144">指定您的應用程式 [名稱]，使用 http://localhost:8080 作為 [登入 URL]，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="452a6-144">Specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![建立新的應用程式註冊][create-app-registration-02]

4. <span data-ttu-id="452a6-146">當您應用程式的註冊頁面顯示時，請複製您的**應用程式識別碼**；您稍後在本教學課程中，將使用此值來設定您的 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="452a6-146">When the page for your app registration appears, copy your **Application ID**; you will use this value to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="452a6-147">按一下 [設定]，然後按一下 [金鑰]。</span><span class="sxs-lookup"><span data-stu-id="452a6-147">Click **Settings**, and then click **Keys**.</span></span>

   ![建立應用程式註冊金鑰][create-app-registration-03]

5. <span data-ttu-id="452a6-149">新增 [說明]，並指定新金鑰的 [持續時間]，然後按一下 [儲存]；當您按一下 [儲存] 圖示時，系統就會自動填入金鑰的值，您必須複製金鑰值，以供稍後在本教學課程中設定您的 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="452a6-149">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="452a6-150">(之後您就無法擷取此值)。</span><span class="sxs-lookup"><span data-stu-id="452a6-150">(You will not be able to retrieve this value later.)</span></span>

   ![指定應用程式註冊金鑰參數][create-app-registration-04]

6. <span data-ttu-id="452a6-152">從應用程式註冊的主要頁面，按一下 [設定]，然後按一下 [所需的權限]。</span><span class="sxs-lookup"><span data-stu-id="452a6-152">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![應用程式註冊所需的權限][create-app-registration-05]

7. <span data-ttu-id="452a6-154">按一下 [Windows Azure Active Directory]。</span><span class="sxs-lookup"><span data-stu-id="452a6-154">Click **Windows Azure Active Directory**.</span></span>

   ![選取 [Windows Azure Active Directory]][create-app-registration-06]

8. <span data-ttu-id="452a6-156">核取 [以登入使用者身分存取目錄] 和 [登入及讀取使用者個人檔案] 的方塊，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="452a6-156">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![啟用存取權限][create-app-registration-07]

9. <span data-ttu-id="452a6-158">在 [所需的權限] 頁面上，按一下 [授與權限]，然後在出現提示時按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="452a6-158">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![授與存取權限][create-app-registration-08]

10. <span data-ttu-id="452a6-160">從應用程式註冊的主要頁面，按一下 [設定]，然後按一下 [回覆 URL]。</span><span class="sxs-lookup"><span data-stu-id="452a6-160">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

    ![編輯 [回覆 URL]][create-app-registration-09]

11. <span data-ttu-id="452a6-162">輸入 "<http://localhost:8080/login/oauth2/code/azure>" 作為新的回覆 URL，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="452a6-162">Enter "<http://localhost:8080/login/oauth2/code/azure>" as a new reply URL, and then click **Save**.</span></span>

    ![新增新回覆 URL][create-app-registration-10]

12. <span data-ttu-id="452a6-164">從您應用程式註冊的主頁面中，按一下 [資訊清單]，然後將 `oauth2AllowImplicitFlow` 參數的值設定為 `true`，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="452a6-164">From the main page for your app registration, click **Manifest**, then set the value of the `oauth2AllowImplicitFlow` parameter to `true`, and then click **Save**.</span></span>

    ![設定應用程式資訊清單][create-app-registration-11]

    > [!NOTE]
    > 
    > <span data-ttu-id="452a6-166">如需 `oauth2AllowImplicitFlow` 參數和其他應用程式設定的詳細資訊，請參閱 [Azure Active Directory 應用程式資訊清單][AAD app manifest]。</span><span class="sxs-lookup"><span data-stu-id="452a6-166">For more information about the `oauth2AllowImplicitFlow` parameter and other application settings, see [Azure Active Directory application manifest][AAD app manifest].</span></span> 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a><span data-ttu-id="452a6-167">將使用者帳戶新增至您的目錄，然後將該帳戶新增至群組</span><span class="sxs-lookup"><span data-stu-id="452a6-167">Add a user account to your directory, and add that account to a group</span></span>

1. <span data-ttu-id="452a6-168">從 Active Directory 的 [概觀] 頁面中，按一下 [所有使用者]，再按一下 [新增使用者]。</span><span class="sxs-lookup"><span data-stu-id="452a6-168">From the **Overview** page of your Active Directory, click **All Users**, and then click **New user**.</span></span>

   ![新增新的使用者帳戶][create-user-01]

1. <span data-ttu-id="452a6-170">當 [使用者] 面板顯示時，輸入 [名稱] 和 [使用者名稱]。</span><span class="sxs-lookup"><span data-stu-id="452a6-170">When the **User** panel is displayed, enter the **Name** and **User name**.</span></span>

   ![輸入使用者帳戶資訊][create-user-02]

   > [!NOTE]
   > 
   > <span data-ttu-id="452a6-172">當您輸入使用者名稱時，必須指定本教學課程中稍早的目錄 URL；例如：</span><span class="sxs-lookup"><span data-stu-id="452a6-172">You need to specify your directory URL from earlier in this tutorial when you enter the user name; for example:</span></span>
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. <span data-ttu-id="452a6-173">按一下 [群組]，然後選取您要在應用程式中用來進行授權的群組，然後按一下 [選取]。</span><span class="sxs-lookup"><span data-stu-id="452a6-173">Click **Groups**, then select the groups that you will use for authorization in your application, and then click **Select**.</span></span> <span data-ttu-id="452a6-174">(基於本教學課程的目的，將帳戶新增至_使用者_群組。)</span><span class="sxs-lookup"><span data-stu-id="452a6-174">(For the purposes of this tutorial, add the account to the _Users_ group.)</span></span>

   ![選取使用者的群組][create-user-03]

1. <span data-ttu-id="452a6-176">按一下 [顯示密碼]，並複製密碼；當您登入本教學課程中稍後的應用程式時，將使用此密碼。</span><span class="sxs-lookup"><span data-stu-id="452a6-176">Click **Show password**, and copy the password; you will use this when you log into your application later in this tutorial.</span></span> <span data-ttu-id="452a6-177">複製好密碼之後，按一下 [建立] 將新的使用者帳戶新增至您的目錄。</span><span class="sxs-lookup"><span data-stu-id="452a6-177">When you have copied the password, click **Create** to add the new user account to your directory.</span></span>

   ![顯示密碼][create-user-04]

## <a name="configure-and-compile-your-app"></a><span data-ttu-id="452a6-179">設定及編譯您的應用程式</span><span class="sxs-lookup"><span data-stu-id="452a6-179">Configure and compile your app</span></span>

1. <span data-ttu-id="452a6-180">從您稍早在本教學課程中建立並下載的專案封存，將檔案解壓縮到目錄中。</span><span class="sxs-lookup"><span data-stu-id="452a6-180">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

1. <span data-ttu-id="452a6-181">瀏覽至專案中的父資料夾，然後在文字編輯器中開啟 `pom.xml`Maven 專案檔。</span><span class="sxs-lookup"><span data-stu-id="452a6-181">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

1. <span data-ttu-id="452a6-182">將適用於 Spring OAth2 安全性的相依性新增至 `pom.xml`：</span><span class="sxs-lookup"><span data-stu-id="452a6-182">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

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

1. <span data-ttu-id="452a6-183">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="452a6-183">Save and close the *pom.xml* file.</span></span>

1. <span data-ttu-id="452a6-184">瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="452a6-184">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="452a6-185">使用您稍早建立的值來指定應用程式註冊的設定；例如：</span><span class="sxs-lookup"><span data-stu-id="452a6-185">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

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
   <span data-ttu-id="452a6-186">其中：</span><span class="sxs-lookup"><span data-stu-id="452a6-186">Where:</span></span>

   | <span data-ttu-id="452a6-187">參數</span><span class="sxs-lookup"><span data-stu-id="452a6-187">Parameter</span></span> | <span data-ttu-id="452a6-188">說明</span><span class="sxs-lookup"><span data-stu-id="452a6-188">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="452a6-189">包含稍早 Active Directory 的**目錄識別碼**。</span><span class="sxs-lookup"><span data-stu-id="452a6-189">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="452a6-190">包含您稍早完成應用程式註冊所得到的**應用程式識別碼**。</span><span class="sxs-lookup"><span data-stu-id="452a6-190">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="452a6-191">包含您稍早完成應用程式註冊金鑰所得到的**值**。</span><span class="sxs-lookup"><span data-stu-id="452a6-191">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="452a6-192">包含要用於授權的 Active Directory 群組清單。</span><span class="sxs-lookup"><span data-stu-id="452a6-192">Contains a list of Active Directory groups to use for authorization.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="452a6-193">如需 *application.properties* 檔案中可用值的完整清單，請參閱 GitHub 上的 [Azure Active Directory Spring Boot 範例][AAD Spring Boot Sample]。</span><span class="sxs-lookup"><span data-stu-id="452a6-193">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

1. <span data-ttu-id="452a6-194">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="452a6-194">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="452a6-195">在應用程式的 Java 來源資料夾中，建立名為 controller 的資料夾；例如：src/main/java/com/wingtiptoys/security/controller。</span><span class="sxs-lookup"><span data-stu-id="452a6-195">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="452a6-196">在 controller 資料夾中建立名為 HelloController.java 的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="452a6-196">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="452a6-197">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="452a6-197">Enter the following code, then save and close the file:</span></span>

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
   > <span data-ttu-id="452a6-198">您為 `@PreAuthorize("hasRole('')")` 方法指定的群組名稱必須包含您在 *application.properties* 檔案 `azure.activedirectory.active-directory-groups` 欄位中所指定的其中一個群組。</span><span class="sxs-lookup"><span data-stu-id="452a6-198">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   > 
   > <span data-ttu-id="452a6-199">您也可以指定不同的授權設定，以用於不同的要求對應，例如：</span><span class="sxs-lookup"><span data-stu-id="452a6-199">You can also specify different authorization settings for different request mappings; for example:</span></span>
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

1. <span data-ttu-id="452a6-200">在應用程式的 Java 來源資料夾中，建立名為 security 的資料夾；例如：src/main/java/com/wingtiptoys/security/security。</span><span class="sxs-lookup"><span data-stu-id="452a6-200">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="452a6-201">在 security 資料夾中建立名為 WebSecurityConfig.java 的新 Java 檔案，然後在文字編輯器中加以開啟。</span><span class="sxs-lookup"><span data-stu-id="452a6-201">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="452a6-202">輸入下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="452a6-202">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="452a6-203">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="452a6-203">Build and test your app</span></span>

1. <span data-ttu-id="452a6-204">開啟命令提示字元，並將目錄切換到應用程式的 pom.xml 檔案所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="452a6-204">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="452a6-205">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="452a6-205">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![建置應用程式][build-application]

1. <span data-ttu-id="452a6-207">由 Maven 建置並啟動應用程式之後，在網頁瀏覽器中開啟 <http://localhost:8080>；系統會提示您輸入使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="452a6-207">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![登入您的應用程式][application-login]

   > [!NOTE]
   > 
   > <span data-ttu-id="452a6-209">如果這是新使用者帳戶的第一次登入，系統可能會提示您變更密碼。</span><span class="sxs-lookup"><span data-stu-id="452a6-209">You may be prompted to change your password if this is the first login for a new user account.</span></span>
   > 
   > ![變更您的密碼][update-password]
   > 

1. <span data-ttu-id="452a6-211">成功登入之後，您應該會從控制器看到範例文字 "Hello World"。</span><span class="sxs-lookup"><span data-stu-id="452a6-211">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![成功登入][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="452a6-213">未獲得授權的使用者帳戶將會收到 **HTTP 403 未獲授權**訊息。</span><span class="sxs-lookup"><span data-stu-id="452a6-213">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="summary"></a><span data-ttu-id="452a6-214">總結</span><span class="sxs-lookup"><span data-stu-id="452a6-214">Summary</span></span>

<span data-ttu-id="452a6-215">在本教學課程中，您已使用 Azure Active Direcotry 入門版建立了新的 Java Web 應用程式、設定了新的 Azure AD 租用戶並在其中註冊了新的應用程式，並接著將您的應用程式設定為使用 Spring 註解和類別來保護 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="452a6-215">In this tutorial, you created a new Java web application using the Azure Active Directory starter, configured a new Azure AD tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="452a6-216">後續步驟</span><span class="sxs-lookup"><span data-stu-id="452a6-216">Next steps</span></span>

<span data-ttu-id="452a6-217">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="452a6-217">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="452a6-218">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="452a6-218">Spring on Azure</span></span>](/java/azure/spring-framework)

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
