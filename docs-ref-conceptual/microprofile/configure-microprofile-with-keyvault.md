---
title: 使用 Azure Key Vault 設定 MicroProfile
description: 了解如何使用 Azure Key Vault 將祕密插入 MicroProfile Web 服務中
services: key-vault
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: c405711813176823f2ddee6b4002f75c2b25fdb5
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533617"
---
# <a name="configure-microprofile-by-using-azure-key-vault"></a>使用 Azure Key Vault 設定 MicroProfile

本文示範如何設定 [MicroProfile](http://microprofile.io) 應用程式以從 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 擷取祕密。 在此程序中，您將使用 [MicroProfile Config API](https://microprofile.io/project/eclipse/microprofile-config) 建立對 Azure Key Vault 的直接連線。 身為開發人員，藉由使用 MicroProfile Config API，您將可利用標準 API 將組態資料擷取和插入至其微服務。

在開始之前，請先快速了解 Azure Key Vault 和 MicroProfile Config API 的組合能讓您在程式碼中寫入哪些資訊。 以下是某欄位的程式碼片段，且其所屬類別標註了 `@Inject` 和 `@ConfigProperty`。 註釋中指定的 *name* 是要在您的金鑰保存庫中查閱的屬性名稱，而 *defaultValue* 則是找不到金鑰時所將設定的值。 其結果是，儲存在金鑰保存庫中的值或預設值，會在執行階段自動插入欄位中。 此動作可以簡化您的工作，因為您不再需要在建構函式和 setter 方法中傳遞值。 MicroProfile 會為您處理這項工作。

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

若要要求祕密 (如有必要)，您也可以直接存取 MicroProfile 組態。 例如︰

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

此範例程式碼使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile](https://microprofile.io/) 來建立您可在本機電腦上執行的小型 Java Web 應用程式需求 (WAR) 檔案。 其中不會示範如何轉換為 Docker 或將程式碼推送至 Azure，但本教學課程結尾處附有連結，會連結至說明此操作的其他教學課程。

此範例使用會在您的金鑰保存庫中建立組態來源 (使用 MicroProfile Config API) 的免費開放原始碼程式庫。 若要深入了解此程式庫及檢閱程式碼，請參閱[專案 GitHub 頁面](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)。 如果您使用此程式庫，本教學課程中的程式碼就可以只專注於此程式庫的組態，然後將金鑰插入您的程式碼中。 您不需要撰寫任何 Azure 特定程式碼。

若要在本機電腦上執行此程式碼，請依照前幾節的指示，從建立金鑰保存庫資源開始。

## <a name="create-a-key-vault-resource"></a>建立金鑰保存庫資源

在本節中，您將使用 Azure CLI 來建立金鑰保存庫資源，並在其中填入祕密。

1. 建立 Azure 服務主體。 此步驟會為您提供存取金鑰保存庫所需的用戶端識別碼和金鑰：

    ```bash
    az login
    az account set --subscription <subscription_id>

    az ad sp create-for-rbac --name <service_principal_name>
    ```

    我們將使用 *microprofile-keyvault-service-principal* 來取代前述步驟中的 *\<service_principal_name>* 。 Azure 的回應會類似於下列內容：

    ```json
    {
      "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
      "displayName": "microprofile-keyvault-service-principal",
      "name": "http://microprofile-keyvault-service-principal",
      "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
      "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
    }
    ```

    在此應特別注意的是 *appId* 和 *password* 值。 在本文稍後的步驟中，您會將其作為*用戶端識別碼*和*金鑰*。

1. (選擇性) 現在您已建立服務主體，接下來可以建立資源群組。 如果您已有想要使用的資源群組，可以略過此步驟。 若要取得資源群組位置清單，您可以呼叫 `az account list-locations`，並使用該清單中的 *name* 值來指定應建立資源群組位置。

    ```bash
    # In this tutorial, "westus" is the resource group location
    # and "jg-test" is the resource group name.
    az group create -l <resource_group_location> -n <resource_group_name>
    ```

1. 建立金鑰保存庫資源。 稍後您將使用金鑰保存庫名稱來參考金鑰保存庫，因此請務必選擇好記的名稱。

    ```bash
    az keyvault create --name <your_keyvault_name>            \
                      --resource-group <your_resource_group> \
                      --location <location>                  \
                      --enabled-for-deployment true          \
                      --enabled-for-disk-encryption true     \
                      --enabled-for-template-deployment true \
                      --sku standard
    ```

1. 請將適當的權限授與您先前建立的服務主體，使其可以存取金鑰保存庫祕密。 下列程式碼中的 appId 值，是您在步驟 1 中建立服務主體時所產生的 *appId* 值。 也就是說，步驟 1 中的 *appId* 為 *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*，但您應使用您自己的終端機輸出的值。

    ```bash
    az keyvault set-policy --name <your_keyvault_name>   \
                          --secret-permission get list  \
                          --spn <your_sp_appId_created_in_step1>
    ```

1. 現在，您可以將祕密推送至您的金鑰保存庫。 請使用金鑰名稱 *demo-key*，並將金鑰的值設為 *demo-value*：

    ```bash
    az keyvault secret set --name demo-key      \
                           --value demo-value   \
                           --vault-name <your_keyvault_name>  
    ```

就這麼簡單！ 現在，您已有使用單一祕密在 Azure 中執行的金鑰保存庫。 接下來您可以複製此存放庫，並將其設定為在應用程式中使用此資源。

## <a name="get-up-and-running-locally"></a>在本機啟動並執行

此範例會以 GitHub 上的可用範例應用程式為基礎，因此您會先複製該應用程式，然後再逐步執行程式碼。 

1. 輸入下列命令，將程式碼複製到您的電腦上：

    `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

    `cd microprofile-configsource-keyvault`

1. 移至 *src/main/resources/META-INF/microprofile-config.properties*，然後使用先前步驟中的值變更 *microprofile-config.properties* 檔案中的屬性。

1. 嘗試使用 `mvn clean package payara-micro:start` 來執行伺服器。

1. 嘗試在網頁瀏覽器中存取 [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config)。 您應該會看到簡單的回應，其中顯示從您的金鑰保存庫讀取的值。

## <a name="summary"></a>總結

此範例應用程式結合了 MicroProfile Config API、Azure Key Vault 和免費的 [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) 開放原始碼程式庫，讓您可以輕鬆地將組態資料和祕密插入 MicroProfile Web 服務中。
