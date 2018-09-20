---
title: 如何對 Azure Key Vault 使用 Spring Boot Starter
description: 了解如何使用 Azure Key Vault Starter 來設定 Spring Boot Initializer 應用程式。
services: key-vault
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 78b7a9a2e26168b19dc8a1d12e47456752b57ffc
ms.sourcegitcommit: e017de4677c5bedd6ef88c8c1b6da279dc973efe
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/15/2018
ms.locfileid: "45639771"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a><span data-ttu-id="89dae-103">如何對 Azure Key Vault 使用 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="89dae-103">How to use the Spring Boot Starter for Azure Key Vault</span></span>

## <a name="overview"></a><span data-ttu-id="89dae-104">概觀</span><span class="sxs-lookup"><span data-stu-id="89dae-104">Overview</span></span>

<span data-ttu-id="89dae-105">本文示範如何使用 **[Spring Initializr]** 建立應用程式，Spring Initializr 會使用適用於 Azure Key Vault 的 Spring Boot Starter，來擷取在金鑰保存庫中儲存作為祕密的連接字串。</span><span class="sxs-lookup"><span data-stu-id="89dae-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Key Vault to retrieve a connection string that is stored as a secret in a key vault.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89dae-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="89dae-106">Prerequisites</span></span>

<span data-ttu-id="89dae-107">請務必具備下列必要條件，以便本文中說明的步驟：</span><span class="sxs-lookup"><span data-stu-id="89dae-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="89dae-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="89dae-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="89dae-109">[Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="89dae-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="89dae-110">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="89dae-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-the-spring-initialzr"></a><span data-ttu-id="89dae-111">使用 Spring Initialzr 建立應用程式</span><span class="sxs-lookup"><span data-stu-id="89dae-111">Create an app using the Spring Initialzr</span></span>

1. <span data-ttu-id="89dae-112">瀏覽至 <https://start.spring.io/> 。</span><span class="sxs-lookup"><span data-stu-id="89dae-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="89dae-113">指定您想要使用 [Java] 產生 [Maven 專案]、輸入應用程式的 [群組] 和 [成品] 名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。</span><span class="sxs-lookup"><span data-stu-id="89dae-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定群組和成品名稱][secrets-01]

1. <span data-ttu-id="89dae-115">向下捲動至 [Azure] 區段，然後核取 [Azure Key Vault] 的方塊。</span><span class="sxs-lookup"><span data-stu-id="89dae-115">Scroll down to the **Azure** section and check the box for **Azure Key Vault**.</span></span>

   ![選取 Azure Key Vault Starter][secrets-02]

1. <span data-ttu-id="89dae-117">捲動到頁面底部，然後按一下按鈕以**產生專案**。</span><span class="sxs-lookup"><span data-stu-id="89dae-117">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![產生 Spring Boot 專案][secrets-03]

1. <span data-ttu-id="89dae-119">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="89dae-119">When prompted, download the project to a path on your local computer.</span></span>

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="89dae-120">登入 Azure，並選取要使用的訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="89dae-120">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="89dae-121">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="89dae-121">Open a command prompt.</span></span>

