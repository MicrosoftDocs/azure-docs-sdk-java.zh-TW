---
title: 使用 Azure Key Vault 設定 MicroProfile
description: 了解如何使用 Azure Key Vault 將祕密插入 MicroProfile Web 服務
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
ms.openlocfilehash: fa93788f9b600d963f35ea599a58d309d3a3ee52
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2018
ms.locfileid: "43240821"
---
# <a name="configure-microprofile-with-azure-key-vault"></a>使用 Azure Key Vault 設定 MicroProfile

本教學課程將示範如何設定 [MicroProfile](http://microprofile.io) 應用程式，以使用 [MicroProfile Config API](https://microprofile.io/project/eclipse/microprofile-config) 對 Azure Key Vault 建立直接連線，進而擷取 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 中的祕密。 藉由使用 MicroProfile Config API，開發人員可使用標準 API 將組態資料擷取和插入至其微服務。

在深入探討之前，讓我們來快速看看 Azure Key Vault 和 MicroProfile Config API 的組合能讓我們在程式碼中寫入哪些資訊。 以下欄位程式碼片段位在以 `@Inject` 和 `@ConfigProperty` 加以註解的類別中。 註解中指定的 `name` 是要在 Azure Key Vault 中查閱的屬性名稱，而 `defaultValue` 是找不到金鑰時會設定的值。 最終結果是，儲存在 Azure Key Vault 中的值或預設值將會自動插入執行階段上的欄位中，如此可以簡化開發人員的工作週期，因為他們不再需要以建構函式和 setter 方法傳遞值，而是讓 MicroProfile 來處理。

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

如有必要，您也可以直接存取 MicroProfile 組態來要求祕密，例如：

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

此範例使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile](https://microprofile.io/) 來建立可在您機器本機中執行的小型 Java war 檔案。 其中不會示範如何轉換為 Docker 或將程式碼推送至 Azure，但是在本教學課程結尾處的 [連結] 區段中，有說明此操作的其他教學課程連結。

此範例會使用免費的開放原始碼程式庫為 Azure Key Vault 建立組態來源 (使用 MicroProfile Config API)。 您可以在 [GitHub 專案頁面](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)上深入了解此程式庫及檢閱程式碼。 藉由使用此程式庫，本教學課程中的程式碼可只專注於程式庫設定，然後將金鑰插入您的程式碼中，我們不需要撰寫任何 Azure 特有的程式碼。

以下是在本機電腦上執行此程式碼所需的步驟，我們從建立 Azure Key Vault 資源開始。

## <a name="creating-an-azure-key-vault-resource"></a>新增 Azure Key Vault 資源

我們將使用 Azure CLI 來建立 Azure Key Vault 資源，並填入一個祕密。

1. 首先，建立 Azure 服務主體。 這會提供我們存取 Key Vault 用戶端所需的識別碼和金鑰：

```bash
az login
az account set --subscription <subscription_id>

az ad sp create-for-rbac --name <service_principal_name>
```

假設我們使用 `microprofile-keyvault-service-principal` 作為前一個步驟中的服務主體名稱。 執行此動作後，Azure 的回應格式會如下所示 (已稍微刪改)：

```json
{
  "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
  "displayName": "microprofile-keyvault-service-principal",
  "name": "http://microprofile-keyvault-service-principal",
  "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
  "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
}
```

在此要特別注意的是 `appID` 和 `password` 值 - 我們將在之後的 `client ID` 和 `key` 上分別使用這兩個值。

現在，我們已建立了服務主體，且可以選擇性地建立資源群組 (如果您已經有想用的資源群組，可以略過此步驟)。 請注意，若要取得資源群組位置清單，您可以呼叫 `az account list-locations`，並使用該清單中的 `name` 值來指定應建立資源群組位置。

```bash
# For this tutorial, the author chose to use `westus`
# and `jg-test` for the resource group name.
az group create -l <resource_group_location> -n <resource_group_name>
```

我們現在來建立 Azure Key Vault 資源。 請注意，Key Vault 名稱是之後要用來參考金鑰保存庫的名稱，因此，請選擇令人印象深刻的名稱。

```bash
az keyvault create --name <your_keyvault_name>            \
                   --resource-group <your_resource_group> \
                   --location <location>                  \
                   --enabled-for-deployment true          \
                   --enabled-for-disk-encryption true     \
                   --enabled-for-template-deployment true \
                   --sku standard
```

我們也需要將適當的權限授與稍早建立的服務主體，使其可以存取 Key Vault 祕密。 請注意，appID 值是我們剛才建立服務主體所在位置中的 `appId` (也就是 `5292398e-XXXX-40ce-XXXX-d49fXXXX9e79` - 但請使用您終端機輸出中的值)。

```bash
az keyvault set-policy --name <your_keyvault_name>   \
                       --secret-permission get list  \
                       --spn <your_sp_appId_created_in_step1>
```

現在，我們已經可以將祕密推送至 Key Vault 了。 讓我們使用 `demo-key` 作為金鑰名稱，並將金鑰值設為 `demo-value`：

```bash
az keyvault secret set --name demo-key      \
                       --value demo-value   \
                       --vault-name <your_keyvault_name>  
```

就這麼簡單！ 現在，具有單一祕密的 Key Vault 正在 Azure 中執行。 我們現在可以複製此存放庫，並將它設定為在應用程式中使用此資源。

## <a name="getting-up-and-running-locally"></a>啟動並在本機執行

此範例會以 GitHub 上的可用範例應用程式為基礎，因此我們會先將其複製，然後逐步執行程式碼。 請遵循下列步驟，將程式碼複製到您的機器上：

1. `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

1. `cd microprofile-configsource-keyvault`

1. 瀏覽至 `src/main/resources/META-INF/microprofile-config.properties`，並將 microprofile-config.properties 檔案中的屬性變更為上述的詳細資料。

1. 請嘗試使用 `mvn clean package payara-micro:start` 來執行伺服器

1. 嘗試在 Web 瀏覽器中存取 [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) - 您應該會看到簡單的回應，其中顯示從 Azure Key Vault 所讀取的值。

## <a name="summary"></a>總結

此範例應用程式結合 MicroProfile Config API、Azure Key Vault 和免費的 [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) 開放原始碼程式庫，讓您可以輕鬆地將組態資料和祕密插入我們的 MicroProfile Web 服務。
