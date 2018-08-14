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
ms.openlocfilehash: a2734fc08f2f59f64ba6c6c20ff18d75070b68d5
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090711"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a>如何對 Azure Key Vault 使用 Spring Boot Starter

## <a name="overview"></a>概觀

本文示範如何使用 **[Spring Initializr]** 建立應用程式，Spring Initializr 會使用適用於 Azure Key Vault 的 Spring Boot Starter，來擷取在金鑰保存庫中儲存作為祕密的連接字串。

## <a name="prerequisites"></a>先決條件

請務必具備下列必要條件，以便本文中說明的步驟：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* [Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) \(英文\) 1.7 版或更新版本。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。

## <a name="create-an-app-using-the-spring-initialzr"></a>使用 Spring Initialzr 建立應用程式

1. 瀏覽至 <https://start.spring.io/>。

1. 指定您想要使用 [Java] 產生 [Maven 專案]、輸入應用程式的 [群組] 和 [成品] 名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。

   ![指定群組和成品名稱][secrets-01]

1. 向下捲動至 [Azure] 區段，然後核取 [Azure Key Vault] 的方塊。

   ![選取 Azure Key Vault Starter][secrets-02]

1. 捲動到頁面底部，然後按一下按鈕以**產生專案**。

   ![產生 Spring Boot 專案][secrets-03]

1. 出現提示時，將專案下載至本機電腦上的路徑。

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a>登入 Azure，並選取要使用的訂用帳戶

1. 開啟命令提示字元。

1. 使用 Azure CLI 登入您的 Azure 帳戶：

   ```azurecli
   az login
   ```
   依照指示完成登入程序。

1. 列出您的訂用帳戶：

   ```azurecli
   az account list
   ```
   Azure 會傳回訂用帳戶清單，而且您必須複製所要使用之訂用帳戶的 GUID；例如：

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

1. 指定您希望在 Azure 中使用的帳戶 GUID，例如：

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-and-configure-a-new-azure-key-vault-using-the-azure-cli"></a>使用 Azure CLI 建立和設定新的 Azure Key Vault

1. 為將要用於金鑰保存庫的 Azure 資源建立資源群組；例如：
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `name` | 指定資源群組的唯一名稱。 |
   | `location` | 指定要裝載資源群組的 [Azure 區域](https://azure.microsoft.com/regions/)。 |

   Azure CLI 將會顯示建立資源群組的結果，例如：  

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

2. 從應用程式註冊建立 Azure 服務主體；例如：
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `name` | 指定 Azure 服務主體的名稱。 |

   Azure CLI 會傳回 JSON 狀態訊息，其中包含 appId 和 password，可供您稍候用來作為用戶端識別碼和用戶端密碼；例如：

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

3. 在資源群組中建立新的金鑰保存庫；例如：
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `name` | 指定金鑰保存庫的唯一名稱。 |
   | `location` | 指定要裝載資源群組的 [Azure 區域](https://azure.microsoft.com/regions/)。 |
   | `enabled-for-deployment` | 指定[金鑰保存庫部署選項](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `enabled-for-disk-encryption` | 指定[金鑰保存庫加密選項](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `enabled-for-template-deployment` | 指定[金鑰保存庫加密選項](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `sku` | 指定[金鑰保存庫 SKU 選項](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `query` | 指定要從回應擷取的值，也就是要完成本教學課程所必須擁有的金鑰保存庫 URI。 |

   Azure CLI 會顯示您稍候會用到的金鑰保存庫 URI；例如：  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

4. 設定您稍早建立之 Azure 服務主體的存取原則；例如：
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `name` | 指定您稍早取得的金鑰保存庫名稱。 |
   | `secret-permission` | 指定金鑰保存庫的[安全性原則](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `spn` | 指定您稍早取得的應用程式註冊 GUID。 |

   Azure CLI 會顯示安全性原則的建立結果；例如：  

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

5. 將祕密儲存在您的新金鑰保存庫中；例如：
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `vault-name` | 指定您稍早取得的金鑰保存庫名稱。 |
   | `name` | 指定祕密的名稱。 |
   | `value` | 指定祕密的值。 |

   Azure CLI 會顯示祕密的建立結果；例如：  

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

## <a name="configure-and-compile-your-spring-boot-application"></a>設定及編譯 Spring Boot 應用程式

1. 從您稍早下載至目錄的 Spring Boot 專案封存檔解壓縮檔案。

2. 瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。

3. 使用您稍早在本教學課程中完成步驟所得到的值，為金鑰保存庫新增值；例如：
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   其中：

   |          參數          |                                 說明                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           指定您在建立金鑰保存庫時所得到的 URI。           |
   | `azure.keyvault.client-id`  |  指定您在建立服務主體時所得到的 appId GUID。   |
   | `azure.keyvault.client-key` | 指定您在建立服務主體時所得到的 password GUID。 |


4. 瀏覽至專案的主要原始程式碼檔案；例如：/src/main/java/com/wingtiptoys/secrets。

5. 在文字編輯器中開啟應用程式的主要 Java 檔案；例如 SecretsApplication.java，並在該檔案中新增下列幾行：

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
   此程式碼範例會從金鑰保存庫擷取連接字串，並將它顯示在命令列中。

6. 儲存並關閉 Java 檔案。

## <a name="build-and-test-your-app"></a>建置及測試您的應用程式

1. 瀏覽至 Spring Boot 應用程式的 pom.xml 檔案所在的目錄：

1. 使用 Maven 建置 Spring Boot 應用程式；例如：

   ```bash
   mvn clean package
   ```

   Maven 將會顯示建置的結果。

   ![Spring Boot 應用程式建置狀態][build-application-01]

1. 使用 Maven 執行 Spring Boot 應用程式；應用程式會顯示從金鑰保存庫取得的連接字串。 例如︰

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot 執行階段訊息][build-application-02]

## <a name="next-steps"></a>後續步驟

如需有關使用 Azure Key Vault 的詳細資訊，請參閱下列文章：

* [Key Vault 文件]。

* [開始使用 Azure 金鑰保存庫]

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式](deploy-spring-boot-java-app-on-kubernetes.md)

如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。

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
