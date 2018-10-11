---
title: 使用 Java 來管理 Azure 虛擬機器 | Microsoft Docs
description: 使用適用於 Java 的 Azure SDK 來管理 Azure 虛擬機器的程式碼範例
author: rloutlaw
manager: douge
ms.assetid: 88629aee-6279-433e-a08b-4f8e290446d0
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: e3048b3317477f4b1fb8edf93e4bebad6b7fafce
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893609"
---
# <a name="manage-azure-virtual-machines-from-your-java-applications"></a>從 Java 應用程式管理 Azure 虛擬機器

[此範例](https://github.com/Azure-Samples/compute-java-manage-vm/)使用[適用於 Java 的 Azure 管理程式庫](https://github.com/Azure/azure-sdk-for-java)來建立和使用 Azure 虛擬機器。

## <a name="run-the-sample"></a>執行範例

建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。 然後，執行：

```
git clone https://github.com/Azure-Samples/compute-java-manage-vm.git
cd compute-java-manage-vm
mvn clean compile exec:java
```

檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 進行驗證

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-windows-virtual-machine"></a>建立 Windows 虛擬機器

```java
// Prepare a data disk for VM
Disk dataDisk = azure.disks().define(SdkContext.randomResourceName("dsk", 30))
            .withRegion(region)
            .withNewResourceGroup(rgName)
            .withData()
            .withSizeInGB(50)
            .create();

// create the windows virtual machine with the data disk            
VirtualMachine windowsVM = azure.virtualMachines().define(windowsVmName)
            .withRegion(region)
            .withNewResourceGroup(rgName)
            .withNewPrimaryNetwork("10.0.0.0/28")
            .withPrimaryPrivateIpAddressDynamic()
            .withoutPrimaryPublicIpAddress()
            .withPopularWindowsImage(KnownWindowsVirtualMachineImage.WINDOWS_SERVER_2012_R2_DATACENTER)
            .withAdminUsername(userName)
            .withAdminPassword(password)
            .withNewDataDisk(10)
            .withNewDataDisk(dataDiskCreatable)
            .withExistingDataDisk(dataDisk)
            .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
            .create();
```

此程式碼：   

0. 定義可供使用隨機名稱建立 50 GB 大小的 `Disk`，以便用於虛擬機器。
0. 使用 `azure.virtualMachines().define()..create()` 鏈結來建立 Windows Server 2012 虛擬機器。 API 會在建立虛擬機器的同時，建立上一個步驟所定義的 `Disk`。 此外，系統還會透過 `withNewDataDisk(10)` 將 10 GB 的資料磁碟連結到虛擬機器。

深入了解如何使用[可建立的<T>物件](java-sdk-azure-concepts.md#Creatables)來定義資源在本機的代表，並只在其他 Azure 資源需要這些物件時才加以建立。

## <a name="stop-start-and-restart-a-virtual-machine"></a>停止、啟動及重新啟動虛擬機器

```java
// look up a virtual machine by its ID and then restart, stop, and start it
azureVM = azure.getVirtualMachine.getById(windowsVM.id());

azureVM.restart();
azureVM.powerOff();
azureVM.start();
```

`powerOff()` 會停止虛擬機器的作業系統，但不會將其資源取消配置。

## <a name="add-a-virtual-machine-to-an-existing-network"></a>將虛擬機器新增至現有網路

```java
// Get the virtual network the current virtual machine is using
Network network = windowsVM.getPrimaryNetworkInterface().primaryIPConfiguration().getNetwork();

// Create a Linux VM in the same subnet
VirtualMachine linuxVM = azure.virtualMachines().define(linuxVmName)
           .withRegion(region)
           .withExistingResourceGroup(rgName)
           .withExistingPrimaryNetwork(network)
           .withSubnet("subnet1") // default subnet name when no name specified at creation
           .withPrimaryPrivateIPAddressDynamic()
           .withoutPrimaryPublicIPAddress()
           .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
           .withRootUsername(userName)
           .withRootPassword(password)
           .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
           .create();
```

使用 `withPopularLinuxImage` 來定義 Linux VM 而非 Windows VM。


## <a name="list-virtual-machines"></a>列出虛擬機器

```java
// get a list of VMs in the same resource group as an existing VM
String resourceGroupName = windowsVM.resourceGroupName();
PagedList<VirtualMachine> resourceGroupVMs = azure.virtualMachines()
        .listByResourceGroup(resourceGroupName); 

// for each vitual machine in the resource group, log their name and plan
for (VirtualMachine virtualMachine : azure.virtualMachines().listByResourceGroup(resourceGroupName)) {
    System.out.println("VM " + virtualMachine.computerName() + 
        " has plan " + virtualMachine.plan());
}
```

使用 `azure.virtualMachines().list()` 來列出訂用帳戶的所有虛擬機器，並逐一查看 `tags()` 所傳回的對應以便跨資源群組管理已標記的虛擬機器集合。

## <a name="update-a-virtual-machine"></a>更新虛擬機器

```java
// add a 10GB data disk to the virtual machine
windowsVM.update()
     .withNewDataDisk(10)
     .apply();
```

使用 `update()...apply()` 以及用來設定透過 `define()...create()` 所建立之虛擬機器時所使用的相同方法，來更新虛擬機器組態。

## <a name="delete-a-virtual-machine"></a>刪除虛擬機器

```java
// delete by ID if you already are working with the VM object
azure.virtualMachines().deleteById(windowsVM.id());

// delete by resource group and name
azure.virtualMachines().deleteByResourceGroup(rgName,windowsVmName);
```

## <a name="sample-explanation"></a>範例的說明

[此程式碼範例](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java) 會建立具有 50 GB 資料磁碟的 Windows 虛擬機器。 此範例接著會建立第二個 10 GB 資料磁碟，然後將其連結到這個 Windows 虛擬機器。
然後此範例會在 Windows 虛擬機器所在的相同虛擬網路中建立 Linux 虛擬機器。

此範例會記錄這兩個虛擬機器的相關資訊，並在兩者完成前將其同時刪除。

| 範例中使用的類別 | 注意
|-------|-------|
| [VirtualMachine](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | 查詢屬性並管理虛擬機器的狀態。 使用 `azure.virtualMachines().list()` 以清單形式擷取，或依名稱或識別碼 `azure.virtualMachines().getByResourceGroup()` 擷取
| [VirtualMachineSizeTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | 對應至[虛擬機器大小選項](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)且具有靜態值的類別，可供 `withSize()` 方法用來定義配置給 VM 的資源。
| [磁碟](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk) | 使用 `withData()` 建立磁碟來儲存資料，或在定義磁碟時使用適當的 `withLinux` 或 `withWindows` 方法建立作業系統映像。 在建立磁碟時將磁碟連結至虛擬機器 (`using withNewDataDisk` 或 `withExistingDataDisk`)，或在建立磁碟後透過 VirtualMachine 物件上的 `update()..apply()` 將磁碟連結至虛擬機器。
| [DiskSkuTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk_sku_types) | 具有靜態值的類別，用以定義具有標準或[進階](https://docs.microsoft.com/azure/storage/storage-premium-storage)儲存方案的磁碟。
| [KnownLinuxVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | 具有一組 Linux 虛擬機器選項，以便在定義虛擬機器時用於 `withPopularLinuxImage()` 方法的類別。
| [KnownWindowsVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_windows_virtual_machine_image) | 具有一組 Windows 虛擬機器映像選項，以便在定義虛擬機器時用於 `withPopularWindowsImage()` 方法的類別。

## <a name="next-steps"></a>後續步驟

[!INCLUDE [next-steps](includes/java-next-steps.md)]