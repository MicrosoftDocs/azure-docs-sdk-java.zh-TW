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
# <a name="manage-azure-virtual-machines-from-your-java-applications"></a><span data-ttu-id="6098e-103">從 Java 應用程式管理 Azure 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="6098e-103">Manage Azure virtual machines from your Java applications</span></span>

<span data-ttu-id="6098e-104">[此範例](https://github.com/Azure-Samples/compute-java-manage-vm/)使用[適用於 Java 的 Azure 管理程式庫](https://github.com/Azure/azure-sdk-for-java)來建立和使用 Azure 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="6098e-104">[This sample](https://github.com/Azure-Samples/compute-java-manage-vm/) uses the [Azure management libraries for Java](https://github.com/Azure/azure-sdk-for-java) to create and work with Azure virtual machines.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="6098e-105">執行範例</span><span class="sxs-lookup"><span data-stu-id="6098e-105">Run the sample</span></span>

<span data-ttu-id="6098e-106">建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="6098e-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="6098e-107">然後，執行：</span><span class="sxs-lookup"><span data-stu-id="6098e-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-manage-vm.git
cd compute-java-manage-vm
mvn clean compile exec:java
```

<span data-ttu-id="6098e-108">檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java)。</span><span class="sxs-lookup"><span data-stu-id="6098e-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="6098e-109">使用 Azure 進行驗證</span><span class="sxs-lookup"><span data-stu-id="6098e-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-windows-virtual-machine"></a><span data-ttu-id="6098e-110">建立 Windows 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="6098e-110">Create a Windows virtual machine</span></span>

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

<span data-ttu-id="6098e-111">此程式碼：</span><span class="sxs-lookup"><span data-stu-id="6098e-111">This code:</span></span>   

0. <span data-ttu-id="6098e-112">定義可供使用隨機名稱建立 50 GB 大小的 `Disk`，以便用於虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="6098e-112">Defines a `Disk` Creatable with a 50GB size and random name for use with a virtual machine.</span></span>
0. <span data-ttu-id="6098e-113">使用 `azure.virtualMachines().define()..create()` 鏈結來建立 Windows Server 2012 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="6098e-113">Uses the `azure.virtualMachines().define()..create()` chain to create the Windows Server 2012 virtual machine.</span></span> <span data-ttu-id="6098e-114">API 會在建立虛擬機器的同時，建立上一個步驟所定義的 `Disk`。</span><span class="sxs-lookup"><span data-stu-id="6098e-114">The API creates the `Disk` defined in the previous step the same time as the virtual machine.</span></span> <span data-ttu-id="6098e-115">此外，系統還會透過 `withNewDataDisk(10)` 將 10 GB 的資料磁碟連結到虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="6098e-115">A 10GB data disk is also attached to the virtual machine through `withNewDataDisk(10)`.</span></span>

<span data-ttu-id="6098e-116">深入了解如何使用[可建立的<T>物件](java-sdk-azure-concepts.md#Creatables)來定義資源在本機的代表，並只在其他 Azure 資源需要這些物件時才加以建立。</span><span class="sxs-lookup"><span data-stu-id="6098e-116">Learn more about using [Creatable<T> objects](java-sdk-azure-concepts.md#Creatables) to define local representations of resources and create them just as other Azure resources need them.</span></span>

## <a name="stop-start-and-restart-a-virtual-machine"></a><span data-ttu-id="6098e-117">停止、啟動及重新啟動虛擬機器</span><span class="sxs-lookup"><span data-stu-id="6098e-117">Stop, start, and restart a virtual machine</span></span>

```java
// look up a virtual machine by its ID and then restart, stop, and start it
azureVM = azure.getVirtualMachine.getById(windowsVM.id());

azureVM.restart();
azureVM.powerOff();
azureVM.start();
```

<span data-ttu-id="6098e-118">`powerOff()` 會停止虛擬機器的作業系統，但不會將其資源取消配置。</span><span class="sxs-lookup"><span data-stu-id="6098e-118">`powerOff()` stops the virtual machine operating system but does not deallocate its resources.</span></span>

## <a name="add-a-virtual-machine-to-an-existing-network"></a><span data-ttu-id="6098e-119">將虛擬機器新增至現有網路</span><span class="sxs-lookup"><span data-stu-id="6098e-119">Add a virtual machine to an existing network</span></span>

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

<span data-ttu-id="6098e-120">使用 `withPopularLinuxImage` 來定義 Linux VM 而非 Windows VM。</span><span class="sxs-lookup"><span data-stu-id="6098e-120">Use `withPopularLinuxImage` to define a Linux VM instead of a Windows one.</span></span>


## <a name="list-virtual-machines"></a><span data-ttu-id="6098e-121">列出虛擬機器</span><span class="sxs-lookup"><span data-stu-id="6098e-121">List virtual machines</span></span>

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

<span data-ttu-id="6098e-122">使用 `azure.virtualMachines().list()` 來列出訂用帳戶的所有虛擬機器，並逐一查看 `tags()` 所傳回的對應以便跨資源群組管理已標記的虛擬機器集合。</span><span class="sxs-lookup"><span data-stu-id="6098e-122">List all virtual machines for a subscription using `azure.virtualMachines().list()` and iterate through the Map returned by `tags()` to manage tagged collections of virtual machines across resource groups.</span></span>

## <a name="update-a-virtual-machine"></a><span data-ttu-id="6098e-123">更新虛擬機器</span><span class="sxs-lookup"><span data-stu-id="6098e-123">Update a virtual machine</span></span>

```java
// add a 10GB data disk to the virtual machine
windowsVM.update()
     .withNewDataDisk(10)
     .apply();
```

<span data-ttu-id="6098e-124">使用 `update()...apply()` 以及用來設定透過 `define()...create()` 所建立之虛擬機器時所使用的相同方法，來更新虛擬機器組態。</span><span class="sxs-lookup"><span data-stu-id="6098e-124">Update the virtual machine configuration using `update()...apply()` and the same methods used to configure the virtual machine when created through `define()...create()`.</span></span>

## <a name="delete-a-virtual-machine"></a><span data-ttu-id="6098e-125">刪除虛擬機器</span><span class="sxs-lookup"><span data-stu-id="6098e-125">Delete a virtual machine</span></span>

```java
// delete by ID if you already are working with the VM object
azure.virtualMachines().deleteById(windowsVM.id());

// delete by resource group and name
azure.virtualMachines().deleteByResourceGroup(rgName,windowsVmName);
```

## <a name="sample-explanation"></a><span data-ttu-id="6098e-126">範例的說明</span><span class="sxs-lookup"><span data-stu-id="6098e-126">Sample explanation</span></span>

<span data-ttu-id="6098e-127">[此程式碼範例](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java) 會建立具有 50 GB 資料磁碟的 Windows 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="6098e-127">[The sample code](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java) creates a Windows virtual machine with a 50GB data disk.</span></span> <span data-ttu-id="6098e-128">此範例接著會建立第二個 10 GB 資料磁碟，然後將其連結到這個 Windows 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="6098e-128">The sample then creates a second 10GB data disk and attaches it to this Windows virtual machine.</span></span>
<span data-ttu-id="6098e-129">然後此範例會在 Windows 虛擬機器所在的相同虛擬網路中建立 Linux 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="6098e-129">Then the sample creates a Linux virtual machine in the same virtual network as the Windows virtual machine.</span></span>

<span data-ttu-id="6098e-130">此範例會記錄這兩個虛擬機器的相關資訊，並在兩者完成前將其同時刪除。</span><span class="sxs-lookup"><span data-stu-id="6098e-130">The sample logs information about both virtual machines and deletes them both before completing.</span></span>

| <span data-ttu-id="6098e-131">範例中使用的類別</span><span class="sxs-lookup"><span data-stu-id="6098e-131">Class used in sample</span></span> | <span data-ttu-id="6098e-132">注意</span><span class="sxs-lookup"><span data-stu-id="6098e-132">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="6098e-133">VirtualMachine</span><span class="sxs-lookup"><span data-stu-id="6098e-133">VirtualMachine</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | <span data-ttu-id="6098e-134">查詢屬性並管理虛擬機器的狀態。</span><span class="sxs-lookup"><span data-stu-id="6098e-134">Query properties and manage state of virtual machines.</span></span> <span data-ttu-id="6098e-135">使用 `azure.virtualMachines().list()` 以清單形式擷取，或依名稱或識別碼 `azure.virtualMachines().getByResourceGroup()` 擷取</span><span class="sxs-lookup"><span data-stu-id="6098e-135">Retrieved in list form  with`azure.virtualMachines().list()` or by name or ID `azure.virtualMachines().getByResourceGroup()`</span></span>
| [<span data-ttu-id="6098e-136">VirtualMachineSizeTypes</span><span class="sxs-lookup"><span data-stu-id="6098e-136">VirtualMachineSizeTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | <span data-ttu-id="6098e-137">對應至[虛擬機器大小選項](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)且具有靜態值的類別，可供 `withSize()` 方法用來定義配置給 VM 的資源。</span><span class="sxs-lookup"><span data-stu-id="6098e-137">Class with static values that map to [virtual machine size options](https://azure.microsoft.com/pricing/details/virtual-machines/linux/), used by the `withSize()` method to define the resources allocated to the VM.</span></span>
| [<span data-ttu-id="6098e-138">磁碟</span><span class="sxs-lookup"><span data-stu-id="6098e-138">Disk</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk) | <span data-ttu-id="6098e-139">使用 `withData()` 建立磁碟來儲存資料，或在定義磁碟時使用適當的 `withLinux` 或 `withWindows` 方法建立作業系統映像。</span><span class="sxs-lookup"><span data-stu-id="6098e-139">Create a disk to store data using `withData()` or operating system image using the appropriate `withLinux` or `withWindows` method when defining the disk.</span></span> <span data-ttu-id="6098e-140">在建立磁碟時將磁碟連結至虛擬機器 (`using withNewDataDisk` 或 `withExistingDataDisk`)，或在建立磁碟後透過 VirtualMachine 物件上的 `update()..apply()` 將磁碟連結至虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="6098e-140">Attach disks to virtual machines either at the time of creation (`using withNewDataDisk` or `withExistingDataDisk`) or after creation by `update()..apply()` on the VirtualMachine object.</span></span>
| [<span data-ttu-id="6098e-141">DiskSkuTypes</span><span class="sxs-lookup"><span data-stu-id="6098e-141">DiskSkuTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk_sku_types) | <span data-ttu-id="6098e-142">具有靜態值的類別，用以定義具有標準或[進階](https://docs.microsoft.com/azure/storage/storage-premium-storage)儲存方案的磁碟。</span><span class="sxs-lookup"><span data-stu-id="6098e-142">Class with static values to define a disk with a standard or [premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) storage plan.</span></span>
| [<span data-ttu-id="6098e-143">KnownLinuxVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="6098e-143">KnownLinuxVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | <span data-ttu-id="6098e-144">具有一組 Linux 虛擬機器選項，以便在定義虛擬機器時用於 `withPopularLinuxImage()` 方法的類別。</span><span class="sxs-lookup"><span data-stu-id="6098e-144">Class with a set of Linux virtual machine options for use with the `withPopularLinuxImage()` method when defining a virtual machine.</span></span>
| [<span data-ttu-id="6098e-145">KnownWindowsVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="6098e-145">KnownWindowsVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_windows_virtual_machine_image) | <span data-ttu-id="6098e-146">具有一組 Windows 虛擬機器映像選項，以便在定義虛擬機器時用於 `withPopularWindowsImage()` 方法的類別。</span><span class="sxs-lookup"><span data-stu-id="6098e-146">Class with a set of Windows virtual machine image options for use with the `withPopularWindowsImage()` method when defining a virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6098e-147">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6098e-147">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]