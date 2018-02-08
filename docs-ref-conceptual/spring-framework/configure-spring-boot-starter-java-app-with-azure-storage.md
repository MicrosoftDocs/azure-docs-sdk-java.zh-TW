---
title: "如何對 Azure 儲存體使用 Spring Boot Starter"
description: "了解如何使用 Azure 儲存體 Starter 來設定 Spring Boot Initializer 應用程式。"
services: storage
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: yungez;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: 50c8475c66250c8e872849007349277fd3fe797b
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a><span data-ttu-id="28594-103">如何對 Azure 儲存體使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="28594-103">How to use the Spring Boot Starter for Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="28594-104">概觀</span><span class="sxs-lookup"><span data-stu-id="28594-104">Overview</span></span>

<span data-ttu-id="28594-105">本文會逐步引導您使用 **Spring Initializr** 建立自訂應用程式，然後使用該應用程式存取 Azure 儲存體。</span><span class="sxs-lookup"><span data-stu-id="28594-105">This article walks you through creating a custom application using the **Spring Initializr**, and then using that application to access Azure storage.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28594-106">先決條件</span><span class="sxs-lookup"><span data-stu-id="28594-106">Prerequisites</span></span>

<span data-ttu-id="28594-107">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="28594-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="28594-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或註冊[免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/)。</span><span class="sxs-lookup"><span data-stu-id="28594-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="28594-109">[Azure 命令列介面 (CLI)](http://docs.microsoft.com/cli/azure/overview)。</span><span class="sxs-lookup"><span data-stu-id="28594-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="28594-110">最新的 [Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="28594-110">An up-to-date [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="28594-111">Apache 的 [Maven](http://maven.apache.org/) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="28594-111">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="28594-112">使用 Spring Initializr 建立自訂應用程式</span><span class="sxs-lookup"><span data-stu-id="28594-112">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="28594-113">瀏覽至 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="28594-113">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="28594-114">指定您想要使用 [Java] 產生 [Maven 專案]、輸入應用程式的 [群組] 和 [成品] 名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。</span><span class="sxs-lookup"><span data-stu-id="28594-114">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Spring Initializr 的基本選項](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-basic.png)

   > [!NOTE]
   >
   > <span data-ttu-id="28594-116">Spring Initializr 會使用 [群組] 和 [成品] 名稱來建立套件名稱；例如：com.contoso.wingtiptoysdemo。</span><span class="sxs-lookup"><span data-stu-id="28594-116">The Spring Initializr will use the **Group** and **Aritifact** names to create the package name; for example: *com.contoso.wingtiptoysdemo*.</span></span>
   >

1. <span data-ttu-id="28594-117">向下捲動至 [Azure] 區段，然後核取 [Azure 儲存體] 的方塊。</span><span class="sxs-lookup"><span data-stu-id="28594-117">Scroll down to the **Azure** section and check the box for **Azure Storage**.</span></span>

   ![Spring Initializr 的完整選項](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-advanced.png)

1. <span data-ttu-id="28594-119">捲動到頁面底部，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="28594-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Spring Initializr 的完整選項](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-generate.png)

1. <span data-ttu-id="28594-121">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="28594-121">When prompted, download the project to a path on your local computer.</span></span>

   ![下載自訂的 Spring Boot 專案](media/configure-spring-boot-starter-java-app-with-azure-storage/download-app.png)

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="28594-123">登入 Azure，並選取要使用的訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="28594-123">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="28594-124">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="28594-124">Open a command prompt.</span></span>

1. <span data-ttu-id="28594-125">使用 Azure CLI 登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="28594-125">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="28594-126">依照指示完成登入程序。</span><span class="sxs-lookup"><span data-stu-id="28594-126">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="28594-127">列出您的訂用帳戶：</span><span class="sxs-lookup"><span data-stu-id="28594-127">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="28594-128">Azure 會傳回訂用帳戶清單，而且您必須複製所要使用之訂用帳戶的 GUID；例如：</span><span class="sxs-lookup"><span data-stu-id="28594-128">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]

1. Specify the GUID for the account you want to use with Azure; for example:

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="28594-129">建立 Azure 儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="28594-129">Create an Azure Storage account</span></span>

1. <span data-ttu-id="28594-130">為您將在這篇文章中使用的 Azure 資源建立資源群組；例如：</span><span class="sxs-lookup"><span data-stu-id="28594-130">Create a resource group for the Azure resources you will use in this article; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="28594-131">其中：</span><span class="sxs-lookup"><span data-stu-id="28594-131">Where:</span></span>
   | <span data-ttu-id="28594-132">參數</span><span class="sxs-lookup"><span data-stu-id="28594-132">Parameter</span></span> | <span data-ttu-id="28594-133">說明</span><span class="sxs-lookup"><span data-stu-id="28594-133">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="28594-134">指定資源群組的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="28594-134">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="28594-135">指定要裝載資源群組的 [Azure 區域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="28594-135">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="28594-136">Azure CLI 將會顯示建立資源群組的結果，例如：</span><span class="sxs-lookup"><span data-stu-id="28594-136">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/wingtiptoysresources",
     "location": "westus",
     "managedBy": null,
     "name": "wingtiptoysresources",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

1. <span data-ttu-id="28594-137">在 Spring Boot 應用程式的資源群組中建立 Azure 儲存體帳戶；例如：</span><span class="sxs-lookup"><span data-stu-id="28594-137">Create an Azure storage account in the in the resource group for your Spring Boot app; for example:</span></span>
   ```azurecli
   az storage account create --name wingtiptoysstorage --resource-group wingtiptoysresources --location westus --sku Standard_LRS
   ```
   <span data-ttu-id="28594-138">其中：</span><span class="sxs-lookup"><span data-stu-id="28594-138">Where:</span></span>
   | <span data-ttu-id="28594-139">參數</span><span class="sxs-lookup"><span data-stu-id="28594-139">Parameter</span></span> | <span data-ttu-id="28594-140">說明</span><span class="sxs-lookup"><span data-stu-id="28594-140">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="28594-141">指定儲存體帳戶的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="28594-141">Specifies a unique name for your storage account.</span></span> |
   | `resource-group` | <span data-ttu-id="28594-142">指定您在上一個步驟中建立的資源群組名稱。</span><span class="sxs-lookup"><span data-stu-id="28594-142">Specifies the name of the resource group group you created in the previous step.</span></span> |
   | `location` | <span data-ttu-id="28594-143">指定要裝載儲存體帳戶的 [Azure 區域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="28594-143">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your storage account will be hosted.</span></span> |
   | `sku` | <span data-ttu-id="28594-144">指定下列其中之一：`Premium_LRS`、`Standard_GRS`、`Standard_LRS`、`Standard_RAGRS`、`Standard_ZRS`。</span><span class="sxs-lookup"><span data-stu-id="28594-144">Specifies one of the following: `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, `Standard_ZRS`.</span></span> |

   <span data-ttu-id="28594-145">Azure 會傳回 JSON 長字串，其中包含佈建狀態；例如：|</span><span class="sxs-lookup"><span data-stu-id="28594-145">Azure will return a long JSON string which contains the provisioning state; for example: |</span></span>
   
   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "identity": null,
     "kind": "Storage"
       ...
       ... (A long list of values will be displayed here.)
       ...
     "statusOfPrimary": "available",
     "statusOfSecondary": null,
     "tags": {},
     "type": "Microsoft.Storage/storageAccounts"
   }
   ```

1. <span data-ttu-id="28594-146">擷取儲存體帳戶的連接字串；例如：</span><span class="sxs-lookup"><span data-stu-id="28594-146">Retrieve the connection string for your storage account; for example:</span></span>
   ```azurecli
   az storage account show-connection-string --name wingtiptoysstorage --resource-group wingtiptoysresources
   ```
   <span data-ttu-id="28594-147">其中：</span><span class="sxs-lookup"><span data-stu-id="28594-147">Where:</span></span>
   | <span data-ttu-id="28594-148">參數</span><span class="sxs-lookup"><span data-stu-id="28594-148">Parameter</span></span> | <span data-ttu-id="28594-149">說明</span><span class="sxs-lookup"><span data-stu-id="28594-149">Description</span></span> |
   | ---|---|
   | `name` | <span data-ttu-id="28594-150">指定您在先前的步驟中建立的儲存體帳戶唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="28594-150">Specifies a unique name of the storage account you created in previous steps.</span></span> |
   | `resource-group` | <span data-ttu-id="28594-151">指定您在先前的步驟中建立的資源群組名稱。</span><span class="sxs-lookup"><span data-stu-id="28594-151">Specifies the name of the resource group you created in previous steps.</span></span> |

   <span data-ttu-id="28594-152">Azure 會傳回 JSON 字串，其中包含儲存體帳戶的連接字串；例如：</span><span class="sxs-lookup"><span data-stu-id="28594-152">Azure will return a JSON string which contains the connection string for your storage account; for example:</span></span>

   ```json
   {
     "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz=="
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="28594-153">設定及編譯 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="28594-153">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="28594-154">從下載的專案封存檔將檔案解壓縮到某個目錄。</span><span class="sxs-lookup"><span data-stu-id="28594-154">Extract the files from the downloaded project archive into a directory.</span></span>

1. <span data-ttu-id="28594-155">瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="28594-155">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="28594-156">新增儲存體帳戶的金鑰；例如：</span><span class="sxs-lookup"><span data-stu-id="28594-156">Add the key for your storage account; for example:</span></span>
   ```yaml
   azure.storage.connection-string=DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz==
   ```

1. <span data-ttu-id="28594-157">瀏覽至專案中的 /src/main/java/com/example/wingtiptoysdemo 資料夾，然後在文字編輯器中開啟 *WingtiptoysdemoApplication.java* 檔案。</span><span class="sxs-lookup"><span data-stu-id="28594-157">Navigate to the */src/main/java/com/example/wingtiptoysdemo* folder in your project and open the *WingtiptoysdemoApplication.java* file in a text editor.</span></span>

1. <span data-ttu-id="28594-158">使用列出容器中 Blob 的下列範例取代現有的 Java 程式碼：</span><span class="sxs-lookup"><span data-stu-id="28594-158">Replace the existing Java code with the following example that lists the blobs in a container:</span></span>

   ```java
   package com.example.wingtiptoysdemo;

   import com.microsoft.azure.storage.*;
   import com.microsoft.azure.storage.blob.*;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   import java.net.URISyntaxException;

   @SpringBootApplication
   public class WingtiptoysdemoApplication implements CommandLineRunner {

      @Autowired
      private CloudStorageAccount cloudStorageAccount;

      final String containerName = "mycontainer";

      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysdemoApplication.class, args);
      }

      public void run(String... var1)
             throws URISyntaxException, StorageException {
          // Create a container (if it does not exist).
          createContainerIfNotExists(containerName);
          // Upload a blob to the container.
          uploadTextBlob(containerName);
      }

      private void createContainerIfNotExists(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Create the container if it does not exist.
            container.createIfNotExists();
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }

      private void uploadTextBlob(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Get a blob reference for a text file.
            CloudBlockBlob blob = container.getBlockBlobReference("test.txt");
            // Upload some text into the blob.
            blob.uploadText("Hello World!");
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }
   }
   ```
   > [!NOTE]
   >
   > <span data-ttu-id="28594-159">上述範例會自動裝配您在 application.properties 檔案中所定義的儲存體帳戶設定。</span><span class="sxs-lookup"><span data-stu-id="28594-159">The above example autowires the storage account settings that you defined in the *application.properties* file.</span></span>
   >

1. <span data-ttu-id="28594-160">編譯和執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="28594-160">Compile and run the application:</span></span>
   ```shell
   mvn clean package spring-boot:run
   ```
   
   <span data-ttu-id="28594-161">應用程式會建立容器，並將 Blob 形式的文字檔上傳至容器，其將會列在您於 [Azure 入口網站](https://portal.azure.com)的儲存體帳戶底下。</span><span class="sxs-lookup"><span data-stu-id="28594-161">The application will create a container and upload a text file as a blob to the container, which will be listed under your storage account in the [Azure portal](https://portal.azure.com).</span></span>

   ![在 Azure 入口網站中列出 Blob](media/configure-spring-boot-starter-java-app-with-azure-storage/list-blobs-in-portal.png)

   > [!NOTE]
   > 
   > <span data-ttu-id="28594-163">在編譯應用程式時，您可能會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="28594-163">When you compile your application, you might see the following error message:</span></span>
   > 
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] BUILD FAILURE`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] Total time: 2.616 s`<br/>
   > `[INFO] Finished at: 2017-11-11T13:14:15Z`<br/>
   > `[INFO] Final Memory: 26M/213M`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2`<br/>
   > `.18.1:test (default-test) on project wingtiptoysdemo: Execution default-test of`<br/>
   > `goal org.apache.maven.plugins:maven-surefire-plugin:2.18.1:test failed: The for`<br/>
   > `ked VM terminated without properly saying goodbye. VM crash or System.exit called?`<br/>
   > `[ERROR] Command was /bin/sh -c cd /home/robert/SpringBoot/wingtiptoysdemo && /u`<br/>
   > `sr/lib/jvm/java-8-openjdk-amd64/jre/bin/java -jar /home/robert/SpringBoot/wingt`<br/>
   > `iptoysdemo/target/surefire/surefirebooter6371623993063346766.jar /home/robert/S`<br/>
   > `pringBoot/wingtiptoysdemo/target/surefire/surefire5107893623933537917tmp /home/`<br/>
   > `robert/SpringBoot/wingtiptoysdemo/target/surefire/surefire_01414159391084128068tmp`<br/>
   > `[ERROR] -> [Help 1]`<br/>
   > 
   > <span data-ttu-id="28594-164">如果發生這種情況，請停用 Maven Surefire 測試；若要這樣做，請在 pom.xml 檔案中新增下列外掛程式項目：</span><span class="sxs-lookup"><span data-stu-id="28594-164">If this happens, you might want to disable the Maven Surefire testing; to do so, add the following plugin entry in your *pom.xml* file:</span></span>
   > 
   > ```xml
   > <plugin>
   >   <groupId>org.apache.maven.plugins</groupId>
   >   <artifactId>maven-surefire-plugin</artifactId>
   >   <version>2.20.1</version>
   >   <configuration>
   >     <skipTests>true</skipTests>
   >   </configuration>
   > </plugin>
   > ```
   > 

## <a name="next-steps"></a><span data-ttu-id="28594-165">後續步驟</span><span class="sxs-lookup"><span data-stu-id="28594-165">Next steps</span></span>

<span data-ttu-id="28594-166">如需可供 Microsoft Azure 使用之其他 Spring Boot Starter 的詳細資訊，請參閱[適用於 Azure 的 Spring Boot Starter](spring-boot-starters-for-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="28594-166">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="28594-167">如需將 Azure 功能整合到 Spring 架構應用程式的其他資訊，請參閱 [Azure 上的 Spring Framework](/java/azure/spring-framework/)。</span><span class="sxs-lookup"><span data-stu-id="28594-167">For additional information about integrating Azure functionality into your Spring-based applications, see [Spring Framework on Azure](/java/azure/spring-framework/).</span></span>

<span data-ttu-id="28594-168">如需可從 Spring Boot 應用程式呼叫之其他 Azure 儲存體 API 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="28594-168">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="28594-169">如何從 Java 使用 Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="28594-169">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="28594-170">如何從 Java 使用 Azure 佇列儲存體</span><span class="sxs-lookup"><span data-stu-id="28594-170">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="28594-171">如何從 Java 使用 Azure 資料表儲存體</span><span class="sxs-lookup"><span data-stu-id="28594-171">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="28594-172">如何從 Java 使用 Azure 檔案儲存體</span><span class="sxs-lookup"><span data-stu-id="28594-172">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)
