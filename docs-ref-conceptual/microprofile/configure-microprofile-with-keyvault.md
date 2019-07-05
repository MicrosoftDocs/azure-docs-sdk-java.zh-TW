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
# <a name="configure-microprofile-by-using-azure-key-vault"></a><span data-ttu-id="53304-103">使用 Azure Key Vault 設定 MicroProfile</span><span class="sxs-lookup"><span data-stu-id="53304-103">Configure MicroProfile by using Azure Key Vault</span></span>

<span data-ttu-id="53304-104">本文示範如何設定 [MicroProfile](http://microprofile.io) 應用程式以從 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 擷取祕密。</span><span class="sxs-lookup"><span data-stu-id="53304-104">This article demonstrates how to configure a [MicroProfile](http://microprofile.io) application to retrieve secrets from an [Azure key vault](https://azure.microsoft.com/services/key-vault/).</span></span> <span data-ttu-id="53304-105">在此程序中，您將使用 [MicroProfile Config API](https://microprofile.io/project/eclipse/microprofile-config) 建立對 Azure Key Vault 的直接連線。</span><span class="sxs-lookup"><span data-stu-id="53304-105">In this process, you use the [MicroProfile Config APIs](https://microprofile.io/project/eclipse/microprofile-config) to create a direct connection to an Azure key vault.</span></span> <span data-ttu-id="53304-106">身為開發人員，藉由使用 MicroProfile Config API，您將可利用標準 API 將組態資料擷取和插入至其微服務。</span><span class="sxs-lookup"><span data-stu-id="53304-106">As a developer, by using the MicroProfile Config APIs, you benefit from a standard API for retrieving and injecting configuration data into their microservices.</span></span>

<span data-ttu-id="53304-107">在開始之前，請先快速了解 Azure Key Vault 和 MicroProfile Config API 的組合能讓您在程式碼中寫入哪些資訊。</span><span class="sxs-lookup"><span data-stu-id="53304-107">Before you start, take a quick look at what a combination of Azure Key Vault and the MicroProfile Config API enables you to write in your code.</span></span> <span data-ttu-id="53304-108">以下是某欄位的程式碼片段，且其所屬類別標註了 `@Inject` 和 `@ConfigProperty`。</span><span class="sxs-lookup"><span data-stu-id="53304-108">The following code snippet is of a field in a class that's annotated with `@Inject` and `@ConfigProperty`.</span></span> <span data-ttu-id="53304-109">註釋中指定的 *name* 是要在您的金鑰保存庫中查閱的屬性名稱，而 *defaultValue* 則是找不到金鑰時所將設定的值。</span><span class="sxs-lookup"><span data-stu-id="53304-109">The *name* that's specified in the annotation is the name of the property to look up in your key vault, and the *defaultValue* is what will be set if the key is not discovered.</span></span> <span data-ttu-id="53304-110">其結果是，儲存在金鑰保存庫中的值或預設值，會在執行階段自動插入欄位中。</span><span class="sxs-lookup"><span data-stu-id="53304-110">The result is that the value that's stored in the key vault, or the default value, is injected automatically into the field at runtime.</span></span> <span data-ttu-id="53304-111">此動作可以簡化您的工作，因為您不再需要在建構函式和 setter 方法中傳遞值。</span><span class="sxs-lookup"><span data-stu-id="53304-111">This action can simplify your life, because you no longer need to pass values around in constructors and setter methods.</span></span> <span data-ttu-id="53304-112">MicroProfile 會為您處理這項工作。</span><span class="sxs-lookup"><span data-stu-id="53304-112">Instead, MicroProfile handles this task.</span></span>

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

<span data-ttu-id="53304-113">若要要求祕密 (如有必要)，您也可以直接存取 MicroProfile 組態。</span><span class="sxs-lookup"><span data-stu-id="53304-113">To request secrets, as necessary, you can also access the MicroProfile config directly.</span></span> <span data-ttu-id="53304-114">例如︰</span><span class="sxs-lookup"><span data-stu-id="53304-114">For example:</span></span>

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

<span data-ttu-id="53304-115">此範例程式碼使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile](https://microprofile.io/) 來建立您可在本機電腦上執行的小型 Java Web 應用程式需求 (WAR) 檔案。</span><span class="sxs-lookup"><span data-stu-id="53304-115">This sample code uses [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile](https://microprofile.io/) to create a tiny Java web application requirement (WAR) file that you can run locally on your machine.</span></span> <span data-ttu-id="53304-116">其中不會示範如何轉換為 Docker 或將程式碼推送至 Azure，但本教學課程結尾處附有連結，會連結至說明此操作的其他教學課程。</span><span class="sxs-lookup"><span data-stu-id="53304-116">It doesn't demonstrate how to Dockerize or push the code to Azure, but the section at the end of this article has links to other useful tutorials that explain this.</span></span>

<span data-ttu-id="53304-117">此範例使用會在您的金鑰保存庫中建立組態來源 (使用 MicroProfile Config API) 的免費開放原始碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="53304-117">The sample uses a free and open source library that creates a config source (using the MicroProfile Config API) in your key vault.</span></span> <span data-ttu-id="53304-118">若要深入了解此程式庫及檢閱程式碼，請參閱[專案 GitHub 頁面](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)。</span><span class="sxs-lookup"><span data-stu-id="53304-118">To learn more about this library and review the code, see the [project GitHub page](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault).</span></span> <span data-ttu-id="53304-119">如果您使用此程式庫，本教學課程中的程式碼就可以只專注於此程式庫的組態，然後將金鑰插入您的程式碼中。</span><span class="sxs-lookup"><span data-stu-id="53304-119">If you use the library, the code in this tutorial can simply focus on the configuration of the library and then inject keys into your code.</span></span> <span data-ttu-id="53304-120">您不需要撰寫任何 Azure 特定程式碼。</span><span class="sxs-lookup"><span data-stu-id="53304-120">You don't need to write any Azure-specific code.</span></span>

<span data-ttu-id="53304-121">若要在本機電腦上執行此程式碼，請依照前幾節的指示，從建立金鑰保存庫資源開始。</span><span class="sxs-lookup"><span data-stu-id="53304-121">To run this code on your local machine, starting with creating a key vault resource, follow the instructions in the next sections.</span></span>

## <a name="create-a-key-vault-resource"></a><span data-ttu-id="53304-122">建立金鑰保存庫資源</span><span class="sxs-lookup"><span data-stu-id="53304-122">Create a key vault resource</span></span>

<span data-ttu-id="53304-123">在本節中，您將使用 Azure CLI 來建立金鑰保存庫資源，並在其中填入祕密。</span><span class="sxs-lookup"><span data-stu-id="53304-123">In this section, you use the Azure CLI to create a key vault resource and populate it with one secret.</span></span>

1. <span data-ttu-id="53304-124">建立 Azure 服務主體。</span><span class="sxs-lookup"><span data-stu-id="53304-124">Create an Azure service principal.</span></span> <span data-ttu-id="53304-125">此步驟會為您提供存取金鑰保存庫所需的用戶端識別碼和金鑰：</span><span class="sxs-lookup"><span data-stu-id="53304-125">This step gives you the client ID and key that you need to access your key vault:</span></span>

    ```bash
    az login
    az account set --subscription <subscription_id>

    az ad sp create-for-rbac --name <service_principal_name>
    ```

    <span data-ttu-id="53304-126">我們將使用 *microprofile-keyvault-service-principal* 來取代前述步驟中的 *\<service_principal_name>* 。</span><span class="sxs-lookup"><span data-stu-id="53304-126">Let's use *microprofile-keyvault-service-principal* to replace *\<service_principal_name>* in the preceding step.</span></span> <span data-ttu-id="53304-127">Azure 的回應會類似於下列內容：</span><span class="sxs-lookup"><span data-stu-id="53304-127">The response from Azure would be similar to the following:</span></span>

    ```json
    {
      "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
      "displayName": "microprofile-keyvault-service-principal",
      "name": "http://microprofile-keyvault-service-principal",
      "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
      "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
    }
    ```

    <span data-ttu-id="53304-128">在此應特別注意的是 *appId* 和 *password* 值。</span><span class="sxs-lookup"><span data-stu-id="53304-128">Of particular note here are the *appId* and *password* values.</span></span> <span data-ttu-id="53304-129">在本文稍後的步驟中，您會將其作為*用戶端識別碼*和*金鑰*。</span><span class="sxs-lookup"><span data-stu-id="53304-129">You'll use them later in this article as *client ID* and *key*.</span></span>

1. <span data-ttu-id="53304-130">(選擇性) 現在您已建立服務主體，接下來可以建立資源群組。</span><span class="sxs-lookup"><span data-stu-id="53304-130">(Optional) Now that you've created a service principal, you can create a resource group.</span></span> <span data-ttu-id="53304-131">如果您已有想要使用的資源群組，可以略過此步驟。</span><span class="sxs-lookup"><span data-stu-id="53304-131">If you already have a resource group that you want to use, you can skip this step.</span></span> <span data-ttu-id="53304-132">若要取得資源群組位置清單，您可以呼叫 `az account list-locations`，並使用該清單中的 *name* 值來指定應建立資源群組位置。</span><span class="sxs-lookup"><span data-stu-id="53304-132">To get a list of resource group locations, you can call `az account list-locations` and use the *name* value from that list to specify where the resource group should be created.</span></span>

    ```bash
    # In this tutorial, "westus" is the resource group location
    # and "jg-test" is the resource group name.
    az group create -l <resource_group_location> -n <resource_group_name>
    ```

1. <span data-ttu-id="53304-133">建立金鑰保存庫資源。</span><span class="sxs-lookup"><span data-stu-id="53304-133">Create a key vault resource.</span></span> <span data-ttu-id="53304-134">稍後您將使用金鑰保存庫名稱來參考金鑰保存庫，因此請務必選擇好記的名稱。</span><span class="sxs-lookup"><span data-stu-id="53304-134">You'll use your key vault name to refer to the key vault later, so be sure to choose a memorable name.</span></span>

    ```bash
    az keyvault create --name <your_keyvault_name>            \
                      --resource-group <your_resource_group> \
                      --location <location>                  \
                      --enabled-for-deployment true          \
                      --enabled-for-disk-encryption true     \
                      --enabled-for-template-deployment true \
                      --sku standard
    ```

1. <span data-ttu-id="53304-135">請將適當的權限授與您先前建立的服務主體，使其可以存取金鑰保存庫祕密。</span><span class="sxs-lookup"><span data-stu-id="53304-135">Grant the appropriate permissions to the service principal that you created earlier, so that it can access the key vault secrets.</span></span> <span data-ttu-id="53304-136">下列程式碼中的 appId 值，是您在步驟 1 中建立服務主體時所產生的 *appId* 值。</span><span class="sxs-lookup"><span data-stu-id="53304-136">The appId value in the following code is the *appId* value from step 1, where you created the service principal.</span></span> <span data-ttu-id="53304-137">也就是說，步驟 1 中的 *appId* 為 *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*，但您應使用您自己的終端機輸出的值。</span><span class="sxs-lookup"><span data-stu-id="53304-137">That is, the *appId* in step 1 was *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*, but you should use the value from your own terminal output.</span></span>

    ```bash
    az keyvault set-policy --name <your_keyvault_name>   \
                          --secret-permission get list  \
                          --spn <your_sp_appId_created_in_step1>
    ```

1. <span data-ttu-id="53304-138">現在，您可以將祕密推送至您的金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="53304-138">Now you can push a secret into your key vault.</span></span> <span data-ttu-id="53304-139">請使用金鑰名稱 *demo-key*，並將金鑰的值設為 *demo-value*：</span><span class="sxs-lookup"><span data-stu-id="53304-139">Use the key name *demo-key*, and set the value of the key to *demo-value*:</span></span>

    ```bash
    az keyvault secret set --name demo-key      \
                           --value demo-value   \
                           --vault-name <your_keyvault_name>  
    ```

<span data-ttu-id="53304-140">就這麼簡單！</span><span class="sxs-lookup"><span data-stu-id="53304-140">That's it!</span></span> <span data-ttu-id="53304-141">現在，您已有使用單一祕密在 Azure 中執行的金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="53304-141">You now have a key vault running in Azure with a single secret.</span></span> <span data-ttu-id="53304-142">接下來您可以複製此存放庫，並將其設定為在應用程式中使用此資源。</span><span class="sxs-lookup"><span data-stu-id="53304-142">You can now clone this repo and configure it to use this resource in your app.</span></span>

## <a name="get-up-and-running-locally"></a><span data-ttu-id="53304-143">在本機啟動並執行</span><span class="sxs-lookup"><span data-stu-id="53304-143">Get up and running locally</span></span>

<span data-ttu-id="53304-144">此範例會以 GitHub 上的可用範例應用程式為基礎，因此您會先複製該應用程式，然後再逐步執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="53304-144">This example is based on a sample application that's available on GitHub, so you'll clone that application and then step through the code.</span></span> 

1. <span data-ttu-id="53304-145">輸入下列命令，將程式碼複製到您的電腦上：</span><span class="sxs-lookup"><span data-stu-id="53304-145">Clone the code onto your machine by entering the following commands:</span></span>

    `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

    `cd microprofile-configsource-keyvault`

1. <span data-ttu-id="53304-146">移至 *src/main/resources/META-INF/microprofile-config.properties*，然後使用先前步驟中的值變更 *microprofile-config.properties* 檔案中的屬性。</span><span class="sxs-lookup"><span data-stu-id="53304-146">Go to *src/main/resources/META-INF/microprofile-config.properties*, and then change the properties in the *microprofile-config.properties* file by using the values from the previous steps.</span></span>

1. <span data-ttu-id="53304-147">嘗試使用 `mvn clean package payara-micro:start` 來執行伺服器。</span><span class="sxs-lookup"><span data-stu-id="53304-147">Try running the server by using `mvn clean package payara-micro:start`.</span></span>

1. <span data-ttu-id="53304-148">嘗試在網頁瀏覽器中存取 [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config)。</span><span class="sxs-lookup"><span data-stu-id="53304-148">Try accessing [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) in your web browser.</span></span> <span data-ttu-id="53304-149">您應該會看到簡單的回應，其中顯示從您的金鑰保存庫讀取的值。</span><span class="sxs-lookup"><span data-stu-id="53304-149">You should see a simple response that demonstrates values that are being read from your key vault.</span></span>

## <a name="summary"></a><span data-ttu-id="53304-150">總結</span><span class="sxs-lookup"><span data-stu-id="53304-150">Summary</span></span>

<span data-ttu-id="53304-151">此範例應用程式結合了 MicroProfile Config API、Azure Key Vault 和免費的 [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) 開放原始碼程式庫，讓您可以輕鬆地將組態資料和祕密插入 MicroProfile Web 服務中。</span><span class="sxs-lookup"><span data-stu-id="53304-151">This sample application combines the MicroProfile Config API, Azure Key Vault, and the free and open source [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) library to enable easy injection of configuration data and secrets into your MicroProfile web services.</span></span>