1. <span data-ttu-id="89dae-122">使用 Azure CLI 登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="89dae-122">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="89dae-123">依照指示完成登入程序。</span><span class="sxs-lookup"><span data-stu-id="89dae-123">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="89dae-124">列出您的訂用帳戶：</span><span class="sxs-lookup"><span data-stu-id="89dae-124">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="89dae-125">Azure 會傳回訂用帳戶清單，而且您必須複製所要使用之訂用帳戶的 GUID；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-125">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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
   ```

1. <span data-ttu-id="89dae-126">指定您希望在 Azure 中使用的帳戶 GUID，例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-126">Specify the GUID for the account you want to use with Azure; for example:</span></span>

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-and-configure-a-new-azure-key-vault-using-the-azure-cli"></a><span data-ttu-id="89dae-127">使用 Azure CLI 建立和設定新的 Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="89dae-127">Create and configure a new Azure Key Vault using the Azure CLI</span></span>

1. <span data-ttu-id="89dae-128">為將要用於金鑰保存庫的 Azure 資源建立資源群組；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-128">Create a resource group for the Azure resources you will use for your key vault; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="89dae-129">其中：</span><span class="sxs-lookup"><span data-stu-id="89dae-129">Where:</span></span>

   | <span data-ttu-id="89dae-130">參數</span><span class="sxs-lookup"><span data-stu-id="89dae-130">Parameter</span></span> | <span data-ttu-id="89dae-131">說明</span><span class="sxs-lookup"><span data-stu-id="89dae-131">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="89dae-132">指定資源群組的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="89dae-132">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="89dae-133">指定要裝載資源群組的 [Azure 區域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="89dae-133">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="89dae-134">Azure CLI 將會顯示建立資源群組的結果，例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-134">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

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

2. <span data-ttu-id="89dae-135">從應用程式註冊建立 Azure 服務主體；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-135">Create an Azure service principal from your application registration; for example:</span></span>
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   <span data-ttu-id="89dae-136">其中：</span><span class="sxs-lookup"><span data-stu-id="89dae-136">Where:</span></span>

   | <span data-ttu-id="89dae-137">參數</span><span class="sxs-lookup"><span data-stu-id="89dae-137">Parameter</span></span> | <span data-ttu-id="89dae-138">說明</span><span class="sxs-lookup"><span data-stu-id="89dae-138">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="89dae-139">指定 Azure 服務主體的名稱。</span><span class="sxs-lookup"><span data-stu-id="89dae-139">Specifies the name for your Azure service principal.</span></span> |

   <span data-ttu-id="89dae-140">Azure CLI 會傳回 JSON 狀態訊息，其中包含 appId 和 password，可供您稍候用來作為用戶端識別碼和用戶端密碼；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-140">The Azure CLI will return a JSON status message that contains the *appId* and *password*, which you will use later as the client id and client password; for example:</span></span>

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

3. <span data-ttu-id="89dae-141">在資源群組中建立新的金鑰保存庫；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-141">Create a new key vault in the resource group; for example:</span></span>
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   <span data-ttu-id="89dae-142">其中：</span><span class="sxs-lookup"><span data-stu-id="89dae-142">Where:</span></span>

   | <span data-ttu-id="89dae-143">參數</span><span class="sxs-lookup"><span data-stu-id="89dae-143">Parameter</span></span> | <span data-ttu-id="89dae-144">說明</span><span class="sxs-lookup"><span data-stu-id="89dae-144">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="89dae-145">指定金鑰保存庫的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="89dae-145">Specifies a unique name for your key vault.</span></span> |
   | `location` | <span data-ttu-id="89dae-146">指定要裝載資源群組的 [Azure 區域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="89dae-146">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |
   | `enabled-for-deployment` | <span data-ttu-id="89dae-147">指定[金鑰保存庫部署選項](https://docs.microsoft.com/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="89dae-147">Specifies the [key vault deployment option](https://docs.microsoft.com/cli/azure/keyvault).</span></span> |
   | `enabled-for-disk-encryption` | <span data-ttu-id="89dae-148">指定[金鑰保存庫加密選項](https://docs.microsoft.com/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="89dae-148">Specifies the [key vault encryption option](https://docs.microsoft.com/cli/azure/keyvault).</span></span> |
   | `enabled-for-template-deployment` | <span data-ttu-id="89dae-149">指定[金鑰保存庫加密選項](https://docs.microsoft.com/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="89dae-149">Specifies the [key vault encryption option](https://docs.microsoft.com/cli/azure/keyvault).</span></span> |
   | `sku` | <span data-ttu-id="89dae-150">指定[金鑰保存庫 SKU 選項](https://docs.microsoft.com/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="89dae-150">Specifies the [key vault SKU option](https://docs.microsoft.com/cli/azure/keyvault).</span></span> |
   | `query` | <span data-ttu-id="89dae-151">指定要從回應擷取的值，也就是要完成本教學課程所必須擁有的金鑰保存庫 URI。</span><span class="sxs-lookup"><span data-stu-id="89dae-151">Specifies a value to retrieve from the response, which is the key vault URI that you will need to complete this tutorial.</span></span> |

   <span data-ttu-id="89dae-152">Azure CLI 會顯示您稍候會用到的金鑰保存庫 URI；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-152">The Azure CLI will display the URI for key vault, which you will use later; for example:</span></span>  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

4. <span data-ttu-id="89dae-153">設定您稍早建立之 Azure 服務主體的存取原則；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-153">Set the access policy for the Azure service principal you created earlier; for example:</span></span>
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   <span data-ttu-id="89dae-154">其中：</span><span class="sxs-lookup"><span data-stu-id="89dae-154">Where:</span></span>

   | <span data-ttu-id="89dae-155">參數</span><span class="sxs-lookup"><span data-stu-id="89dae-155">Parameter</span></span> | <span data-ttu-id="89dae-156">說明</span><span class="sxs-lookup"><span data-stu-id="89dae-156">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="89dae-157">指定您稍早取得的金鑰保存庫名稱。</span><span class="sxs-lookup"><span data-stu-id="89dae-157">Specifies your key vault name from earlier.</span></span> |
   | `secret-permission` | <span data-ttu-id="89dae-158">指定金鑰保存庫的[安全性原則](https://docs.microsoft.com/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="89dae-158">Specifies the [security policies](https://docs.microsoft.com/cli/azure/keyvault) for your key vault.</span></span> |
   | `spn` | <span data-ttu-id="89dae-159">指定您稍早取得的應用程式註冊 GUID。</span><span class="sxs-lookup"><span data-stu-id="89dae-159">Specifies the GUID for your application registration from earlier.</span></span> |

   <span data-ttu-id="89dae-160">Azure CLI 會顯示安全性原則的建立結果；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-160">The Azure CLI will display the results of your security policy creation; for example:</span></span>  

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

5. <span data-ttu-id="89dae-161">將祕密儲存在您的新金鑰保存庫中；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-161">Store a secret in your new key vault; for example:</span></span>
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   <span data-ttu-id="89dae-162">其中：</span><span class="sxs-lookup"><span data-stu-id="89dae-162">Where:</span></span>

   | <span data-ttu-id="89dae-163">參數</span><span class="sxs-lookup"><span data-stu-id="89dae-163">Parameter</span></span> | <span data-ttu-id="89dae-164">說明</span><span class="sxs-lookup"><span data-stu-id="89dae-164">Description</span></span> |
   |---|---|
   | `vault-name` | <span data-ttu-id="89dae-165">指定您稍早取得的金鑰保存庫名稱。</span><span class="sxs-lookup"><span data-stu-id="89dae-165">Specifies your key vault name from earlier.</span></span> |
   | `name` | <span data-ttu-id="89dae-166">指定祕密的名稱。</span><span class="sxs-lookup"><span data-stu-id="89dae-166">Specifies the name of your secret.</span></span> |
   | `value` | <span data-ttu-id="89dae-167">指定祕密的值。</span><span class="sxs-lookup"><span data-stu-id="89dae-167">Specifies the value of your secret.</span></span> |

   <span data-ttu-id="89dae-168">Azure CLI 會顯示祕密的建立結果；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-168">The Azure CLI will display the results of your secret creation; for example:</span></span>  

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

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="89dae-169">設定及編譯 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="89dae-169">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="89dae-170">從您稍早下載至目錄的 Spring Boot 專案封存檔解壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="89dae-170">Extract the files from the Spring Boot project archive files that you downloaded earlier into a directory.</span></span>

2. <span data-ttu-id="89dae-171">瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。</span><span class="sxs-lookup"><span data-stu-id="89dae-171">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

3. <span data-ttu-id="89dae-172">使用您稍早在本教學課程中完成步驟所得到的值，為金鑰保存庫新增值；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-172">Add the values for your key vault using values from the steps that you completed earlier in this tutorial; for example:</span></span>
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   <span data-ttu-id="89dae-173">其中：</span><span class="sxs-lookup"><span data-stu-id="89dae-173">Where:</span></span>

   |          <span data-ttu-id="89dae-174">參數</span><span class="sxs-lookup"><span data-stu-id="89dae-174">Parameter</span></span>          |                                 <span data-ttu-id="89dae-175">說明</span><span class="sxs-lookup"><span data-stu-id="89dae-175">Description</span></span>                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           <span data-ttu-id="89dae-176">指定您在建立金鑰保存庫時所得到的 URI。</span><span class="sxs-lookup"><span data-stu-id="89dae-176">Specifies the URI from when you created your key vault.</span></span>           |
   | `azure.keyvault.client-id`  |  <span data-ttu-id="89dae-177">指定您在建立服務主體時所得到的 appId GUID。</span><span class="sxs-lookup"><span data-stu-id="89dae-177">Specifies the *appId* GUID from when you created your service principal.</span></span>   |
   | `azure.keyvault.client-key` | <span data-ttu-id="89dae-178">指定您在建立服務主體時所得到的 password GUID。</span><span class="sxs-lookup"><span data-stu-id="89dae-178">Specifies the *password* GUID from when you created your service principal.</span></span> |


4. <span data-ttu-id="89dae-179">瀏覽至專案的主要原始程式碼檔案；例如：/src/main/java/com/wingtiptoys/secrets。</span><span class="sxs-lookup"><span data-stu-id="89dae-179">Navigate to the main source code file of your project; for example: */src/main/java/com/wingtiptoys/secrets*.</span></span>

5. <span data-ttu-id="89dae-180">在文字編輯器中開啟應用程式的主要 Java 檔案；例如 SecretsApplication.java，並在該檔案中新增下列幾行：</span><span class="sxs-lookup"><span data-stu-id="89dae-180">Open the application's main Java file in a file in a text editor; for example: *SecretsApplication.java*, and add the following lines to the file:</span></span>

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
   <span data-ttu-id="89dae-181">此程式碼範例會從金鑰保存庫擷取連接字串，並將它顯示在命令列中。</span><span class="sxs-lookup"><span data-stu-id="89dae-181">This code example retrieves the connection string from the key vault and displays it to the command line.</span></span>

6. <span data-ttu-id="89dae-182">儲存並關閉 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="89dae-182">Save and close the Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="89dae-183">建置及測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="89dae-183">Build and test your app</span></span>

1. <span data-ttu-id="89dae-184">瀏覽至 Spring Boot 應用程式的 pom.xml 檔案所在的目錄：</span><span class="sxs-lookup"><span data-stu-id="89dae-184">Navigate to the directory where the *pom.xml* file for your Spring Boot app is located:</span></span>

1. <span data-ttu-id="89dae-185">使用 Maven 建置 Spring Boot 應用程式；例如：</span><span class="sxs-lookup"><span data-stu-id="89dae-185">Build your Spring Boot application with Maven; for example:</span></span>

   ```bash
   mvn clean package
   ```

   <span data-ttu-id="89dae-186">Maven 將會顯示建置的結果。</span><span class="sxs-lookup"><span data-stu-id="89dae-186">Maven will display the results of your build.</span></span>

   ![Spring Boot 應用程式建置狀態][build-application-01]

1. <span data-ttu-id="89dae-188">使用 Maven 執行 Spring Boot 應用程式；應用程式會顯示從金鑰保存庫取得的連接字串。</span><span class="sxs-lookup"><span data-stu-id="89dae-188">Run your Spring Boot application with Maven; the application will display the connection string from your key vault.</span></span> <span data-ttu-id="89dae-189">例如︰</span><span class="sxs-lookup"><span data-stu-id="89dae-189">For example:</span></span>

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot 執行階段訊息][build-application-02]

## <a name="next-steps"></a><span data-ttu-id="89dae-191">後續步驟</span><span class="sxs-lookup"><span data-stu-id="89dae-191">Next steps</span></span>

<span data-ttu-id="89dae-192">如需有關使用 Azure Key Vault 的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="89dae-192">For more information about using Azure Key Vaults, see the following articles:</span></span>

* <span data-ttu-id="89dae-193">[Key Vault 文件]。</span><span class="sxs-lookup"><span data-stu-id="89dae-193">[Key Vault Documentation].</span></span>

* <span data-ttu-id="89dae-194">[開始使用 Azure 金鑰保存庫]</span><span class="sxs-lookup"><span data-stu-id="89dae-194">[Get started with Azure Key Vault]</span></span>

<span data-ttu-id="89dae-195">如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="89dae-195">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="89dae-196">將 Spring Boot 應用程式部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="89dae-196">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="89dae-197">在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="89dae-197">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="89dae-198">如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="89dae-198">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Key Vault 文件]: /azure/key-vault/
[Key Vault Documentation]: /azure/key-vault/
[開始使用 Azure 金鑰保存庫]: /azure/key-vault/key-vault-get-started
[Get started with Azure Key Vault]: /azure/key-vault/key-vault-get-started
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

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
