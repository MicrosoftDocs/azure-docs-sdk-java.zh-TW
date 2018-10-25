---
title: 適用於 Java 的 Azure Key Vault 程式庫
description: 適用於 Java 的 Azure Key Vault 程式庫概觀
keywords: Azure, Java, SDK, API, keyvault, 安全的, 金鑰, 祕密, 保存庫
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: keyvault
ms.openlocfilehash: 84ea77a19c326409f453f62359cf46c90398daeb
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799924"
---
# <a name="azure-key-vault-libraries-for-java"></a>適用於 Java 的 Azure Key Vault 程式庫

## <a name="overview"></a>概觀

使用 [Azure Key Vault](/azure/key-vault/) 來保護及管理雲端應用程式和服務所使用的密碼編譯金鑰和祕密。

若要開始使用 Azure Key Vault，請參閱[開始使用 Azure Key Vault](/azure/key-vault/key-vault-get-started)。

## <a name="client-library"></a>用戶端程式庫

使用用戶端程式庫建立、更新和刪除 Azure Key Vault 中的金鑰和祕密。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.1.0</version>
</dependency>
```   

## <a name="example"></a>範例

從 Key Vault 擷取[JSON Web 金鑰](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)。

```java
KeyVaultClient kvc = new KeyVaultClient(credentials);
KeyBundle returnedKeyBundle = kvc.getKey(vaultUrl, keyName);
JsonWebKey jsonKey = returnedKeyBundle.key();
```

> [!div class="nextstepaction"]
> [探索用戶端 API](/java/api/overview/azure/keyvault/client)


## <a name="management-api"></a>管理 API

使用 Azure Key Vault 管理程式庫來建立金鑰保存庫、授權應用程式並管理權限。 

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-keyvault</artifactId>
    <version>1.15.0</version>
</dependency>
```

## <a name="example"></a>範例

授權使用[服務主體](/azure/azure-resource-manager/resource-group-create-service-principal-portal)`clientId`來執行的應用程式列出和擷取金鑰保存庫中的祕密。 

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
> [探索管理 API](/java/api/overview/azure/keyvault/management)


## <a name="samples"></a>範例

深入探索可在應用程式中使用的 [Azure Key Vault Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault)。
