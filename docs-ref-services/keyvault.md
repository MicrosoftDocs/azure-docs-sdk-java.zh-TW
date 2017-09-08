---
title: "適用於 Java 的 Azure Key Vault 程式庫"
description: "適用於 Java 的 Azure Key Vault 程式庫概觀"
keywords: "Azure, Java, SDK, API, keyvault, 安全的, 金鑰, 祕密, 保存庫"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: keyvault
ms.openlocfilehash: dc5e0552728e362451c033317e5a804c8067e79c
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="azure-key-vault-libraries-for-java"></a><span data-ttu-id="43048-104">適用於 Java 的 Azure Key Vault 程式庫</span><span class="sxs-lookup"><span data-stu-id="43048-104">Azure Key Vault libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="43048-105">概觀</span><span class="sxs-lookup"><span data-stu-id="43048-105">Overview</span></span>

<span data-ttu-id="43048-106">使用 [Azure Key Vault](/azure/key-vault/) 來保護及管理雲端應用程式和服務所使用的密碼編譯金鑰和祕密。</span><span class="sxs-lookup"><span data-stu-id="43048-106">Safeguard and manage cryptographic keys and secrets used by cloud applications and services with [Azure Key Vault](/azure/key-vault/).</span></span>

<span data-ttu-id="43048-107">若要開始使用 Azure Key Vault，請參閱[開始使用 Azure Key Vault](/azure/key-vault/key-vault-get-started)。</span><span class="sxs-lookup"><span data-stu-id="43048-107">To get started with Azure Key Vault, see [Get started with Azure Key Vault](/azure/key-vault/key-vault-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="43048-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="43048-108">Client library</span></span>

<span data-ttu-id="43048-109">使用用戶端程式庫建立、更新和刪除 Azure Key Vault 中的金鑰和祕密。</span><span class="sxs-lookup"><span data-stu-id="43048-109">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="43048-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="43048-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="43048-111">範例</span><span class="sxs-lookup"><span data-stu-id="43048-111">Example</span></span>

<span data-ttu-id="43048-112">從 Key Vault 擷取[JSON Web 金鑰](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)。</span><span class="sxs-lookup"><span data-stu-id="43048-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```java
KeyVaultClient kvc = new KeyVaultClient(credentials);
KeyBundle returnedKeyBundle = getKey(vaultUrl, keyName);
JsonWebKey jsonKey = returnedKeyBundle.key();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="43048-113">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="43048-113">Explore the Client APIs</span></span>](/java/api/overview/azure/keyvault/clientlibrary)


## <a name="management-api"></a><span data-ttu-id="43048-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="43048-114">Management API</span></span>

<span data-ttu-id="43048-115">使用 Azure Key Vault 管理程式庫來建立金鑰保存庫、授權應用程式並管理權限。</span><span class="sxs-lookup"><span data-stu-id="43048-115">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="43048-116">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="43048-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-keyvault</artifactId>
    <version>1.1.2</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="43048-117">範例</span><span class="sxs-lookup"><span data-stu-id="43048-117">Example</span></span>

<span data-ttu-id="43048-118">授權使用[服務主體](/azure/azure-resource-manager/resource-group-create-service-principal-portal)`clientId`來執行的應用程式列出和擷取金鑰保存庫中的祕密。</span><span class="sxs-lookup"><span data-stu-id="43048-118">Authorize and application running with [service principal](/azure/azure-resource-manager/resource-group-create-service-principal-portal) `clientId` to list and retrieve secrets from a key vault.</span></span> 

```java
vault1 = vault1.update()
            .defineAccessPolicy()
                .forServicePrincipal(clientId)
                .allowKeyAllPermissions()
                .allowSecretPermissions(SecretPermissions.GET)
                .allowSecretPermissions(SecretPermissions.LIST)
                .attach()
            .apply();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="43048-119">瀏覽管理 API</span><span class="sxs-lookup"><span data-stu-id="43048-119">Explore the Management APIs</span></span>](/java/api/overview/azure/keyvault/managementapi)


## <a name="samples"></a><span data-ttu-id="43048-120">範例</span><span class="sxs-lookup"><span data-stu-id="43048-120">Samples</span></span>

<span data-ttu-id="43048-121">[管理 Key Vault][1]</span><span class="sxs-lookup"><span data-stu-id="43048-121">[Manage Key Vaults][1]</span></span>   

[1]: https://github.com/Azure-Samples/key-vault-java-manage-key-vaults

<span data-ttu-id="43048-122">深入探索可在應用程式中使用的 [Azure Key Vault Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault)。</span><span class="sxs-lookup"><span data-stu-id="43048-122">Explore more [sample Java code for Azure Key Vault](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault) you can use in your apps.</span></span>
