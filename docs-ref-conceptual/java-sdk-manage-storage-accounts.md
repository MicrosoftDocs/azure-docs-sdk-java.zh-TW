---
title: "使用 Java 來管理 Azure 儲存體帳戶 | Microsoft Docs"
description: "使用適用於 Java 的 Azure SDK 來管理 Azure 儲存體帳戶的程式碼範例"
author: rloutlaw
manager: douge
ms.assetid: 49be8b66-3b56-4c10-8f14-9d326d815cb4
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 5945164b2b04e1fa9169590a71f6c5f9f45842d6
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="manage-azure-storage-accounts-from-your-java-applications"></a>從 Java 應用程式管理 Azure 儲存體帳戶

[此範例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)會建立 [Azure 儲存體](https://docs.microsoft.com/azure/storage/storage-introduction)帳戶，並透過 [Java 管理程式庫](https://github.com/Azure/azure-sdk-for-java)來與帳戶存取金鑰搭配運作。 

## <a name="run-the-sample"></a>執行範例

建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。 然後，執行：

```
git clone https://github.com/Azure-Samples/storage-java-manage-storage-accounts.git
cd storage-java-manage-storage-accounts
mvn clean compile exec:java
```

檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)。

## <a name="authenticate-with-azure"></a>使用 Azure 進行驗證

[!INCLUDE [auth-include](includes/java-auth-include.md)] 

## <a name="create-a-storage-account"></a>建立儲存體帳戶

```java
// create a new storage account
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .create();
```

提供的儲存體名稱必須和 Azure 中的所有名稱不同，並只包含小寫字母和數字。 這個帳戶所使用的預設效能和複寫設定檔是 [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage)。

## <a name="list-keys-in-a-storage-account"></a>列出儲存體帳戶中的金鑰
```java
// list the name and value for each access key in the storage account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

每個 Azure 儲存體帳戶中都會提供兩個金鑰，讓您可以在重新產生一個金鑰的同時，仍允許使用另一個金鑰存取儲存體。

## <a name="regenerate-a-key-in-a-storage-account"></a>在儲存體帳戶中重新產生金鑰

```java
// regenerate the first key in a storage account and return an updated list of keys 
List<StorageAccountKey> updatedStorageAccountKeys =
    storageAccount.regenerateKey(storageAccountKeys.get(0).keyName());
```

您必須在產生新的金鑰後，以新的金鑰更新所有的 Azure 資源和應用程式。

## <a name="list-all-storage-accounts-in-a-resource-group"></a>列出資源群組中的所有儲存體帳戶
```java
// get a list of accounts in a resource group , log info about each one
List<StorageAccount> accounts = azure.storageAccounts().listByResourceGroup(rgName);
for (StorageAccount sa : accounts) {
    System.out.println("Storage Account " + sa.name() + " created @ " + sa.creationTime());
}
```

[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) 提供一組實用的方法，供您檢查儲存體帳戶的組態。

## <a name="delete-a-storage-account"></a>刪除儲存體帳戶
```java
// delete by ID when you already have a storage account object
azure.storageAccounts().deleteById(storageAccount.id());

// delete by resource group and account name if you don't have an account object
azure.storageAccounts().deleteByResourceGroup(rgName,accountName);
```

> [!NOTE]
> 若有使用中的磁碟映像連線至虛擬機器，或是有磁碟正由其他構件使用中，儲存體帳戶就可能無法透過這些方法來移除。 請先將儲存體與這些資源中斷連結，再移除帳戶。

## <a name="sample-explanation"></a>範例的說明

[GitHub 上的程式碼範例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)：

- 建立儲存體帳戶
- 讀取並重新產生存取金鑰
- 列出資源群組中的所有儲存體帳戶
- 刪除儲存體帳戶 

| 範例中使用的類別 | 注意事項
|-------|-------|
| [StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account)  | 用來表示 Azure 儲存體帳戶。 請使用該類別的方法來取得有關儲存體帳戶的資訊。
| [StorageAccountKey](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account_key) | `StorageAccount.getKeys()` 會傳回儲存體帳戶金鑰。 在 `StorageAccount` 中使用 `regenerateKey` 方法來更新金鑰。

## <a name="next-steps"></a>後續步驟

[!INCLUDE [next-steps](includes/java-next-steps.md)]