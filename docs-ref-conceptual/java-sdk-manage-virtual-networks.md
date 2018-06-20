---
title: 使用 Java 來管理 Azure 虛擬網路 | Microsoft Docs
description: 用來管理 Java 程式碼中 Azure 虛擬網路的程式碼範例
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
ms.locfileid: "21931084"
---
# <a name="create-and-manage-azure-virtual-networks-from-your-java-apps"></a><span data-ttu-id="b2ed7-103">從 Java 應用程式建立和管理 Azure 虛擬網路</span><span class="sxs-lookup"><span data-stu-id="b2ed7-103">Create and manage Azure virtual networks from your Java apps</span></span>

<span data-ttu-id="b2ed7-104">[此範例](https://github.com/Azure-Samples/network-java-manage-virtual-network)會建立[虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)來隔離您所控制之網路區段上的 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-104">[This sample](https://github.com/Azure-Samples/network-java-manage-virtual-network) creates a [virtual network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) to isolate your Azure resources on network segment you control.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="b2ed7-105">執行範例</span><span class="sxs-lookup"><span data-stu-id="b2ed7-105">Run the sample</span></span>

<span data-ttu-id="b2ed7-106">建立[驗證檔案](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，並使用電腦上檔案的完整路徑來設定 `AZURE_AUTH_LOCATION` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="b2ed7-107">然後，執行：</span><span class="sxs-lookup"><span data-stu-id="b2ed7-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/network-java-manage-virtual-network.git
cd network-java-manage-virtual-network
mvn clean compile exec:java
```

<span data-ttu-id="b2ed7-108">檢視 [GitHub 上的完整程式碼範例](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java)。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="b2ed7-109">使用 Azure 進行驗證</span><span class="sxs-lookup"><span data-stu-id="b2ed7-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-network-security-group-to-block-internet-traffic"></a><span data-ttu-id="b2ed7-110">建立網路安全性群組來封鎖網際網路流量</span><span class="sxs-lookup"><span data-stu-id="b2ed7-110">Create a network security group to block Internet traffic</span></span>

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

<span data-ttu-id="b2ed7-111">此[網路安全性規則](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)會同時封鎖傳入和傳出的公用網際網路流量。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-111">This [network security rule](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) blocks both inbound and outbound public Internet traffic.</span></span> <span data-ttu-id="b2ed7-112">此網路安全性群組在套用至虛擬網路中的子網路後才會生效。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-112">This network security group will not have an effect until applied to a subnet in your virtual network.</span></span>

## <a name="create-a-virtual-network-with-two-subnets"></a><span data-ttu-id="b2ed7-113">建立具有兩個子網路的虛擬網路</span><span class="sxs-lookup"><span data-stu-id="b2ed7-113">Create a virtual network with two subnets</span></span>

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

<span data-ttu-id="b2ed7-114">後端子網路會使用下列在網路安全性群組中設定的規則來拒絕網際網路存取。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-114">The backend subnet denies Internet access usingfollowing the rules set in the network security group.</span></span> <span data-ttu-id="b2ed7-115">前端子網路則會使用[預設規則](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)，以允許傳出到網際網路的流量。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-115">The front end subnet uses the [default rules](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) which allow outbound traffic to the Internet.</span></span>

## <a name="create-a-network-security-group-to-allow-inbound-http-traffic"></a><span data-ttu-id="b2ed7-116">建立網路安全性群組來允許輸入 HTTP 流量</span><span class="sxs-lookup"><span data-stu-id="b2ed7-116">Create a network security group to allow inbound HTTP traffic</span></span>
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

<span data-ttu-id="b2ed7-117">此網路安全性規則會在連接埠 80 上開放來自公用網際網路的傳入流量，並封鎖所有從網路內流到公用網際網路的傳出流量。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-117">This network security rule opens up inbound traffic on port 80 from the public Internet, and blocks all outbound traffic from inside the network to the public Internet.</span></span> 

## <a name="update-a-virtual-network"></a><span data-ttu-id="b2ed7-118">更新虛擬網路</span><span class="sxs-lookup"><span data-stu-id="b2ed7-118">Update a virtual network</span></span>
```java
// update the front end subnet to use the rules in the new network security group
virtualNetwork1.update()
          .updateSubnet(vnet1FrontEndSubnetName)
          .withExistingNetworkSecurityGroup(frontEndSubnetNsg)
          .parent()
          .apply();
```

<span data-ttu-id="b2ed7-119">使用上一個步驟所建立的網路安全性規則，將前端子網路更新為允許傳入 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-119">Update the front end subnet to allow inbound HTTP traffic using the network security rule created in the previous step.</span></span>

## <a name="create-a-virtual-machine-on-a-subnet"></a><span data-ttu-id="b2ed7-120">在子網路上建立虛擬機器</span><span class="sxs-lookup"><span data-stu-id="b2ed7-120">Create a virtual machine on a subnet</span></span>
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

<span data-ttu-id="b2ed7-121">`withExistingPrimaryNetwork()` 和 `withSubnet()` 會將虛擬機器設定為使用前述步驟中所建立之虛擬網路上的前端子網路。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-121">`withExistingPrimaryNetwork()` and `withSubnet()` configure the virtual machine to use the front-end subnet on the virtual network created in the previous steps.</span></span>

## <a name="list-virtual-networks-in-a-resource-group"></a><span data-ttu-id="b2ed7-122">列出資源群組中的虛擬網路</span><span class="sxs-lookup"><span data-stu-id="b2ed7-122">List virtual networks in a resource group</span></span>
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

<span data-ttu-id="b2ed7-123">您可以使用外部集合列出並檢查 `Network` 物件，或使用巢狀的 for-each 迴圈逐一查看每個網路的每個子資源，如此範例所示。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-123">You can list and inspect `Network` object using the outer collection or iterate through each child resource for each network using the nested for-each loop as seen in this example.</span></span>

## <a name="delete-a-virtual-network"></a><span data-ttu-id="b2ed7-124">刪除虛擬網路</span><span class="sxs-lookup"><span data-stu-id="b2ed7-124">Delete a virtual network</span></span>
```java
// if you already have the virtual network object it is easiest to delete by ID
azure.networks().deleteById(virtualNetwork1.id());

// Delete by resource group and name if you don't have the VirtualMachine object
azure.networks().deleteByResourceGroup(rgName,vnetName1);
```

<span data-ttu-id="b2ed7-125">移除虛擬網路將會刪除網路上的子網路，但不會刪除套用至子網路的網路安全性群組規則。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-125">Removing a virtual network deletes the subnets on the network but does not delete the network security group rules applied to the subnets.</span></span> <span data-ttu-id="b2ed7-126">這些規則定義可以重新套用至其他子網路。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-126">Those definitions can be reapplied to other subnets.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="b2ed7-127">範例的說明</span><span class="sxs-lookup"><span data-stu-id="b2ed7-127">Sample explanation</span></span>

<span data-ttu-id="b2ed7-128">此範例會建立具有兩個子網路、且每個子網路上各有一部虛擬機器的虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-128">This sample creates a virtual network with two subnets and with one virtual machine on each subnet.</span></span> <span data-ttu-id="b2ed7-129">後端子網路會與公用網際網路隔絕。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-129">The back subnet is cut off from the public Internet.</span></span> <span data-ttu-id="b2ed7-130">前端子網路則會接受來自網際網路的傳入 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-130">The front-facing subnet accepts inbound HTTP traffic from the Internet.</span></span> <span data-ttu-id="b2ed7-131">虛擬網路中的兩個虛擬機器會透過預設的網路安全性群組規則彼此通訊。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-131">Both virtual machines in the virtual network communicate with each other through the default network security group rules.</span></span>

| <span data-ttu-id="b2ed7-132">範例中使用的類別</span><span class="sxs-lookup"><span data-stu-id="b2ed7-132">Class used in sample</span></span> | <span data-ttu-id="b2ed7-133">注意事項</span><span class="sxs-lookup"><span data-stu-id="b2ed7-133">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="b2ed7-134">網路</span><span class="sxs-lookup"><span data-stu-id="b2ed7-134">Network</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | <span data-ttu-id="b2ed7-135">從 `azure.networks().define()...create()` 所建立之虛擬網路在本機所代表的物件。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-135">Local object representation of the virtual network created from `azure.networks().define()...create()` .</span></span> <span data-ttu-id="b2ed7-136">使用 `update()...apply()` Fluent 鏈結來更新現有的虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-136">Use the `update()...apply()` fluent chain to update an existing virtual network.</span></span>
| [<span data-ttu-id="b2ed7-137">子網路</span><span class="sxs-lookup"><span data-stu-id="b2ed7-137">Subnet</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._subnet) | <span data-ttu-id="b2ed7-138">在定義或更新網路時使用 `withSubnet()` 於虛擬網路上建立子網路。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-138">Create subnets on the virtual network when defining or updating the network using `withSubnet()`.</span></span> <span data-ttu-id="b2ed7-139">從 `Network.subnets().get()` 或 `Network.subnets().entrySet()` 取得代表子網路的物件。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-139">Get object representations of a subnet from `Network.subnets().get()` or `Network.subnets().entrySet()`.</span></span> <span data-ttu-id="b2ed7-140">這些物件會有方法可以查詢子網路屬性。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-140">These objects have methods to query subnet properties.</span></span>
| [<span data-ttu-id="b2ed7-141">NetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="b2ed7-141">NetworkSecurityGroup</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network_security_group) | <span data-ttu-id="b2ed7-142">使用 `azure.networkSecurityGroups().define()...create()` Fluent 鏈結所建立，然後透過在虛擬網路中更新或建立子網路以套用至子網路。</span><span class="sxs-lookup"><span data-stu-id="b2ed7-142">Created using the `azure.networkSecurityGroups().define()...create()` fluent chain and then applied to subnets through the updating or creating subnets in a virtual network.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b2ed7-143">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b2ed7-143">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]