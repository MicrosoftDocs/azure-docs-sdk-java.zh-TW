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
# <a name="configure-microprofile-with-azure-key-vault"></a><span data-ttu-id="fabc2-103">使用 Azure Key Vault 設定 MicroProfile</span><span class="sxs-lookup"><span data-stu-id="fabc2-103">Configure MicroProfile with Azure Key Vault</span></span>

<span data-ttu-id="fabc2-104">本教學課程將示範如何設定 [MicroProfile](http://microprofile.io) 應用程式，以使用 [MicroProfile Config API](https://microprofile.io/project/eclipse/microprofile-config) 對 Azure Key Vault 建立直接連線，進而擷取 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 中的祕密。</span><span class="sxs-lookup"><span data-stu-id="fabc2-104">This tutorial will demonstrate how to configure a [MicroProfile](http://microprofile.io) application to retrieve secrets from [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) using the [MicroProfile Config APIs](https://microprofile.io/project/eclipse/microprofile-config) to create a direct connection to Azure Key Vault.</span></span> <span data-ttu-id="fabc2-105">藉由使用 MicroProfile Config API，開發人員可使用標準 API 將組態資料擷取和插入至其微服務。</span><span class="sxs-lookup"><span data-stu-id="fabc2-105">By using the MicroProfile Config APIs, developers benefit from a standard API for retrieving and injecting configuration data into their microservices.</span></span>

<span data-ttu-id="fabc2-106">在深入探討之前，讓我們來快速看看 Azure Key Vault 和 MicroProfile Config API 的組合能讓我們在程式碼中寫入哪些資訊。</span><span class="sxs-lookup"><span data-stu-id="fabc2-106">Before we dive in, lets quickly take a look at what a combination of Azure Key Vault and the MicroProfile Config API enables us to write in our code.</span></span> <span data-ttu-id="fabc2-107">以下欄位程式碼片段位在以 `@Inject` 和 `@ConfigProperty` 加以註解的類別中。</span><span class="sxs-lookup"><span data-stu-id="fabc2-107">Here's a code snippet of a field in a class that has been annotated with `@Inject` and `@ConfigProperty`.</span></span> <span data-ttu-id="fabc2-108">註解中指定的 `name` 是要在 Azure Key Vault 中查閱的屬性名稱，而 `defaultValue` 是找不到金鑰時會設定的值。</span><span class="sxs-lookup"><span data-stu-id="fabc2-108">The `name` specified in the annotation is the name of the property to look up in Azure Key Vault, and the `defaultValue` is what will be set if the key is not discovered.</span></span> <span data-ttu-id="fabc2-109">最終結果是，儲存在 Azure Key Vault 中的值或預設值將會自動插入執行階段上的欄位中，如此可以簡化開發人員的工作週期，因為他們不再需要以建構函式和 setter 方法傳遞值，而是讓 MicroProfile 來處理。</span><span class="sxs-lookup"><span data-stu-id="fabc2-109">The end result is that the value stored in Azure Key Vault, or the default value, will be injected automatically into the field at runtime, simplifying the life of developers as they no longer need to pass values around in constuctors and setter methods, instead leaving it to MicroProfile to handle.</span></span>

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

<span data-ttu-id="fabc2-110">如有必要，您也可以直接存取 MicroProfile 組態來要求祕密，例如：</span><span class="sxs-lookup"><span data-stu-id="fabc2-110">It is also possible to access the MicroProfile config directly, to request secrets as necessary, e.g.:</span></span>

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

<span data-ttu-id="fabc2-111">此範例使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile](https://microprofile.io/) 來建立可在您機器本機中執行的小型 Java war 檔案。</span><span class="sxs-lookup"><span data-stu-id="fabc2-111">This sample makes use of [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile](https://microprofile.io/) to create a tiny Java war file that can be run locally on your machine.</span></span> <span data-ttu-id="fabc2-112">其中不會示範如何轉換為 Docker 或將程式碼推送至 Azure，但是在本教學課程結尾處的 [連結] 區段中，有說明此操作的其他教學課程連結。</span><span class="sxs-lookup"><span data-stu-id="fabc2-112">It does not demonstrate how to dockerise or push the code to Azure, but the links section at the end of this tutorial has links to other useful tutorials that explain this.</span></span>

<span data-ttu-id="fabc2-113">此範例會使用免費的開放原始碼程式庫為 Azure Key Vault 建立組態來源 (使用 MicroProfile Config API)。</span><span class="sxs-lookup"><span data-stu-id="fabc2-113">This sample makes use of a free and open source library that creats a config source (using the MicroProfile Config API) for Azure Key Vault.</span></span> <span data-ttu-id="fabc2-114">您可以在 [GitHub 專案頁面](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)上深入了解此程式庫及檢閱程式碼。</span><span class="sxs-lookup"><span data-stu-id="fabc2-114">You can learn more about this library, and review the code, on the [project GitHub page](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault).</span></span> <span data-ttu-id="fabc2-115">藉由使用此程式庫，本教學課程中的程式碼可只專注於程式庫設定，然後將金鑰插入您的程式碼中，我們不需要撰寫任何 Azure 特有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fabc2-115">By using this library, the code in this tutorial can simply focus on configuration of the library, followed by injecting keys into your code, and we don't need to write any Azure-specific code.</span></span>

<span data-ttu-id="fabc2-116">以下是在本機電腦上執行此程式碼所需的步驟，我們從建立 Azure Key Vault 資源開始。</span><span class="sxs-lookup"><span data-stu-id="fabc2-116">Here are the steps required to run this code on your local machine, starting with creating an Azure Key Vault resource.</span></span>

## <a name="creating-an-azure-key-vault-resource"></a><span data-ttu-id="fabc2-117">新增 Azure Key Vault 資源</span><span class="sxs-lookup"><span data-stu-id="fabc2-117">Creating an Azure Key Vault resource</span></span>

<span data-ttu-id="fabc2-118">我們將使用 Azure CLI 來建立 Azure Key Vault 資源，並填入一個祕密。</span><span class="sxs-lookup"><span data-stu-id="fabc2-118">We will use the Azure CLI to create the Azure Key Vault resource and populate it with one secret.</span></span>

1. <span data-ttu-id="fabc2-119">首先，建立 Azure 服務主體。</span><span class="sxs-lookup"><span data-stu-id="fabc2-119">Firstly, lets create an Azure service principal.</span></span> <span data-ttu-id="fabc2-120">這會提供我們存取 Key Vault 用戶端所需的識別碼和金鑰：</span><span class="sxs-lookup"><span data-stu-id="fabc2-120">This will provide us with the client ID and key we need to access Key Vault:</span></span>

```bash
az login
az account set --subscription <subscription_id>

az ad sp create-for-rbac --name <service_principal_name>
```

<span data-ttu-id="fabc2-121">假設我們使用 `microprofile-keyvault-service-principal` 作為前一個步驟中的服務主體名稱。</span><span class="sxs-lookup"><span data-stu-id="fabc2-121">Lets say we use `microprofile-keyvault-service-principal` for the service principal name in the previous step.</span></span> <span data-ttu-id="fabc2-122">執行此動作後，Azure 的回應格式會如下所示 (已稍微刪改)：</span><span class="sxs-lookup"><span data-stu-id="fabc2-122">The response from Azure for doing this will be in the following, slightly censored, form:</span></span>

```json
{
  "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
  "displayName": "microprofile-keyvault-service-principal",
  "name": "http://microprofile-keyvault-service-principal",
  "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
  "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
}
```

<span data-ttu-id="fabc2-123">在此要特別注意的是 `appID` 和 `password` 值 - 我們將在之後的 `client ID` 和 `key` 上分別使用這兩個值。</span><span class="sxs-lookup"><span data-stu-id="fabc2-123">Of particular note here is the `appID` and `password` values - these are what we will use later on as `client ID` and `key`, respectively.</span></span>

<span data-ttu-id="fabc2-124">現在，我們已建立了服務主體，且可以選擇性地建立資源群組 (如果您已經有想用的資源群組，可以略過此步驟)。</span><span class="sxs-lookup"><span data-stu-id="fabc2-124">Now that we have created a service principal, we can optionally create a resource group (if you already have one you want to use, you can skip this step).</span></span> <span data-ttu-id="fabc2-125">請注意，若要取得資源群組位置清單，您可以呼叫 `az account list-locations`，並使用該清單中的 `name` 值來指定應建立資源群組位置。</span><span class="sxs-lookup"><span data-stu-id="fabc2-125">Note that to get a list of resource group locations, you can call `az account list-locations` and use the `name` value from that list to specify where the resource group should be created.</span></span>

```bash
# For this tutorial, the author chose to use `westus`
# and `jg-test` for the resource group name.
az group create -l <resource_group_location> -n <resource_group_name>
```

<span data-ttu-id="fabc2-126">我們現在來建立 Azure Key Vault 資源。</span><span class="sxs-lookup"><span data-stu-id="fabc2-126">We now create an Azure Key Vault resource.</span></span> <span data-ttu-id="fabc2-127">請注意，Key Vault 名稱是之後要用來參考金鑰保存庫的名稱，因此，請選擇令人印象深刻的名稱。</span><span class="sxs-lookup"><span data-stu-id="fabc2-127">Note that the Key Vault name is what you will use to reference the key vault later, so choose something memorable.</span></span>

```bash
az keyvault create --name <your_keyvault_name>            \
                   --resource-group <your_resource_group> \
                   --location <location>                  \
                   --enabled-for-deployment true          \
                   --enabled-for-disk-encryption true     \
                   --enabled-for-template-deployment true \
                   --sku standard
```

<span data-ttu-id="fabc2-128">我們也需要將適當的權限授與稍早建立的服務主體，使其可以存取 Key Vault 祕密。</span><span class="sxs-lookup"><span data-stu-id="fabc2-128">We also need to grant the appropriate permissions to the service principal we created earlier, so that it may access the Key Vault secrets.</span></span> <span data-ttu-id="fabc2-129">請注意，appID 值是我們剛才建立服務主體所在位置中的 `appId` (也就是 `5292398e-XXXX-40ce-XXXX-d49fXXXX9e79` - 但請使用您終端機輸出中的值)。</span><span class="sxs-lookup"><span data-stu-id="fabc2-129">Note that the appID value is the `appId` value from above where we created the service principal (that is, `5292398e-XXXX-40ce-XXXX-d49fXXXX9e79` - but use the value from your terminal output).</span></span>

```bash
az keyvault set-policy --name <your_keyvault_name>   \
                       --secret-permission get list  \
                       --spn <your_sp_appId_created_in_step1>
```

<span data-ttu-id="fabc2-130">現在，我們已經可以將祕密推送至 Key Vault 了。</span><span class="sxs-lookup"><span data-stu-id="fabc2-130">We are now at the point where we can push a secret into Key Vault.</span></span> <span data-ttu-id="fabc2-131">讓我們使用 `demo-key` 作為金鑰名稱，並將金鑰值設為 `demo-value`：</span><span class="sxs-lookup"><span data-stu-id="fabc2-131">Lets use the key name `demo-key`, and set the value of the key to be `demo-value`:</span></span>

```bash
az keyvault secret set --name demo-key      \
                       --value demo-value   \
                       --vault-name <your_keyvault_name>  
```

<span data-ttu-id="fabc2-132">就這麼簡單！</span><span class="sxs-lookup"><span data-stu-id="fabc2-132">That's it!</span></span> <span data-ttu-id="fabc2-133">現在，具有單一祕密的 Key Vault 正在 Azure 中執行。</span><span class="sxs-lookup"><span data-stu-id="fabc2-133">We now have Key Vault running in Azure with a single secret.</span></span> <span data-ttu-id="fabc2-134">我們現在可以複製此存放庫，並將它設定為在應用程式中使用此資源。</span><span class="sxs-lookup"><span data-stu-id="fabc2-134">We can now clone this repo and configure it to use this resource in our app.</span></span>

## <a name="getting-up-and-running-locally"></a><span data-ttu-id="fabc2-135">啟動並在本機執行</span><span class="sxs-lookup"><span data-stu-id="fabc2-135">Getting up and running locally</span></span>

<span data-ttu-id="fabc2-136">此範例會以 GitHub 上的可用範例應用程式為基礎，因此我們會先將其複製，然後逐步執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="fabc2-136">This example is based on a sample application available on GitHub, so we will clone that and then step through the code.</span></span> <span data-ttu-id="fabc2-137">請遵循下列步驟，將程式碼複製到您的機器上：</span><span class="sxs-lookup"><span data-stu-id="fabc2-137">Follow the steps below to get the code cloned onto your machine:</span></span>

1. `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

1. `cd microprofile-configsource-keyvault`

1. <span data-ttu-id="fabc2-138">瀏覽至 `src/main/resources/META-INF/microprofile-config.properties`，並將 microprofile-config.properties 檔案中的屬性變更為上述的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fabc2-138">Navigate to `src/main/resources/META-INF/microprofile-config.properties` and change the properties in microprofile-config.properties file with details from above.</span></span>

1. <span data-ttu-id="fabc2-139">請嘗試使用 `mvn clean package payara-micro:start` 來執行伺服器</span><span class="sxs-lookup"><span data-stu-id="fabc2-139">Try running the server using `mvn clean package payara-micro:start`</span></span>

1. <span data-ttu-id="fabc2-140">嘗試在 Web 瀏覽器中存取 [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) - 您應該會看到簡單的回應，其中顯示從 Azure Key Vault 所讀取的值。</span><span class="sxs-lookup"><span data-stu-id="fabc2-140">Try accessing [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) in your web browser - you should see a simple response that demonstrates values being read from Azure Key Vault.</span></span>

## <a name="summary"></a><span data-ttu-id="fabc2-141">總結</span><span class="sxs-lookup"><span data-stu-id="fabc2-141">Summary</span></span>

<span data-ttu-id="fabc2-142">此範例應用程式結合 MicroProfile Config API、Azure Key Vault 和免費的 [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) 開放原始碼程式庫，讓您可以輕鬆地將組態資料和祕密插入我們的 MicroProfile Web 服務。</span><span class="sxs-lookup"><span data-stu-id="fabc2-142">This sample application bakes together the MicroProfile Config API, Azure Key Vault, and the free and open source [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) library to enable easy injection of configuration data and secrets into our MicroProfile web services.</span></span>
