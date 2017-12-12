---
title: "如何對 Azure Key Vault 使用 Spring Boot Starter"
description: "了解如何使用 Azure Key Vault Starter 來設定 Spring Boot Initializer 應用程式。"
services: key-vault
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/29/2017
ms.author: robmcm
ms.openlocfilehash: 8b35a972a00c995730dfa59b1b6a47fab7716b76
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a><span data-ttu-id="ff25e-103">如何對 Azure Key Vault 使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="ff25e-103">How to use the Spring Boot Starter for Azure Key Vault</span></span>

## <a name="overview"></a><span data-ttu-id="ff25e-104">概觀</span><span class="sxs-lookup"><span data-stu-id="ff25e-104">Overview</span></span>

<span data-ttu-id="ff25e-105">本文示範如何使用 **[Spring Initializr]** 建立應用程式，Spring Initializr 會使用適用於 Azure Key Vault 的 Spring Boot Starter，來擷取在金鑰保存庫中儲存作為祕密的連接字串。</span><span class="sxs-lookup"><span data-stu-id="ff25e-105">This article demonstrates creating an app with the **[Spring Initializr]** which uses the Spring Boot Starter for Azure Key Vault to retrieve a connection string that is stored as a secret in a key vault.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff25e-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="ff25e-106">Prerequisites</span></span>

<span data-ttu-id="ff25e-107">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="ff25e-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="ff25e-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="ff25e-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="ff25e-109">[Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="ff25e-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="ff25e-110">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="ff25e-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-the-spring-initialzr"></a><span data-ttu-id="ff25e-111">使用 Spring Initialzr 建立應用程式</span><span class="sxs-lookup"><span data-stu-id="ff25e-111">Create an app using the Spring Initialzr</span></span>

1. <span data-ttu-id="ff25e-112">瀏覽至 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="ff25e-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="ff25e-113">指定您想要使用 [Java] 產生 [Maven 專案]、輸入應用程式的 [群組] 和 [成品] 名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。</span><span class="sxs-lookup"><span data-stu-id="ff25e-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定群組和成品名稱][secrets-01]

1. <span data-ttu-id="ff25e-115">向下捲動至 [Azure] 區段，然後核取 [Azure Key Vault] 的方塊。</span><span class="sxs-lookup"><span data-stu-id="ff25e-115">Scroll down to the **Azure** section and check the box for **Azure Key Vault**.</span></span>

   ![選取 Azure Key Vault Starter][secrets-02]

1. <span data-ttu-id="ff25e-117">捲動到頁面底部，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="ff25e-117">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![產生 Spring Boot 專案][secrets-03]

1. <span data-ttu-id="ff25e-119">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="ff25e-119">When prompted, download the project to a path on your local computer.</span></span>

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="ff25e-120">登入 Azure，並選取要使用的訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="ff25e-120">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="ff25e-121">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="ff25e-121">Open a command prompt.</span></span>

1. <span data-ttu-id="ff25e-122">使用 Azure CLI 登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="ff25e-122">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="ff25e-123">依照指示完成登入程序。</span><span class="sxs-lookup"><span data-stu-id="ff25e-123">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="ff25e-124">列出您的訂用帳戶：</span><span class="sxs-lookup"><span data-stu-id="ff25e-124">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="ff25e-125">Azure 會傳回訂用帳戶清單，而且您必須複製所要使用之訂用帳戶的 GUID；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-125">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

## <a name="create-and-configure-a-new-azure-key-vault-using-the-azure-cli"></a><span data-ttu-id="ff25e-126">使用 Azure CLI 建立和設定新的 Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ff25e-126">Create and configure a new Azure Key Vault using the Azure CLI</span></span>

