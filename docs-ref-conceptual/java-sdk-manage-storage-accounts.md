---
title: 使用 Java 來管理 Azure 儲存體帳戶 | Microsoft Docs
description: 使用適用於 Java 的 Azure SDK 來管理 Azure 儲存體帳戶的程式碼範例
author: rloutlaw
manager: douge
ms.assetid: 49be8b66-3b56-4c10-8f14-9d326d815cb4
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 620ee28691f70ed6cf29c4f7c169cd43a6e71351
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592573"
---
# <a name="manage-azure-storage-accounts-from-your-java-applications"></a><span data-ttu-id="70e91-103">從 Java 應用程式管理 Azure 儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70e91-103">Manage Azure storage accounts from your Java applications</span></span>

<span data-ttu-id="70e91-104">[此範例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)會建立 [Azure 儲存體](https://docs.microsoft.com/azure/storage/storage-introduction)帳戶，並透過 [Java 管理程式庫](https://github.com/Azure/azure-sdk-for-java)來與帳戶存取金鑰搭配運作。</span><span class="sxs-lookup"><span data-stu-id="70e91-104">[This sample](https://github.com/Azure-Samples/storage-java-manage-storage-accounts) creates an [Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction) account and works with the account access keys using the [Java management libraries](https://github.com/Azure/azure-sdk-for-java).</span></span> 

## <a name="run-the-sample"></a><span data-ttu-id="70e91-105">執行範例</span><span class="sxs-lookup"><span data-stu-id="70e91-105">Run the sample</span></span>

<span data-ttu-id="70e91-106">建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="70e91-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="70e91-107">然後，執行：</span><span class="sxs-lookup"><span data-stu-id="70e91-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/storage-java-manage-storage-accounts.git
cd storage-java-manage-storage-accounts
mvn clean compile exec:java
```

<span data-ttu-id="70e91-108">檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)。</span><span class="sxs-lookup"><span data-stu-id="70e91-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="70e91-109">使用 Azure 進行驗證</span><span class="sxs-lookup"><span data-stu-id="70e91-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)] 

## <a name="create-a-storage-account"></a><span data-ttu-id="70e91-110">建立儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70e91-110">Create a storage account</span></span>

```java
// create a new storage account
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .create();
```

<span data-ttu-id="70e91-111">提供的儲存體名稱必須和 Azure 中的所有名稱不同，並只包含小寫字母和數字。</span><span class="sxs-lookup"><span data-stu-id="70e91-111">The storage name provided must be unique across all names in Azure and contain only lowercase letters and numbers.</span></span> <span data-ttu-id="70e91-112">這個帳戶所使用的預設效能和複寫設定檔是 [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage)。</span><span class="sxs-lookup"><span data-stu-id="70e91-112">The default performance and replication profile used for this account is [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage).</span></span>

## <a name="list-keys-in-a-storage-account"></a><span data-ttu-id="70e91-113">列出儲存體帳戶中的金鑰</span><span class="sxs-lookup"><span data-stu-id="70e91-113">List keys in a storage account</span></span>
```java
// list the name and value for each access key in the storage account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

<span data-ttu-id="70e91-114">每個 Azure 儲存體帳戶中都會提供兩個金鑰，讓您可以在重新產生一個金鑰的同時，仍允許使用另一個金鑰存取儲存體。</span><span class="sxs-lookup"><span data-stu-id="70e91-114">Two keys are provided in each Azure storage account so that you can regenerate one key while still allowing access to storage using the other key.</span></span>

## <a name="regenerate-a-key-in-a-storage-account"></a><span data-ttu-id="70e91-115">在儲存體帳戶中重新產生金鑰</span><span class="sxs-lookup"><span data-stu-id="70e91-115">Regenerate a key in a storage account</span></span>

```java
// regenerate the first key in a storage account and return an updated list of keys 
List<StorageAccountKey> updatedStorageAccountKeys =
    storageAccount.regenerateKey(storageAccountKeys.get(0).keyName());
```

<span data-ttu-id="70e91-116">您必須在產生新的金鑰後，以新的金鑰更新所有的 Azure 資源和應用程式。</span><span class="sxs-lookup"><span data-stu-id="70e91-116">You must update all Azure resources and applications with the new key after generating a new one.</span></span>

## <a name="list-all-storage-accounts-in-a-resource-group"></a><span data-ttu-id="70e91-117">列出資源群組中的所有儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70e91-117">List all storage accounts in a resource group</span></span>
```java
// get a list of accounts in a resource group , log info about each one
List<StorageAccount> accounts = azure.storageAccounts().listByResourceGroup(rgName);
for (StorageAccount sa : accounts) {
    System.out.println("Storage Account " + sa.name() + " created @ " + sa.creationTime());
}
```

<span data-ttu-id="70e91-118">[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) 提供一組實用的方法，供您檢查儲存體帳戶的組態。</span><span class="sxs-lookup"><span data-stu-id="70e91-118">[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) provides a set of useful methods to inspect the configuration of a storage account.</span></span>

## <a name="delete-a-storage-account"></a><span data-ttu-id="70e91-119">刪除儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70e91-119">Delete a storage account</span></span>
```java
// delete by ID when you already have a storage account object
azure.storageAccounts().deleteById(storageAccount.id());

// delete by resource group and account name if you don't have an account object
azure.storageAccounts().deleteByResourceGroup(rgName,accountName);
```

> [!NOTE]
> <span data-ttu-id="70e91-120">若有使用中的磁碟映像連線至虛擬機器，或是有磁碟正由其他構件使用中，儲存體帳戶就可能無法透過這些方法來移除。</span><span class="sxs-lookup"><span data-stu-id="70e91-120">Storage accounts with in-use disk images connected to virtual machines or disks in use by other artifacts may not remove with these methods.</span></span> <span data-ttu-id="70e91-121">請先將儲存體與這些資源中斷連結，再移除帳戶。</span><span class="sxs-lookup"><span data-stu-id="70e91-121">Detach the storage from these resources before removing the account.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="70e91-122">範例的說明</span><span class="sxs-lookup"><span data-stu-id="70e91-122">Sample explanation</span></span>

<span data-ttu-id="70e91-123">[GitHub 上的程式碼範例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)：</span><span class="sxs-lookup"><span data-stu-id="70e91-123">[The sample code on GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts):</span></span>

- <span data-ttu-id="70e91-124">建立儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70e91-124">creates a storage account</span></span>
- <span data-ttu-id="70e91-125">讀取並重新產生存取金鑰</span><span class="sxs-lookup"><span data-stu-id="70e91-125">reads and regenerates access keys</span></span>
- <span data-ttu-id="70e91-126">列出資源群組中的所有儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70e91-126">lists all storage accounts in a resource group</span></span>
- <span data-ttu-id="70e91-127">刪除儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="70e91-127">deletes the storage account</span></span> 

| <span data-ttu-id="70e91-128">範例中使用的類別</span><span class="sxs-lookup"><span data-stu-id="70e91-128">Class used in sample</span></span> | <span data-ttu-id="70e91-129">注意</span><span class="sxs-lookup"><span data-stu-id="70e91-129">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="70e91-130">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="70e91-130">StorageAccount</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account)  | <span data-ttu-id="70e91-131">用來表示 Azure 儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="70e91-131">Representation of an Azure storage account.</span></span> <span data-ttu-id="70e91-132">請使用該類別的方法來取得有關儲存體帳戶的資訊。</span><span class="sxs-lookup"><span data-stu-id="70e91-132">Use the methods in the class to get information about the storage account.</span></span>
| [<span data-ttu-id="70e91-133">StorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="70e91-133">StorageAccountKey</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account_key) | <span data-ttu-id="70e91-134">`StorageAccount.getKeys()` 會傳回儲存體帳戶金鑰。</span><span class="sxs-lookup"><span data-stu-id="70e91-134">`StorageAccount.getKeys()` returns the storage account keys.</span></span> <span data-ttu-id="70e91-135">在 `StorageAccount` 中使用 `regenerateKey` 方法來更新金鑰。</span><span class="sxs-lookup"><span data-stu-id="70e91-135">Use the `regenerateKey` methods in `StorageAccount` to update the keys.</span></span>

## <a name="next-steps"></a><span data-ttu-id="70e91-136">後續步驟</span><span class="sxs-lookup"><span data-stu-id="70e91-136">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]