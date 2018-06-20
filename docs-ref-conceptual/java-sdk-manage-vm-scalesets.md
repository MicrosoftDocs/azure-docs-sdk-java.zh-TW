---
title: 使用 Java 管理虛擬機器擴展集 | Microsoft Docs
description: 使用適用於 Java 的 Azure SDK 來管理 Azure 虛擬機器擴展集的程式碼範例
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
ms.locfileid: "21931154"
---
# <a name="manage-azure-virtual-machine-scale-sets-from-your-java-applications"></a>從 Java 應用程式管理 Azure 虛擬機器擴展集

[此範例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets)使用 [Java 管理程式庫](https://github.com/Azure/azure-sdk-for-java)來建立[虛擬機器擴展集](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview)。 

## <a name="run-the-sample"></a>執行範例

建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。 然後，執行：

```
git clone https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets.git
cd compute-java-manage-virtual-machine-scale-sets
mvn clean compile exec:java
```

檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 進行驗證

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-virtual-network-for-the-scale-set"></a>為擴展集建立虛擬網路

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

請先設定虛擬網路和負載平衡器，再建立擴展集定義。 擴展集會使用這些資源來進行其初始設定。

## <a name="create-a-load-balancer-to-distribute-load-across-the-scale-set"></a>建立負載平衡器以跨擴展集分散負載

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

 負載平衡器會定義兩個後端網路位址集區，一個用來跨 HTTP 平衡負載 (`backendPoolName1`)，另一個用來跨 HTTPS 平衡負載 (`backendPoolName2`)。  `defineHttpProbe()` 方法會在負載平衡器上設定[健康情況探查端點](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)。 NAT 規則會在擴展集虛擬機器上公開連接埠 22 和 23，以供進行 telnet 和 SSH 存取。

## <a name="create-a-scale-set"></a>建立擴展集
 
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

使用上一個步驟所建立的虛擬網路定義和負載平衡器定義，來建立具有三個 Linux 執行個體、且每個執行個體各有三個 100 GB 資料磁碟的擴展集 (`withCapacity(3)`)。 `defineNewExtension()` 方法鏈結會在每個 VM 上安裝 Apache Web 伺服器。

## <a name="list-virtual-machine-scale-set-network-interfaces"></a>列出虛擬機器擴展集網路介面

```java
// List network interfaces on the scale set and iterate through them
PagedList<VirtualMachineScaleSetNetworkInterface> vmssNics = 
     virtualMachineScaleSet.listNetworkInterfaces();
for (VirtualMachineScaleSetNetworkInterface vmssNic : vmssNics) {
    System.out.println(vmssNic.id());
}
```

## <a name="get-ssh-connection-strings-for-each-scale-set-virtual-machine"></a>取得每個擴展集虛擬機器的 SSH 連接字串

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

稍早建立的 NAT 集區會將虛擬機器上的 SSH 和 telnet 連接埠 (分別為 22 和 23) 對應到負載平衡器上的連接埠。 此程式碼會為每個虛擬機器建置 SSH 連接字串。

## <a name="stop-the-virtual-machine-scale-set"></a>停止虛擬機器擴展集

```java
// stop (not deallocate) all scale set instances
virtualMachineScaleSet.powerOff();
```

已停止的虛擬機器仍會取用保留的資源。 請使用 `deallocate()` 來停止虛擬機器上的作業系統，並釋放其計算資源。

## <a name="deallocate-the-virtual-machine-scale-set"></a>將虛擬機器擴展集解除配置

```java
// deallocate the virtual machine scale set
virtualMachineScaleSet.deallocate();
```

Deallocate() 會關閉虛擬機器上的作業系統，並釋放擴展集執行個體所使用的計算和網路 (例如 IP 位址) 資源。 若有任何磁碟 (包括作業系統) 連結到虛擬機器，則會繼續產生儲存費用。

## <a name="start-a-virtual-machine-scale-set"></a>啟動虛擬機器擴展集

```java
// start a deallocated or stopped virtual machine scale set
virtualMachineScaleSet.start();
```

## <a name="update-the-number-of-virtual-machines-instances-in-the-scale-set"></a>更新擴展集內的虛擬機器執行個體數目
```java
// increase the number of virtual machine scale set instances from three to six
virtualMachineScaleSet.update()
                    .withCapacity(6)
                    .apply();
```

使用 `withCapacity()` 調整擴展集內的虛擬機器數目，並使用 `withSku()` 調整每個虛擬機器的容量。

## <a name="sample-explanation"></a>範例的說明

[程式碼範例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)會先為擴展集建立虛擬網路以跨虛擬機器進行通訊，並建立負載平衡器以跨虛擬機器來分散流量。 `azure.virtualMachineScaleSets().define()...create()` 方法鏈結會建立擴展集，並為其配置三個執行 Apache Web 伺服器的 Linux 執行個體。    
   
| 範例中使用的類別 | 注意事項
|-------|-------|
| [VirtualMachineScaleSet](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set) | 查詢、啟動、停止、更新和刪除擴展集內的所有虛擬機器。
| [VirtualMachineScaleSetVM](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_v_m) | 擷取自 `virtualMachineScaleSet.virtualMachines().get()` 或 `list()`，可讓您在擴展集內查詢、啟動、停止、設定和刪除虛擬機器。
| [VirtualMachineScaleSetNetworkInterface](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_network_interface) | 從 `virtualMachineScaleSet.listNetworkInterfaces()` 傳回、並以唯讀方式表示的擴展集內虛擬機器的網路介面。
| [VirtualMachineScaleSetSkuTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_sku_types) | 靜態欄位的類別，用來設定[虛擬機器擴展集](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/)，以供定義擴展集成員可以取用多少資源。
| [VirtualMachineScaleSetNicIpConfiguration](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_nic_i_p_configuration) | 用來查詢與擴展集虛擬機器上的網路介面相關聯的 IP 組態。

## <a name="next-steps"></a>後續步驟

[!INCLUDE [next-steps](includes/java-next-steps.md)]