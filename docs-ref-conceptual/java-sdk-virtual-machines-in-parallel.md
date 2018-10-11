---
title: 跨區域平行建立 VM | Microsoft Docs
description: 使用 Azure SDK for Java 跨不同 Azure 區域平行建立虛擬機器的程式碼範例
author: rloutlaw
manager: douge
ms.assetid: e5a36699-2d96-4571-84f9-a6af13f3c067
ms.service: Azure
ms.devlang: java
ms.topic: article
ms.date: 03/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: e20feb555c3a360eceae60c1569af9a00a5cd027
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893209"
---
# <a name="create-virtual-machines-across-multiple-regions-from-your-java-applications"></a>從 Java 應用程式跨多個區域建立虛擬機器

[此範例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)會使用[適用於 Java 的 Azure 管理程式庫](https://github.com/Azure/azure-sdk-for-java)，跨不同的 Azure 區域平行建立虛擬機器。

> [!IMPORTANT]
> 此範例會跨四個區域建立總共 48 個執行 Ubuntu 16.04 LTS 且[大小為 STANDARD_DS3_V2](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes) 的 VM。 此程式碼範例會先刪除這些虛擬機器再結束。 請務必先[檢查服務限制和配額](http://docs.microsoft.com/azure/azure-subscription-service-limits)，再使用預設的 VM 數執行此範例。

## <a name="run-the-sample"></a>執行範例

建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。 然後，執行：

```
git clone https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel.git
cd compute-java-create-virtual-machines-across-regions-in-parallel
mvn clean compile exec:java
```

檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 進行驗證

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="set-locations-and-counts-for-the-virtual-machines"></a>設定虛擬機器的位置和計數

```java
// use a Map to define how where and how many VMs to create 
Map<Region, Integer> virtualMachinesByLocation = new HashMap<Region, Integer>();

// create 12 virtual machines in four regions
virtualMachinesByLocation.put(Region.US_EAST, 12);
virtualMachinesByLocation.put(Region.US_SOUTH_CENTRAL, 12);
virtualMachinesByLocation.put(Region.US_WEST, 12);
virtualMachinesByLocation.put(Region.US_NORTH_CENTRAL, 12);
```

此範例稍後會使用這個 `Map` 來設定 VM 的全球分佈。

## <a name="create-a-resource-group"></a>建立資源群組 

```java
// logically associate the resources in the sample into a randomly named resource group
final String rgName = SdkContext.randomResourceName("rgCOPD", 24);
ResourceGroup resourceGroup = azure.resourceGroups().define(rgName)
                .withRegion(Region.US_EAST)
                .create();
```

此範例中的每個資源都會由這個資源群組加以管理。 稍後只要刪除這個資源群組就可以輕鬆清除資源。

## <a name="define-the-virtual-machines"></a>定義虛擬機器
```java
// list to store the VirtualMachine definitions
List<Creatable<VirtualMachine>> creatableVirtualMachines = new ArrayList<>();
    
// outer loop: iterate through each region included in the map
for (Map.Entry<Region, Integer> entry : virtualMachinesByLocation.entrySet()) {
    Region region = entry.getKey();
    Integer vmCount = entry.getValue();

    // Define one virtual network Creatable per region for the VMs to share
    String networkName = SdkContext.randomResourceName("vnetCOPD-", 20);
    Creatable<Network> networkCreatable = azure.networks().define(networkName)
           .withRegion(region)
           .withExistingResourceGroup(resourceGroup)
           .withAddressSpace("172.16.0.0/16");

    // Define one storage account Creatable per region for storing VM disks
    String storageAccountName = SdkContext.randomResourceName("stgcopd", 20);
    Creatable<StorageAccount> storageAccountCreatable = azure.storageAccounts()
        .define(storageAccountName)
              .withRegion(region)
              .withExistingResourceGroup(resourceGroup);

    // generate a common prefix for every VM name
    String linuxVMNamePrefix = SdkContext.randomResourceName("vm-", 15);

    // inner loop: iterate once for every VM instance in the region
    for (int i = 1; i <= vmCount; i++) {

        // Create one public IP address Creatable for each VM
        Creatable<PublicIpAddress> publicIpAddressCreatable = azure.publicIpAddresses()
             .define(String.format("%s-%d", linuxVMNamePrefix, i))
             .withRegion(region)
             .withExistingResourceGroup(resourceGroup)
             .withLeafDomainLabel(SdkContext.randomResourceName("pip", 10));

        publicIpCreatableKeys.add(publicIpAddressCreatable.key());

        // Create one virtual machine Creatable 
        Creatable<VirtualMachine> virtualMachineCreatable = azure.virtualMachines()
             .define(String.format("%s-%d", linuxVMNamePrefix, i))
             .withRegion(region)
             .withExistingResourceGroup(resourceGroup)
             .withNewPrimaryNetwork(networkCreatable)
             .withPrimaryPrivateIpAddressDynamic()
             .withNewPrimaryPublicIpAddress(publicIpAddressCreatable)
             .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
             .withRootUsername(userName)
             .withSsh(sshKey)
             .withSize(VirtualMachineSizeTypes.STANDARD_DS3_V2)
             .withNewStorageAccount(storageAccountCreatable);
        // add the virtual machine Creatable to the list     
        creatableVirtualMachines.add(virtualMachineCreatable); 
     }
}
```

上面的外部 `for` 迴圈會逐一查看每個區域，以定義可供該區域中所有虛擬機器使用的虛擬網路 Creatable 和儲存體帳戶 Creatable。 深入了解如何使用 [Creatable](java-sdk-azure-concepts.md#Creatables)，以在使用管理程式庫時只在必要時候建立資源。

內部 `for` 迴圈會取得虛擬機器的公用 IP 位址 Creatable，然後使用先前定義之虛擬網路、儲存體帳戶和公用 IP 位址的 Creatable 來定義虛擬機器 Creatable。  此虛擬機器 Creatable 接著會新增到 `creatableVirtualMachines` 清單。

## <a name="create-the-virtual-machines"></a>建立虛擬機器

```java
// create all virtual machines defined in the list, return all Creatable objects used
// including networks, public IP addresses, and storage accounts
CreatedResources<VirtualMachine> virtualMachines = azure.virtualMachines().create(creatableVirtualMachines);

// list the IDs of each virtual machine created 
for (VirtualMachine virtualMachine : virtualMachines.values()) {
    System.out.println(virtualMachine.id());
}

// call createdRelatedResource(key) to get the resources used to define the virtual machines. 
// Save the key at the time you define the Creatable to use CreatedResources like this
for (String publicIpCreatableKey : publicIpCreatableKeys) {
    PublicIPAddress pip = 
         (PublicIPAddress) virtualMachines.createdRelatedResource(publicIpCreatableKey);
}
```

`azure.virtualMachines().create(creatableVirtualMachines)` 呼叫會跨區域平行建立 `creatableVirtualMachines` 清單中所定義的所有虛擬機器。

使用傳回的 `CreatedResources<VirtualMachine>` 物件來存取 `create()` 方法期間在 Azure 訂用帳戶中所建立的任何資源，而不只是使用傳回的 `VirtualMachine` 類型來存取。 將傳回的值從 `createdRelatedResources()` 轉換成正確的類型。 

在我們的[程式庫概念文章](java-sdk-azure-concepts.md)中深入了解如何使用 `Creatable<T>` 和 `CreatedResources`。

## <a name="delete-the-resource-group"></a>刪除資源群組 

```java
// finally block deletes the resource group before the code exits
// deleting a resource group deletes all resources created in it
finally {
    try {
        System.out.println("Deleting Resource Group: " + rgName);
        azure.resourceGroups().deleteByName(rgName);
        System.out.println("Deleted Resource Group: " + rgName);
    } catch (NullPointerException npe) {
        System.out.println("Did not create any resources in Azure. No clean up is necessary");
    } catch (Exception g) {
        g.printStackTrace();
    }
}
```

此區塊會先刪除範例中建立的資源再將範例結束。

## <a name="sample-explanation"></a>範例的說明

檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)。

此範例使用 `Creatable` 物件來為裝載虛擬機器的每個區域定義虛擬網路和儲存體帳戶。 接著，會針對每個虛擬機器的公用 IP 位址定義 `Creatable` 物件。 此範例會使用這些 `Creatable` 物件定義虛擬機器，並將 VM 定義新增至 `virtualMachineCreatable` 清單。

程式碼將每個虛擬機器定義新增至清單後，`azure.virtualMachines().create(creatableVirtualMachines)` 就會在 Azure 中平行建立每個虛擬機器。

程式碼範例接著會從傳回的 CreatedResources 物件取得所有已建立之虛擬機器的 IP 位址，以建立[流量管理員](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)來將負載分散到所有虛擬機器。 

即使發生錯誤，`finally` 區塊也會從 Azure 訂用帳戶中刪除資源。

| 範例中使用的類別 | 注意
|-------|-------|
| [VirtualMachine](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | 查詢屬性並管理虛擬機器的狀態。 透過 `azure.virtualMachines().list()` 以清單形式擷取，或依名稱或識別碼 `azure.virtualMachines().getByResourceGroup()` 擷取
| [VirtualMachineSizeTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | 靜態值，對應至[虛擬機器大小選項](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)以在定義虛擬機器時作為 `withSize()` 的參數。
| [PublicIpAddress](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._public_i_p_address) | 會透過 `azure.publicIpAddresses().define()` 為每個虛擬機器進行定義，但不會立即建立。 儲存每個 `Creatable` 的金鑰，並在稍後透過 `createdRelatedResource()` 來擷取
| [KnownLinuxVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | 一組 Linux 虛擬機器選項，可在定義虛擬機器時作為 `withPopularLinuxImage()` 方法的參數。
| [網路](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | 此範例會透過 `azure.networks().define()` 為每個區域定義一個虛擬網路。 

## <a name="next-steps"></a>後續步驟

[!INCLUDE [next-steps](includes/java-next-steps.md)]