---
title: "適用於 Java 的 Azure 虛擬機器程式庫"
description: 
keywords: "Azure, Java, SDK, API, 計算, 虛擬機器"
author: douge
ms.author: douge
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: compute
ms.openlocfilehash: b414f4dc9289958c2562ddc6d41133279f47a45e
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2017
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="f6c93-103">Azure 虛擬機器程式庫</span><span class="sxs-lookup"><span data-stu-id="f6c93-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="f6c93-104">概觀</span><span class="sxs-lookup"><span data-stu-id="f6c93-104">Overview</span></span>

<span data-ttu-id="f6c93-105">執行 Linux 或 Windows 並可視需要擴充的計算資源。</span><span class="sxs-lookup"><span data-stu-id="f6c93-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="f6c93-106">若要開始使用 Azure 虛擬機器，請參閱[使用 Azure 入口網站建立 Linux 虛擬機器](/azure/virtual-machines/linux/quick-create-portal)。</span><span class="sxs-lookup"><span data-stu-id="f6c93-106">To get started with Azure virtual machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="f6c93-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="f6c93-107">Management API</span></span>

<span data-ttu-id="f6c93-108">從程式碼使用管理 API 在 Azure 中建立、設定及相應放大 Windows 和 Linux 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="f6c93-108">Create, configure, and scale out Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="f6c93-109">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="f6c93-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-compute</artifactId>
    <version>1.2.1</version>
</dependency>
```   


## <a name="example"></a><span data-ttu-id="f6c93-110">範例</span><span class="sxs-lookup"><span data-stu-id="f6c93-110">Example</span></span>

<span data-ttu-id="f6c93-111">在新的 Azure 資源群組中建立新的 Linux 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="f6c93-111">Create a new Linux virtual machine in a new Azure resource group.</span></span>

```java
VirtualMachine newLinuxVm = azure.virtualMachines().define(linuxVmName)
            .withRegion(Region.US_EAST)
            .withNewResourceGroup("myResourceGroup")
            .withNewPrimaryNetwork("10.0.0.0/28")
            .withPrimaryPrivateIpAddressDynamic()
            .withoutPrimaryPublicIpAddress()
            .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
            .withRootUsername(userName)
            .withSshKey(key)
            .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
            .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="f6c93-112">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="f6c93-112">Explore the Management APIs</span></span>](/java/api/overview/azure/virtualmachines/managementapi)


## <a name="samples"></a><span data-ttu-id="f6c93-113">範例</span><span class="sxs-lookup"><span data-stu-id="f6c93-113">Samples</span></span>

<span data-ttu-id="f6c93-114">[管理虛擬機器][1] </span><span class="sxs-lookup"><span data-stu-id="f6c93-114">[Manage virtual machines][1] </span></span>  
<span data-ttu-id="f6c93-115">[管理虛擬網路][6] </span><span class="sxs-lookup"><span data-stu-id="f6c93-115">[Manage virtual networks][6] </span></span>  
<span data-ttu-id="f6c93-116">[從自訂映像建立虛擬機器][2] </span><span class="sxs-lookup"><span data-stu-id="f6c93-116">[Create a virtual machine from a custom image][2] </span></span>  
<span data-ttu-id="f6c93-117">[跨區域平行建立虛擬機器][5]  </span><span class="sxs-lookup"><span data-stu-id="f6c93-117">[Create virtual machines across regions in parallel][5]  </span></span>  
<span data-ttu-id="f6c93-118">[使用負載平衡器建立虛擬機器擴展集][7]</span><span class="sxs-lookup"><span data-stu-id="f6c93-118">[Create a virtual machine scale set with a load balancer][7]</span></span>    

[1]: ../docs-ref-conceptual/java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[5]: ../docs-ref-conceptual/java-sdk-virtual-machines-in-parallel.md
[6]: ../docs-ref-conceptual/java-sdk-manage-virtual-networks.md
[7]: ../docs-ref-conceptual/java-sdk-manage-vm-scalesets.md

<span data-ttu-id="f6c93-119">深入探索可在應用程式中使用的 [Azure 虛擬機器 Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=VM)。</span><span class="sxs-lookup"><span data-stu-id="f6c93-119">Explore more [sample Java code for Azure virtual machines](https://azure.microsoft.com/resources/samples/?platform=java&term=VM) you can use in your apps.</span></span>