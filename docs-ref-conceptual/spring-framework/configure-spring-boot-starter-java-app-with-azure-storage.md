---
title: 如何對 Azure 儲存體使用 Spring Boot Starter
description: 了解如何使用 Azure 儲存體 Starter 來設定 Spring Boot Initializer 應用程式。
services: storage
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 09/10/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: 1a219a066f0f89adbf3f541856b36b842520bfbb
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2018
ms.locfileid: "46505916"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a><span data-ttu-id="2f6aa-103">如何對 Azure 儲存體使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="2f6aa-103">How to use the Spring Boot Starter for Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="2f6aa-104">概觀</span><span class="sxs-lookup"><span data-stu-id="2f6aa-104">Overview</span></span>

<span data-ttu-id="2f6aa-105">本文將逐步引導您使用 **Spring Initializr** 建立自訂應用程式，並將 Azure 儲存體 Starter 新增至您的應用程式，然後使用您的應用程式將 Blob 上傳至 Azure 儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-105">This article walks you through creating a custom application using the **Spring Initializr**, then adding the Azure storage starter to your application, and then using your application to upload a blob to your Azure storage account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f6aa-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="2f6aa-106">Prerequisites</span></span>

<span data-ttu-id="2f6aa-107">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="2f6aa-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或註冊[免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/)。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="2f6aa-109">[Azure 命令列介面 (CLI)](http://docs.microsoft.com/cli/azure/overview)。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="2f6aa-110">最新的 [Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-110">An up-to-date [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="2f6aa-111">Apache 的 [Maven](http://maven.apache.org/) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-111">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="2f6aa-112">您需要 Spring Boot 2.0 版或更新版本才能完成本文中的步驟。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-112">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a><span data-ttu-id="2f6aa-113">為您的應用程式建立 Azure 儲存體帳戶和 Blob 容器</span><span class="sxs-lookup"><span data-stu-id="2f6aa-113">Create an Azure Storage Account and blob container for your application</span></span>

1. <span data-ttu-id="2f6aa-114">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-114">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="2f6aa-115">依序按一下 [+ 建立資源]、[儲存體] 和 [儲存體帳戶]。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-115">Click **+Create a resource**, then **Storage**, and then click **Storage Account**.</span></span>

   ![建立 Azure 儲存體帳戶][IMG01]

1. <span data-ttu-id="2f6aa-117">在 [建立命名空間] 頁面上，輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-117">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="2f6aa-118">輸入唯一**名稱**，這將成為儲存體帳戶 URI 的一部分。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-118">Enter a unique **Name**, which will become part of the URI for your storage account.</span></span> <span data-ttu-id="2f6aa-119">例如：如果您輸入 **wingtiptoysstorage** 作為**名稱**，則 URI 會是 wingtiptoysstorage.core.windows.net。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-119">For example: if you entered **wingtiptoysstorage** for the **Name**, the URI would be *wingtiptoysstorage.core.windows.net*.</span></span>
   * <span data-ttu-id="2f6aa-120">選擇 [Blob 儲存體] 作為 [帳戶種類]。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-120">Choose **Blob storage** for the **Account kind**.</span></span>
   * <span data-ttu-id="2f6aa-121">指定儲存體帳戶的**位置**。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-121">Specify the **Location** for your storage account.</span></span>
   * <span data-ttu-id="2f6aa-122">選取您想要用於儲存體帳戶的**訂用帳戶**。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-122">Choose the **Subscription** you want to use for your storage account.</span></span>
   * <span data-ttu-id="2f6aa-123">指定是否要為儲存體帳戶建立新的**資源群組**，或選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-123">Specify whether to create a new **Resource group** for your storage account, or choose an existing resource group.</span></span>
   
   ![指定 Azure 儲存體帳戶選項][IMG02]

1. <span data-ttu-id="2f6aa-125">當您指定上面列出的選項之後，按一下 [建立] 以建立您的儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-125">When you have specified the options listed above, click **Create** to create your storage account.</span></span>

1. <span data-ttu-id="2f6aa-126">在 Azure 入口網站建立儲存體帳戶之後，按一下 [Blob]，然後按一下 [+ 容器]。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-126">When the Azure portal has created your storage account, click **Blobs**, then click **+Container**.</span></span>

   ![建立 Blob 容器][IMG03]

1. <span data-ttu-id="2f6aa-128">請輸入 Blob 容器的**名稱**，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-128">Enter a **Name** for your blob container, and then click **OK**.</span></span>

   ![指定 Blob 容器選項][IMG04]

1. <span data-ttu-id="2f6aa-130">Blob 容器會在建好後出現在 Azure 入口網站中。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-130">The Azure portal will list your blob container after is has been created.</span></span>

   ![檢視 Blob 容器的清單][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="2f6aa-132">使用 Spring Initializr 建立簡單的 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="2f6aa-132">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="2f6aa-133">瀏覽至 <https://start.spring.io/> 。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-133">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="2f6aa-134">指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-134">Specify the following options:</span></span>

   * <span data-ttu-id="2f6aa-135">以 **Java** 產生 **Maven** 專案。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-135">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="2f6aa-136">指定 **Spring Boot** 版本，應等於或大於 2.0。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-136">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="2f6aa-137">指定應用程式的**群組**和**成品**名稱。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-137">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="2f6aa-138">新增 **Web** 相依性。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-138">Add the **Web** dependency.</span></span>

      ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="2f6aa-140">Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：com.wingtiptoys.storage。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-140">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.storage*.</span></span>
   >

1. <span data-ttu-id="2f6aa-141">當您指定上面列出的選項之後，按一下 [產生專案]。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-141">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="2f6aa-142">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-142">When prompted, download the project to a path on your local computer.</span></span>

   ![下載 Spring 專案][SI02]

1. <span data-ttu-id="2f6aa-144">當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-144">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a><span data-ttu-id="2f6aa-145">設定 Spring Boot 應用程式以使用 Azure Storage Starter</span><span class="sxs-lookup"><span data-stu-id="2f6aa-145">Configure your Spring Boot app to use the Azure Storage starter</span></span>

1. <span data-ttu-id="2f6aa-146">在應用程式的根目錄中尋找 pom.xml 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-146">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\pom.xml`

   <span data-ttu-id="2f6aa-147">-或-</span><span class="sxs-lookup"><span data-stu-id="2f6aa-147">-or-</span></span>

   `/users/example/home/storage/pom.xml`

1. <span data-ttu-id="2f6aa-148">在文字編輯器中開啟 pom.xml 檔案，並將 Spring Cloud Azure 儲存體 Starter 新增至 `<dependencies>` 的清單中：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-148">Open the *pom.xml* file in a text editor, and add the Spring Cloud Azure Storage starter to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-azure-starter-storage</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][SI03]

1. <span data-ttu-id="2f6aa-150">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-150">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="2f6aa-151">建立 Azure 認證檔案</span><span class="sxs-lookup"><span data-stu-id="2f6aa-151">Create an Azure Credential File</span></span>

1. <span data-ttu-id="2f6aa-152">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-152">Open a command prompt.</span></span>

1. <span data-ttu-id="2f6aa-153">瀏覽至 Spring Boot 應用程式的 resources 目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-153">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   <span data-ttu-id="2f6aa-154">-或-</span><span class="sxs-lookup"><span data-stu-id="2f6aa-154">-or-</span></span>

   ```shell
   cd /users/example/home/storage/src/main/resources
   ```

1. <span data-ttu-id="2f6aa-155">登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-155">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="2f6aa-156">列出您的訂用帳戶：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-156">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="2f6aa-157">Azure 會傳回訂用帳戶清單，而且您必須複製所要使用之訂用帳戶的 GUID；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-157">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]

1. Specify the GUID for the subscription you want to use with Azure; for example:

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. <span data-ttu-id="2f6aa-158">建立 Azure 認證檔案：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-158">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="2f6aa-159">此命令會在您的 resources 目錄中建立 my.azureauth 檔案，內含會類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-159">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a><span data-ttu-id="2f6aa-160">設定 Spring Boot 應用程式以使用您的 Azure 儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="2f6aa-160">Configure your Spring Boot app to use your Azure Storage account</span></span>

1. <span data-ttu-id="2f6aa-161">在應用程式的 resources 目錄中尋找 *application.properties*；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-161">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   <span data-ttu-id="2f6aa-162">-或-</span><span class="sxs-lookup"><span data-stu-id="2f6aa-162">-or-</span></span>

   `/users/example/home/storage/src/main/resources/application.properties`

1.  <span data-ttu-id="2f6aa-163">在文字編輯器中開啟 application.properties 檔案、新增下列數行，然後使用您儲存體帳戶的適當屬性來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-163">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your storage account:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.storage.account=wingtiptoysstorage
   ```
   <span data-ttu-id="2f6aa-164">其中：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-164">Where:</span></span>
   | <span data-ttu-id="2f6aa-165">欄位</span><span class="sxs-lookup"><span data-stu-id="2f6aa-165">Field</span></span> | <span data-ttu-id="2f6aa-166">說明</span><span class="sxs-lookup"><span data-stu-id="2f6aa-166">Description</span></span> |
   | ---|---|
   | `spring.cloud.azure.credential-file-path` | <span data-ttu-id="2f6aa-167">指定您稍早在本教學課程中建立的 Azure 認證檔案。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-167">Specifies Azure credential file that you created earlier in this tutorial.</span></span> |
   | `spring.cloud.azure.resource-group` | <span data-ttu-id="2f6aa-168">指定包含您 Azure 儲存體帳戶的 Azure 資源群組。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-168">Specifies the Azure Resource Group that contains your Azure Storage account.</span></span> |
   | `spring.cloud.azure.region` | <span data-ttu-id="2f6aa-169">指定建立您 Azure 儲存體帳戶時指定的地理區域。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-169">Specifies the geographical region that you specified when you created your Azure Storage account.</span></span> |
   | `spring.cloud.azure.storage.account` | <span data-ttu-id="2f6aa-170">指定您稍早在本教學課程中建立的 Azure 儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-170">Specifies Azure Storage account that you created earlier in this tutorial.</span></span>

1. <span data-ttu-id="2f6aa-171">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-171">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a><span data-ttu-id="2f6aa-172">新增範例程式碼來實作基本的 Azure 儲存體功能</span><span class="sxs-lookup"><span data-stu-id="2f6aa-172">Add sample code to implement basic Azure storage functionality</span></span>

<span data-ttu-id="2f6aa-173">在本節中，您會建立必要的 Java 類別，以便將 Blob 儲存在 Azure 儲存體帳戶中。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-173">In this section, you create the necessary Java classes for storing a blob in your Azure storage account.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="2f6aa-174">修改主要應用程式類別</span><span class="sxs-lookup"><span data-stu-id="2f6aa-174">Modify the main application class</span></span>

1. <span data-ttu-id="2f6aa-175">在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-175">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   <span data-ttu-id="2f6aa-176">-或-</span><span class="sxs-lookup"><span data-stu-id="2f6aa-176">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. <span data-ttu-id="2f6aa-177">在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-177">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.storage;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class StorageApplication {
      public static void main(String[] args) {
         SpringApplication.run(StorageApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="2f6aa-178">儲存並關閉主要應用程式 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-178">Save and close the main application Java file.</span></span>

### <a name="add-a-web-controller-class"></a><span data-ttu-id="2f6aa-179">新增 Web 控制器類別</span><span class="sxs-lookup"><span data-stu-id="2f6aa-179">Add a web controller class</span></span>

1. <span data-ttu-id="2f6aa-180">在應用程式的 package 目錄中建立名為 *WebController.java* 的新 Java 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-180">Create a new Java file named *WebController.java* in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\WebController.java`

   <span data-ttu-id="2f6aa-181">-或-</span><span class="sxs-lookup"><span data-stu-id="2f6aa-181">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/WebController.java`

1. <span data-ttu-id="2f6aa-182">在文字編輯器中開啟Web 控制器 Java 檔案，並將下列數行新增至檔案中：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-182">Open the web controller Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.storage;
   
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.core.io.Resource;
   import org.springframework.core.io.WritableResource;
   import org.springframework.util.StreamUtils;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;
   
   import java.io.IOException;
   import java.io.OutputStream;
   import java.nio.charset.Charset;
   
   @RestController
   public class WebController {
   
      @Value("blob://test/myfile.txt")
      private Resource blobFile;

      @GetMapping(value = "/")
      public String readBlobFile() throws IOException {
         return StreamUtils.copyToString(
            this.blobFile.getInputStream(),
            Charset.defaultCharset()) + "\n";
      }
   
      @PostMapping(value = "/")
      public String writeBlobFile(@RequestBody String data) throws IOException {
         try (OutputStream os = ((WritableResource) this.blobFile).getOutputStream()) {
            os.write(data.getBytes());
         }
         return "File was updated.\n";
      }
   }
   ```
   
   <span data-ttu-id="2f6aa-183">其中 `@Value("blob://[container]/[blob]")` 語法會分別為用來儲存資料的容器和 Blob 定義名稱。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-183">Where the `@Value("blob://[container]/[blob]")` syntax respectively defines the names of the container and blob where you want to store the data.</span></span>

1. <span data-ttu-id="2f6aa-184">儲存並關閉 Web 控制器 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-184">Save and close the web controller Java file.</span></span>

1. <span data-ttu-id="2f6aa-185">開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-185">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\storage`

   <span data-ttu-id="2f6aa-186">-或-</span><span class="sxs-lookup"><span data-stu-id="2f6aa-186">-or-</span></span>

   `cd /users/example/home/storage`

1. <span data-ttu-id="2f6aa-187">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-187">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="2f6aa-188">當您的應用程式執行時，您可以使用cURL 來測試您的應用程式；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-188">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   <span data-ttu-id="2f6aa-189">a.</span><span class="sxs-lookup"><span data-stu-id="2f6aa-189">a.</span></span> <span data-ttu-id="2f6aa-190">傳送 POST 要求來更新檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-190">Send a POST request to update a file's contents:</span></span>

      ```shell
      curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
      ```

      <span data-ttu-id="2f6aa-191">您應該會看到檔案已更新的回應。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-191">You should see a response that the file was updated.</span></span>

   <span data-ttu-id="2f6aa-192">b.</span><span class="sxs-lookup"><span data-stu-id="2f6aa-192">b.</span></span> <span data-ttu-id="2f6aa-193">傳送 GET 要求來驗證檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-193">Send a GET request to verify the file's contents:</span></span>

      ```shell
      curl -X GET http://localhost:8080/
      ```

     <span data-ttu-id="2f6aa-194">您應該會看到您所發佈的 "Hello World" 文字。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-194">You should see the "Hello World" text that you posted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f6aa-195">後續步驟</span><span class="sxs-lookup"><span data-stu-id="2f6aa-195">Next steps</span></span>

<span data-ttu-id="2f6aa-196">如需可供 Microsoft Azure 使用之其他 Spring Boot Starter 的詳細資訊，請參閱[適用於 Azure 的 Spring Boot Starter](spring-boot-starters-for-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-196">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="2f6aa-197">如需將 Azure 功能整合到 Spring 架構應用程式的其他資訊，請參閱 [Azure 上的 Spring Framework](/java/azure/spring-framework/)。</span><span class="sxs-lookup"><span data-stu-id="2f6aa-197">For additional information about integrating Azure functionality into your Spring-based applications, see [Spring Framework on Azure](/java/azure/spring-framework/).</span></span>

<span data-ttu-id="2f6aa-198">如需可從 Spring Boot 應用程式呼叫之其他 Azure 儲存體 API 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="2f6aa-198">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="2f6aa-199">如何從 Java 使用 Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="2f6aa-199">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="2f6aa-200">如何從 Java 使用 Azure 佇列儲存體</span><span class="sxs-lookup"><span data-stu-id="2f6aa-200">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="2f6aa-201">如何從 Java 使用 Azure 資料表儲存體</span><span class="sxs-lookup"><span data-stu-id="2f6aa-201">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="2f6aa-202">如何從 Java 使用 Azure 檔案儲存體</span><span class="sxs-lookup"><span data-stu-id="2f6aa-202">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-03.png
