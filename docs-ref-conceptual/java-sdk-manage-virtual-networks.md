---
title: "使用 Java 來管理 Azure 虛擬網路 | Microsoft Docs"
description: "用來管理 Java 程式碼中 Azure 虛擬網路的程式碼範例"
author: rloutlaw
manager: douge
ms.assetid: 92736911-3df6-46e7-b751-25bb36bf89b9
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 3d21cdd890912c1fc58fc65a79ba972b8327edeb
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="create-and-manage-azure-virtual-networks-from-your-java-apps"></a>從 Java 應用程式建立和管理 Azure 虛擬網路

[此範例](https://github.com/Azure-Samples/network-java-manage-virtual-network)會建立[虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)來隔離您所控制之網路區段上的 Azure 資源。

## <a name="run-the-sample"></a>執行範例

建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。 然後，執行：

```
git clone https://github.com/Azure-Samples/network-java-manage-virtual-network.git
cd network-java-manage-virtual-network
mvn clean compile exec:java
```

檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 進行驗證

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-network-security-group-to-block-internet-traffic"></a>建立網路安全性群組來封鎖網際網路流量

```java
// this NSG definition block traffic to and from the public Internet
NetworkSecurityGroup backEndSubnetNsg = azure.networkSecurityGroups()
              .define(vnet1BackEndSubnetNsgName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .defineRule("DenyInternetInComing")
                        .denyInbound()
                        .fromAddress("INTERNET")
                        .fromAnyPort()
                        .toAnyAddress()
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .defineRule("DenyInternetOutGoing")
                        .denyOutbound()
                        .fromAnyAddress()
                        .fromAnyPort()
                        .toAddress("INTERNET")
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .create();
```

此[網路安全性規則](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)會同時封鎖傳入和傳出的公用網際網路流量。 此網路安全性群組在套用至虛擬網路中的子網路後才會生效。

## <a name="create-a-virtual-network-with-two-subnets"></a>建立具有兩個子網路的虛擬網路

```java
// create the a virtual network with two subnets
// assign the backend subnet a rule blocking public internet traffic
Network virtualNetwork1 = azure.networks().define(vnetName1)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .withAddressSpace("192.168.0.0/16")
                    .withSubnet(vnet1FrontEndSubnetName, "192.168.1.0/24")
                    .defineSubnet(vnet1BackEndSubnetName)
                        .withAddressPrefix("192.168.2.0/24")
                        .withExistingNetworkSecurityGroup(backEndSubnetNsg)
                        .attach()
                    .create();
```

後端子網路會使用下列在網路安全性群組中設定的規則來拒絕網際網路存取。 前端子網路則會使用[預設規則](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)，以允許傳出到網際網路的流量。

## <a name="create-a-network-security-group-to-allow-inbound-http-traffic"></a>建立網路安全性群組來允許輸入 HTTP 流量
```java
// create a rule that allows inbound HTTP and blocks outbound Internet traffic
NetworkSecurityGroup frontEndSubnetNsg = azure.networkSecurityGroups()
               .define(vnet1FrontEndSubnetNsgName)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .defineRule("AllowHttpInComing")
                        .allowInbound()
                        .fromAddress("INTERNET")
                        .fromAnyPort()
                        .toAnyAddress()
                        .toPort(80)
                        .withProtocol(SecurityRuleProtocol.TCP)
                        .attach()
                    .defineRule("DenyInternetOutGoing")
                        .denyOutbound()
                        .fromAnyAddress()
                        .fromAnyPort()
                        .toAddress("INTERNET")
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .create();
```

此網路安全性規則會在連接埠 80 上開放來自公用網際網路的傳入流量，並封鎖所有從網路內流到公用網際網路的傳出流量。 

## <a name="update-a-virtual-network"></a>更新虛擬網路
```java
// update the front end subnet to use the rules in the new network security group
virtualNetwork1.update()
          .updateSubnet(vnet1FrontEndSubnetName)
          .withExistingNetworkSecurityGroup(frontEndSubnetNsg)
          .parent()
          .apply();
```

使用上一個步驟所建立的網路安全性規則，將前端子網路更新為允許傳入 HTTP 流量。

## <a name="create-a-virtual-machine-on-a-subnet"></a>在子網路上建立虛擬機器
```java
// attach the new VM to the front end subnet on the virtual network
VirtualMachine frontEndVM = azure.virtualMachines().define(frontEndVmName)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .withExistingPrimaryNetwork(virtualNetwork1) 
                    .withSubnet(vnet1FrontEndSubnetName)
                    .withPrimaryPrivateIpAddressDynamic()
                    .withNewPrimaryPublicIpAddress(publicIpAddressLeafDnsForFrontEndVm)
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();
```

`withExistingPrimaryNetwork()` 和 `withSubnet()` 會將虛擬機器設定為使用前述步驟中所建立之虛擬網路上的前端子網路。

## <a name="list-virtual-networks-in-a-resource-group"></a>列出資源群組中的虛擬網路
```java
// iterate over every virtual network in the resource group 
for (Network virtualNetwork : azure.networks().listByResourceGroup(rgName)) {
    // for each subnet on the virtual network, log the network address prefix 
    for (Map.Entry<String, Subnet> entry : virtualNetwork.subnets().entrySet()) {
        String subnetName = entry.getKey();
        Subnet subnet = entry.getValue();
        System.out.println("Address prefix for subnet " + subnetName + 
             " is " + subnet.addressPrefix());
    }
}
```       

您可以使用外部集合列出並檢查 `Network` 物件，或使用巢狀的 for-each 迴圈逐一查看每個網路的每個子資源，如此範例所示。

## <a name="delete-a-virtual-network"></a>刪除虛擬網路
```java
// if you already have the virtual network object it is easiest to delete by ID
azure.networks().deleteById(virtualNetwork1.id());

// Delete by resource group and name if you don't have the VirtualMachine object
azure.networks().deleteByResourceGroup(rgName,vnetName1);
```

移除虛擬網路將會刪除網路上的子網路，但不會刪除套用至子網路的網路安全性群組規則。 這些規則定義可以重新套用至其他子網路。

## <a name="sample-explanation"></a>範例的說明

此範例會建立具有兩個子網路、且每個子網路上各有一部虛擬機器的虛擬網路。 後端子網路會與公用網際網路隔絕。 前端子網路則會接受來自網際網路的傳入 HTTP 流量。 虛擬網路中的兩個虛擬機器會透過預設的網路安全性群組規則彼此通訊。

| 範例中使用的類別 | 注意事項
|-------|-------|
| [網路](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | 從 `azure.networks().define()...create()` 所建立之虛擬網路在本機所代表的物件。 使用 `update()...apply()` Fluent 鏈結來更新現有的虛擬網路。
| [子網路](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._subnet) | 在定義或更新網路時使用 `withSubnet()` 於虛擬網路上建立子網路。 從 `Network.subnets().get()` 或 `Network.subnets().entrySet()` 取得代表子網路的物件。 這些物件會有方法可以查詢子網路屬性。
| [NetworkSecurityGroup](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network_security_group) | 使用 `azure.networkSecurityGroups().define()...create()` Fluent 鏈結所建立，然後透過在虛擬網路中更新或建立子網路以套用至子網路。 

## <a name="next-steps"></a>後續步驟

[!INCLUDE [next-steps](includes/java-next-steps.md)]