1. <span data-ttu-id="ff25e-127">為將要用於金鑰保存庫的 Azure 資源建立資源群組；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-127">Create a resource group for the Azure resources you will use for your key vault; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="ff25e-128">其中：</span><span class="sxs-lookup"><span data-stu-id="ff25e-128">Where:</span></span>
   | <span data-ttu-id="ff25e-129">參數</span><span class="sxs-lookup"><span data-stu-id="ff25e-129">Parameter</span></span> | <span data-ttu-id="ff25e-130">說明</span><span class="sxs-lookup"><span data-stu-id="ff25e-130">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="ff25e-131">指定資源群組的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="ff25e-131">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="ff25e-132">指定要裝載資源群組的 [Azure 區域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="ff25e-132">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="ff25e-133">Azure CLI 將會顯示建立資源群組的結果，例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-133">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

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

1. <span data-ttu-id="ff25e-134">從應用程式註冊建立 Azure 服務主體；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-134">Create an Azure service principal from your application registration; for example:</span></span>
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   | <span data-ttu-id="ff25e-135">參數</span><span class="sxs-lookup"><span data-stu-id="ff25e-135">Parameter</span></span> | <span data-ttu-id="ff25e-136">說明</span><span class="sxs-lookup"><span data-stu-id="ff25e-136">Description</span></span> |
   |---|---|
   | `id` | <span data-ttu-id="ff25e-137">指定從您稍早的應用程式註冊取得的 GUID。</span><span class="sxs-lookup"><span data-stu-id="ff25e-137">Specifies the GUID from your application registration earlier.</span></span> |

   <span data-ttu-id="ff25e-138">Azure CLI 會傳回 JSON 狀態訊息，其中包含 appId 和 password，可供您稍候用來作為用戶端識別碼和用戶端密碼；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-138">The Azure CLI will return a JSON status message that contains the *appId* and *password*, which you will use later as the client id and client password; for example:</span></span>

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

1. <span data-ttu-id="ff25e-139">在資源群組中建立新的金鑰保存庫；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-139">Create a new key vault in the resource group; for example:</span></span>
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   <span data-ttu-id="ff25e-140">其中：</span><span class="sxs-lookup"><span data-stu-id="ff25e-140">Where:</span></span>
   | <span data-ttu-id="ff25e-141">參數</span><span class="sxs-lookup"><span data-stu-id="ff25e-141">Parameter</span></span> | <span data-ttu-id="ff25e-142">說明</span><span class="sxs-lookup"><span data-stu-id="ff25e-142">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="ff25e-143">指定金鑰保存庫的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="ff25e-143">Specifies a unique name for your key vault.</span></span> |
   | `location` | <span data-ttu-id="ff25e-144">指定要裝載資源群組的 [Azure 區域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="ff25e-144">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |
   | `enabled-for-deployment` | <span data-ttu-id="ff25e-145">指定[金鑰保存庫部署選項](https://docs.microsoft.com/en-us/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="ff25e-145">Specifies the [key vault deployment option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `enabled-for-disk-encryption` | <span data-ttu-id="ff25e-146">指定[金鑰保存庫加密選項](https://docs.microsoft.com/en-us/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="ff25e-146">Specifies the [key vault encryption option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `enabled-for-template-deployment` | <span data-ttu-id="ff25e-147">指定[金鑰保存庫加密選項](https://docs.microsoft.com/en-us/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="ff25e-147">Specifies the [key vault encryption option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `sku` | <span data-ttu-id="ff25e-148">指定[金鑰保存庫 SKU 選項](https://docs.microsoft.com/en-us/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="ff25e-148">Specifies the [key vault SKU option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `query` | <span data-ttu-id="ff25e-149">指定要從回應擷取的值，也就是要完成本教學課程所必須擁有的金鑰保存庫 URI。</span><span class="sxs-lookup"><span data-stu-id="ff25e-149">Specifies a value to retrieve from the response, which is the key vault URI that you will need to complete this tutorial.</span></span> |

   <span data-ttu-id="ff25e-150">Azure CLI 會顯示您稍候會用到的金鑰保存庫 URI；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-150">The Azure CLI will display the URI for key vault, which you will use later; for example:</span></span>  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

1. <span data-ttu-id="ff25e-151">設定您稍早建立之 Azure 服務主體的存取原則；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-151">Set the access policy for the Azure service principal you created earlier; for example:</span></span>
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   <span data-ttu-id="ff25e-152">其中：</span><span class="sxs-lookup"><span data-stu-id="ff25e-152">Where:</span></span>
   | <span data-ttu-id="ff25e-153">參數</span><span class="sxs-lookup"><span data-stu-id="ff25e-153">Parameter</span></span> | <span data-ttu-id="ff25e-154">說明</span><span class="sxs-lookup"><span data-stu-id="ff25e-154">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="ff25e-155">指定您稍早取得的金鑰保存庫名稱。</span><span class="sxs-lookup"><span data-stu-id="ff25e-155">Specifies your key vault name from earlier.</span></span> |
   | `secret-permission` | <span data-ttu-id="ff25e-156">指定金鑰保存庫的[安全性原則](https://docs.microsoft.com/en-us/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="ff25e-156">Specifies the [security policies](https://docs.microsoft.com/en-us/cli/azure/keyvault) for your key vault.</span></span> |
   | `object-id` | <span data-ttu-id="ff25e-157">指定您稍早取得的應用程式註冊 GUID。</span><span class="sxs-lookup"><span data-stu-id="ff25e-157">Specifies the GUID for your application registration from earlier.</span></span> |

   <span data-ttu-id="ff25e-158">Azure CLI 會顯示安全性原則的建立結果；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-158">The Azure CLI will display the results of your security policy creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "location": "westus",
     "name": "wingtiptoyskeyvault",
     "properties": {
       ...
       ... (A long list of values will be displayed here.)
       ...
     },
     "resourceGroup": "wingtiptoysresources",
     "tags": {},
     "type": "Microsoft.KeyVault/vaults"
   }
   ```

1. <span data-ttu-id="ff25e-159">將祕密儲存在您的新金鑰保存庫中；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-159">Store a secret in your new key vault; for example:</span></span>
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   <span data-ttu-id="ff25e-160">其中：</span><span class="sxs-lookup"><span data-stu-id="ff25e-160">Where:</span></span>
   | <span data-ttu-id="ff25e-161">參數</span><span class="sxs-lookup"><span data-stu-id="ff25e-161">Parameter</span></span> | <span data-ttu-id="ff25e-162">說明</span><span class="sxs-lookup"><span data-stu-id="ff25e-162">Description</span></span> |
   |---|---|
   | `vault-name` | <span data-ttu-id="ff25e-163">指定您稍早取得的金鑰保存庫名稱。</span><span class="sxs-lookup"><span data-stu-id="ff25e-163">Specifies your key vault name from earlier.</span></span> |
   | `name` | <span data-ttu-id="ff25e-164">指定祕密的名稱。</span><span class="sxs-lookup"><span data-stu-id="ff25e-164">Specifies the name of your secret.</span></span> |
   | `value` | <span data-ttu-id="ff25e-165">指定祕密的值。</span><span class="sxs-lookup"><span data-stu-id="ff25e-165">Specifies the value of your secret.</span></span> |

   <span data-ttu-id="ff25e-166">Azure CLI 會顯示祕密的建立結果；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-166">The Azure CLI will display the results of your secret creation; for example:</span></span>  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://wingtiptoyskeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://wingtiptoys.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="ff25e-167">設定及編譯 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="ff25e-167">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="ff25e-168">從您稍早下載至目錄的 Spring Boot 專案封存檔解壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="ff25e-168">Extract the files from the Spring Boot project archive files that you downloaded earlier into a directory.</span></span>

1. <span data-ttu-id="ff25e-169">瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="ff25e-169">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="ff25e-170">使用您稍早在本教學課程中完成步驟所得到的值，為金鑰保存庫新增值；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-170">Add the values for your key vault using values from the steps that you completed earlier in this tutorial; for example:</span></span>
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   <span data-ttu-id="ff25e-171">其中：</span><span class="sxs-lookup"><span data-stu-id="ff25e-171">Where:</span></span>
   | <span data-ttu-id="ff25e-172">參數</span><span class="sxs-lookup"><span data-stu-id="ff25e-172">Parameter</span></span> | <span data-ttu-id="ff25e-173">說明</span><span class="sxs-lookup"><span data-stu-id="ff25e-173">Description</span></span> |
   |---|---|
   | `azure.keyvault.uri` | <span data-ttu-id="ff25e-174">指定您在建立金鑰保存庫時所得到的 URI。</span><span class="sxs-lookup"><span data-stu-id="ff25e-174">Specifies the URI from when you created your key vault.</span></span> |
   | `azure.keyvault.client-id` | <span data-ttu-id="ff25e-175">指定您在建立服務主體時所得到的 appId GUID。</span><span class="sxs-lookup"><span data-stu-id="ff25e-175">Specifies the *appId* GUID from when you created your service principal.</span></span> |
   | `azure.keyvault.client-key` | <span data-ttu-id="ff25e-176">指定您在建立服務主體時所得到的 password GUID。</span><span class="sxs-lookup"><span data-stu-id="ff25e-176">Specifies the *password* GUID from when you created your service principal.</span></span> |

1. <span data-ttu-id="ff25e-177">瀏覽至專案的主要原始程式碼檔案；例如：/src/main/java/com/wingtiptoys/secrets。</span><span class="sxs-lookup"><span data-stu-id="ff25e-177">Navigate to the main source code file of your project; for example: */src/main/java/com/wingtiptoys/secrets*.</span></span>

1. <span data-ttu-id="ff25e-178">在文字編輯器中開啟應用程式的主要 Java 檔案；例如 SecretsApplication.java，並在該檔案中新增下列幾行：</span><span class="sxs-lookup"><span data-stu-id="ff25e-178">Open the application's main Java file in a file in a text editor; for example: *SecretsApplication.java*, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }

      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   <span data-ttu-id="ff25e-179">此程式碼範例會從金鑰保存庫擷取連接字串，並將它顯示在命令列中。</span><span class="sxs-lookup"><span data-stu-id="ff25e-179">This code example retrieves the connection string from the key vault and displays it to the command line.</span></span>

1. <span data-ttu-id="ff25e-180">儲存並關閉 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="ff25e-180">Save and close the Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="ff25e-181">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="ff25e-181">Build and test your app</span></span>

1. <span data-ttu-id="ff25e-182">瀏覽至 Spring Boot 應用程式的 pom.xml 檔案所在的目錄：</span><span class="sxs-lookup"><span data-stu-id="ff25e-182">Navigate to the directory where the *pom.xml* file for your Spring Boot app is located:</span></span>

1. <span data-ttu-id="ff25e-183">使用 Maven 建置 Spring Boot 應用程式；例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-183">Build your Spring Boot application with Maven; for example:</span></span>

   ```bash
   mvn clean package
   ```

   <span data-ttu-id="ff25e-184">Maven 將會顯示建置的結果。</span><span class="sxs-lookup"><span data-stu-id="ff25e-184">Maven will display the results of your build.</span></span>

   ![Spring Boot 應用程式建置狀態][build-application-01]

1. <span data-ttu-id="ff25e-186">使用 Maven 執行 Spring Boot 應用程式；應用程式會顯示從金鑰保存庫取得的連接字串。</span><span class="sxs-lookup"><span data-stu-id="ff25e-186">Run your Spring Boot application with Maven; the application will display the connection string from your key vault.</span></span> <span data-ttu-id="ff25e-187">例如：</span><span class="sxs-lookup"><span data-stu-id="ff25e-187">For example:</span></span>

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot 執行階段訊息][build-application-02]

## <a name="next-steps"></a><span data-ttu-id="ff25e-189">後續步驟</span><span class="sxs-lookup"><span data-stu-id="ff25e-189">Next steps</span></span>

<span data-ttu-id="ff25e-190">如需有關使用 Azure Key Vault 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="ff25e-190">For more information about using Azure Key Vaults, see the following articles:</span></span>

* <span data-ttu-id="ff25e-191">[Key Vault 文件]。</span><span class="sxs-lookup"><span data-stu-id="ff25e-191">[Key Vault Documentation].</span></span>

* <span data-ttu-id="ff25e-192">[開始使用 Azure 金鑰保存庫]</span><span class="sxs-lookup"><span data-stu-id="ff25e-192">[Get started with Azure Key Vault]</span></span>

<span data-ttu-id="ff25e-193">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="ff25e-193">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="ff25e-194">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ff25e-194">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="ff25e-195">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="ff25e-195">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="ff25e-196">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="ff25e-196">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Key Vault 文件]: /azure/key-vault/
[開始使用 Azure 金鑰保存庫]: /azure/key-vault/key-vault-get-started
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png