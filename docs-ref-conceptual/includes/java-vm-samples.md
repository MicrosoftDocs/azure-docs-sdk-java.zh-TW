---
ms.openlocfilehash: ab31ee32ea940db2d7bcfa2fe36475d8a648bfc9
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592606"
---
| **建立虛擬機器** || 
|---|---|
| [管理虛擬機器][1] | 建立、修改、啟動、停止和刪除虛擬機器。 |
| [從自訂映像建立虛擬機器][2] | 建立自訂虛擬機器映像，並用它來建立新的虛擬機器。 | 
| [使用從快照集取得的特製化 VHD 建立虛擬機器][3] | 從虛擬機器的 OS 和資料磁碟建立快照集、從快照集建立受控磁碟，然後連結受控磁碟來建立虛擬機器。 |  
| [在相同網路中平行建立虛擬機器][4] | 在相同區域中具有兩個子網路的相同虛擬網路上平行建立虛擬機器。 |
| [跨區域平行建立虛擬機器][5] | 跨多個 Azure 區域建立一組虛擬機器並對它們進行負載平衡。 |
| **網路虛擬機器** || 
| [管理虛擬網路][6] | 設定具有兩個子網路的虛擬網路，並限制網際網路對這兩個子網路的存取。 |
| **建立擴展集** ||
| [使用負載平衡器建立虛擬機器擴展集][7] | 建立 VM 擴展集、設定負載平衡器，並取得擴展集 VM 的 SSH 連接字串。 |

[1]: ../java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[3]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-specialized-disk-from-vhd/
[4]: https://azure.microsoft.com/resources/samples/compute-java-manage-virtual-machines-in-parallel/
[5]: ../java-sdk-virtual-machines-in-parallel.md
[6]: ../java-sdk-manage-virtual-networks.md
[7]: ../java-sdk-manage-vm-scalesets.md