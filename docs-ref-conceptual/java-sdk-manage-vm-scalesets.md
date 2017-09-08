---
title: "使用 Java 管理虛擬機器擴展集 | Microsoft Docs"
description: "使用適用於 Java 的 Azure SDK 來管理 Azure 虛擬機器擴展集的程式碼範例"
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 4653726b387369c18942b6c11392f15b9f0351f3
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="manage-azure-virtual-machine-scale-sets-from-your-java-applications"></a><span data-ttu-id="9a40d-103">從 Java 應用程式管理 Azure 虛擬機器擴展集</span><span class="sxs-lookup"><span data-stu-id="9a40d-103">Manage Azure virtual machine scale sets from your Java applications</span></span>

<span data-ttu-id="9a40d-104">[此範例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets)使用 [Java 管理程式庫](https://github.com/Azure/azure-sdk-for-java)來建立[虛擬機器擴展集](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview)。</span><span class="sxs-lookup"><span data-stu-id="9a40d-104">[This sample](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets) creates a  [virtual machine scale set](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) using the [Java management libraries](https://github.com/Azure/azure-sdk-for-java).</span></span> 

## <a name="run-the-sample"></a><span data-ttu-id="9a40d-105">執行範例</span><span class="sxs-lookup"><span data-stu-id="9a40d-105">Run the sample</span></span>

<span data-ttu-id="9a40d-106">建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="9a40d-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="9a40d-107">然後，執行：</span><span class="sxs-lookup"><span data-stu-id="9a40d-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets.git
cd compute-java-manage-virtual-machine-scale-sets
mvn clean compile exec:java
```

<span data-ttu-id="9a40d-108">檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)。</span><span class="sxs-lookup"><span data-stu-id="9a40d-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="9a40d-109">使用 Azure 進行驗證</span><span class="sxs-lookup"><span data-stu-id="9a40d-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-virtual-network-for-the-scale-set"></a><span data-ttu-id="9a40d-110">為擴展集建立虛擬網路</span><span class="sxs-lookup"><span data-stu-id="9a40d-110">Create a virtual network for the scale set</span></span>

```java
Network network = azure.networks().define(vnetName)
                    .withRegion(region)
                    .withNewResourceGroup(rgName)
                    .withAddressSpace("172.16.0.0/16")
                    .defineSubnet("Front-end")
                    .withAddressPrefix("172.16.1.0/24")
                    .attach()
                    .create();
```

<span data-ttu-id="9a40d-111">請先設定虛擬網路和負載平衡器，再建立擴展集定義。</span><span class="sxs-lookup"><span data-stu-id="9a40d-111">Set up a virtual network and load balancer before creating the scale set definition.</span></span> <span data-ttu-id="9a40d-112">擴展集會使用這些資源來進行其初始設定。</span><span class="sxs-lookup"><span data-stu-id="9a40d-112">The scale set uses these resources for its initial configuration.</span></span>

## <a name="create-a-load-balancer-to-distribute-load-across-the-scale-set"></a><span data-ttu-id="9a40d-113">建立負載平衡器以跨擴展集分散負載</span><span class="sxs-lookup"><span data-stu-id="9a40d-113">Create a load balancer to distribute load across the scale set</span></span>

```java
LoadBalancer loadBalancer1 = azure.loadBalancers().define(loadBalancerName1)
                    .withRegion(region)
                    .withExistingResourceGroup(rgName)
                    // assign a public IP address to the load balancer
                    .definePublicFrontend(frontendName)
                        .withExistingPublicIPAddress(publicIPAddress)
                        .attach()
                    // Add two backend address pools
                    .defineBackend(backendPoolName1)
                        .attach()
                    .defineBackend(backendPoolName2)
                        .attach()
                    // Add two health probes on 80 and 443
                    .defineHttpProbe(httpProbe)
                        .withRequestPath("/")
                        .withPort(80)
                        .attach()
                    .defineHttpProbe(httpsProbe)
                        .withRequestPath("/")
                        .withPort(443)
                        .attach()

                    // balance HTTP and HTTPs traffic
                    .defineLoadBalancingRule(httpLoadBalancingRule)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPort(80)
                        .withProbe(httpProbe)
                        .withBackend(backendPoolName1)
                        .attach()
                    .defineLoadBalancingRule(httpsLoadBalancingRule)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPort(443)
                        .withProbe(httpsProbe)
                        .withBackend(backendPoolName2)
                        .attach()

                    // Add NAT definitions to enable SSH and telnet to the VMs 
                    .defineInboundNatPool(natPool50XXto22)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPortRange(5000, 5099)
                        .withBackendPort(22)
                        .attach()
                    .defineInboundNatPool(natPool60XXto23)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPortRange(6000, 6099)
                        .withBackendPort(23)
                        .attach()
                    .create();
```

 <span data-ttu-id="9a40d-114">負載平衡器會定義兩個後端網路位址集區，一個用來跨 HTTP 平衡負載 (`backendPoolName1`)，另一個用來跨 HTTPS 平衡負載 (`backendPoolName2`)。</span><span class="sxs-lookup"><span data-stu-id="9a40d-114">The load balancer defines two backend network address pools-one to balance load across HTTP (`backendPoolName1`) and the other to balance load across HTTPS (`backendPoolName2`).</span></span>  <span data-ttu-id="9a40d-115">`defineHttpProbe()` 方法會在負載平衡器上設定[健康情況探查端點](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)。</span><span class="sxs-lookup"><span data-stu-id="9a40d-115">The `defineHttpProbe()` methods set up [health probe endpoints](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) on the load balancers.</span></span> <span data-ttu-id="9a40d-116">NAT 規則會在擴展集虛擬機器上公開連接埠 22 和 23，以供進行 telnet 和 SSH 存取。</span><span class="sxs-lookup"><span data-stu-id="9a40d-116">NAT rules expose ports 22 and 23 on the scale set virtual machines for telnet and SSH access.</span></span>

## <a name="create-a-scale-set"></a><span data-ttu-id="9a40d-117">建立擴展集</span><span class="sxs-lookup"><span data-stu-id="9a40d-117">Create a scale set</span></span>
 
```java
 // Create a virtual machine scale set with three virtual machines
 // And, install Apache Web servers on them
VirtualMachineScaleSet virtualMachineScaleSet = azure.virtualMachineScaleSets()
       .define(vmssName)
                .withRegion(region)
                .withExistingResourceGroup(rgName)
                .withSku(VirtualMachineScaleSetSkuTypes.STANDARD_D3_V2)
                .withExistingPrimaryNetworkSubnet(network, "Front-end")
                .withExistingPrimaryInternetFacingLoadBalancer(loadBalancer1)
                .withPrimaryInternetFacingLoadBalancerBackends(backendPoolName1, backendPoolName2)
                .withPrimaryInternetFacingLoadBalancerInboundNatPools(natPool50XXto22, natPool60XXto23)
                .withoutPrimaryInternalLoadBalancer()
                .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                .withRootUsername(userName)
                .withSsh(sshKey)
                .withNewDataDisk(100)
                .withNewDataDisk(100, 1, CachingTypes.READ_WRITE)
                .withNewDataDisk(100, 2, 
                     CachingTypes.READ_WRITE, StorageAccountTypes.STANDARD_LRS)
                .withCapacity(3)
                // Use a VM extension to install Apache Web servers
                .defineNewExtension("CustomScriptForLinux")
                    .withPublisher("Microsoft.OSTCExtensions")
                    .withType("CustomScriptForLinux")
                    .withVersion("1.4")
                    .withMinorVersionAutoUpgrade()
                    .withPublicSetting("fileUris", fileUris)
                    .withPublicSetting("commandToExecute", installCommand)
                    .attach()
                .create();
```

<span data-ttu-id="9a40d-118">使用上一個步驟所建立的虛擬網路定義和負載平衡器定義，來建立具有三個 Linux 執行個體、且每個執行個體各有三個 100 GB 資料磁碟的擴展集 (`withCapacity(3)`)。</span><span class="sxs-lookup"><span data-stu-id="9a40d-118">Use the virtual network definition and load balancer definitions created in the previous step to create a scale set with three Linux instances (`withCapacity(3)`) and three 100GB data disks each.</span></span> <span data-ttu-id="9a40d-119">`defineNewExtension()` 方法鏈結會在每個 VM 上安裝 Apache Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="9a40d-119">The `defineNewExtension()` method chain installs the Apache web server on each VM.</span></span>

## <a name="list-virtual-machine-scale-set-network-interfaces"></a><span data-ttu-id="9a40d-120">列出虛擬機器擴展集網路介面</span><span class="sxs-lookup"><span data-stu-id="9a40d-120">List virtual machine scale set network interfaces</span></span>

```java
// List network interfaces on the scale set and iterate through them
PagedList<VirtualMachineScaleSetNetworkInterface> vmssNics = 
     virtualMachineScaleSet.listNetworkInterfaces();
for (VirtualMachineScaleSetNetworkInterface vmssNic : vmssNics) {
    System.out.println(vmssNic.id());
}
```

## <a name="get-ssh-connection-strings-for-each-scale-set-virtual-machine"></a><span data-ttu-id="9a40d-121">取得每個擴展集虛擬機器的 SSH 連接字串</span><span class="sxs-lookup"><span data-stu-id="9a40d-121">Get SSH connection strings for each scale set virtual machine</span></span>

```java
for (VirtualMachineScaleSetVM instance : virtualMachineScaleSet.virtualMachines().list()) {
    System.out.println("Scale set virtual machine instance #" + instance.instanceId());
    System.out.println(instance.id());
    PagedList<VirtualMachineScaleSetNetworkInterface> networkInterfaces = 
         instance.listNetworkInterfaces();
    // Pick the first NIC on the instance and use its primary IP address
    VirtualMachineScaleSetNetworkInterface networkInterface = networkInterfaces.get(0);
    for (VirtualMachineScaleSetNicIPConfiguration ipConfig : networkInterface.ipConfigurations().values()) {
        if (ipConfig.isPrimary()) {
            List<LoadBalancerInboundNatRule> natRules = ipConfig.listAssociatedLoadBalancerInboundNatRules();
            for (LoadBalancerInboundNatRule natRule : natRules) {
                // find rule matching the inbound SSH port on the backend for the IP address
                // and return the SSH connection string to that port on the load balancer
                if (natRule.backendPort() == 22) {
                    System.out.println("SSH connection string: " + userName + 
                        "@" + publicIPAddress.fqdn() + ":" + natRule.frontendPort());
                break;
                }
            }
            break;
        }
    }
}
```

<span data-ttu-id="9a40d-122">稍早建立的 NAT 集區會將虛擬機器上的 SSH 和 telnet 連接埠 (分別為 22 和 23) 對應到負載平衡器上的連接埠。</span><span class="sxs-lookup"><span data-stu-id="9a40d-122">The NAT pool created earlier mapped the SSH and telnet ports (22 and 23, respectively) on the virtual machines to ports on the load balancer.</span></span> <span data-ttu-id="9a40d-123">此程式碼會為每個虛擬機器建置 SSH 連接字串。</span><span class="sxs-lookup"><span data-stu-id="9a40d-123">This code builds the SSH connection string for each virtual machine.</span></span>

## <a name="stop-the-virtual-machine-scale-set"></a><span data-ttu-id="9a40d-124">停止虛擬機器擴展集</span><span class="sxs-lookup"><span data-stu-id="9a40d-124">Stop the virtual machine scale set</span></span>

```java
// stop (not deallocate) all scale set instances
virtualMachineScaleSet.powerOff();
```

<span data-ttu-id="9a40d-125">已停止的虛擬機器仍會取用保留的資源。</span><span class="sxs-lookup"><span data-stu-id="9a40d-125">Stopped virtual machines continue to consume reserved resources.</span></span> <span data-ttu-id="9a40d-126">請使用 `deallocate()` 來停止虛擬機器上的作業系統，並釋放其計算資源。</span><span class="sxs-lookup"><span data-stu-id="9a40d-126">Use `deallocate()` to stop the operating system on the virtual machines and release their compute resources.</span></span>

## <a name="deallocate-the-virtual-machine-scale-set"></a><span data-ttu-id="9a40d-127">將虛擬機器擴展集解除配置</span><span class="sxs-lookup"><span data-stu-id="9a40d-127">Deallocate the virtual machine scale set</span></span>

```java
// deallocate the virtual machine scale set
virtualMachineScaleSet.deallocate();
```

<span data-ttu-id="9a40d-128">Deallocate() 會關閉虛擬機器上的作業系統，並釋放擴展集執行個體所使用的計算和網路 (例如 IP 位址) 資源。</span><span class="sxs-lookup"><span data-stu-id="9a40d-128">Deallocate() shuts down the operating system on the virtual machines and frees up the compute and network resources (such as IP addresses) used by the scale set instances.</span></span> <span data-ttu-id="9a40d-129">若有任何磁碟 (包括作業系統) 連結到虛擬機器，則會繼續產生儲存費用。</span><span class="sxs-lookup"><span data-stu-id="9a40d-129">You continue to accrue storage charges for any disks (including the OS) attached to the virtual machines.</span></span>

## <a name="start-a-virtual-machine-scale-set"></a><span data-ttu-id="9a40d-130">啟動虛擬機器擴展集</span><span class="sxs-lookup"><span data-stu-id="9a40d-130">Start a virtual machine scale set</span></span>

```java
// start a deallocated or stopped virtual machine scale set
virtualMachineScaleSet.start();
```

## <a name="update-the-number-of-virtual-machines-instances-in-the-scale-set"></a><span data-ttu-id="9a40d-131">更新擴展集內的虛擬機器執行個體數目</span><span class="sxs-lookup"><span data-stu-id="9a40d-131">Update the number of virtual machines instances in the scale set</span></span>
```java
// increase the number of virtual machine scale set instances from three to six
virtualMachineScaleSet.update()
                    .withCapacity(6)
                    .apply();
```

<span data-ttu-id="9a40d-132">使用 `withCapacity()` 調整擴展集內的虛擬機器數目，並使用 `withSku()` 調整每個虛擬機器的容量。</span><span class="sxs-lookup"><span data-stu-id="9a40d-132">Scale the number of virtual machines in the scale set using `withCapacity()` and scale the capacity of each virtual machine using `withSku()`.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="9a40d-133">範例的說明</span><span class="sxs-lookup"><span data-stu-id="9a40d-133">Sample explanation</span></span>

<span data-ttu-id="9a40d-134">[程式碼範例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)會先為擴展集建立虛擬網路以跨虛擬機器進行通訊，並建立負載平衡器以跨虛擬機器來分散流量。</span><span class="sxs-lookup"><span data-stu-id="9a40d-134">[The sample code](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java) first creates a virtual network for the scale set to communicate across and a load balancer to distribute traffic across the virtual machines.</span></span> <span data-ttu-id="9a40d-135">`azure.virtualMachineScaleSets().define()...create()` 方法鏈結會建立擴展集，並為其配置三個執行 Apache Web 伺服器的 Linux 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9a40d-135">The `azure.virtualMachineScaleSets().define()...create()` method chain creates the scale set with three Linux instances running the Apache web server.</span></span>    
   
| <span data-ttu-id="9a40d-136">範例中使用的類別</span><span class="sxs-lookup"><span data-stu-id="9a40d-136">Class used in sample</span></span> | <span data-ttu-id="9a40d-137">注意事項</span><span class="sxs-lookup"><span data-stu-id="9a40d-137">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="9a40d-138">VirtualMachineScaleSet</span><span class="sxs-lookup"><span data-stu-id="9a40d-138">VirtualMachineScaleSet</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set) | <span data-ttu-id="9a40d-139">查詢、啟動、停止、更新和刪除擴展集內的所有虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="9a40d-139">Query, start, stop, update and delete all virtual machines in the scale set.</span></span>
| [<span data-ttu-id="9a40d-140">VirtualMachineScaleSetVM</span><span class="sxs-lookup"><span data-stu-id="9a40d-140">VirtualMachineScaleSetVM</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_v_m) | <span data-ttu-id="9a40d-141">擷取自 `virtualMachineScaleSet.virtualMachines().get()` 或 `list()`，可讓您在擴展集內查詢、啟動、停止、設定和刪除虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="9a40d-141">Retrieved from `virtualMachineScaleSet.virtualMachines().get()` or `list()`, allows you to query, start, stop, configure and delete virtual machines in the scale set.</span></span>
| [<span data-ttu-id="9a40d-142">VirtualMachineScaleSetNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="9a40d-142">VirtualMachineScaleSetNetworkInterface</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_network_interface) | <span data-ttu-id="9a40d-143">從 `virtualMachineScaleSet.listNetworkInterfaces()` 傳回、並以唯讀方式表示的擴展集內虛擬機器的網路介面。</span><span class="sxs-lookup"><span data-stu-id="9a40d-143">Returned from `virtualMachineScaleSet.listNetworkInterfaces()`, read-only representation of a network interface on a virtual machine in a scale set.</span></span>
| [<span data-ttu-id="9a40d-144">VirtualMachineScaleSetSkuTypes</span><span class="sxs-lookup"><span data-stu-id="9a40d-144">VirtualMachineScaleSetSkuTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_sku_types) | <span data-ttu-id="9a40d-145">靜態欄位的類別，用來設定[虛擬機器擴展集](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/)，以供定義擴展集成員可以取用多少資源。</span><span class="sxs-lookup"><span data-stu-id="9a40d-145">Class of static fields used to set the [virtual machine scale set tier](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) used to define how much resources scale set members can consume.</span></span>
| [<span data-ttu-id="9a40d-146">VirtualMachineScaleSetNicIpConfiguration</span><span class="sxs-lookup"><span data-stu-id="9a40d-146">VirtualMachineScaleSetNicIpConfiguration</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_nic_i_p_configuration) | <span data-ttu-id="9a40d-147">用來查詢與擴展集虛擬機器上的網路介面相關聯的 IP 組態。</span><span class="sxs-lookup"><span data-stu-id="9a40d-147">Used to query the IP configuration associated with a network interface on a scale set virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a40d-148">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9a40d-148">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]