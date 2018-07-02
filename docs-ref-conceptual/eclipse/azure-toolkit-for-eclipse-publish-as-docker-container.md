---
title: 使用適用於 Eclipse 的 Azure 工具組來發佈 Docker 容器
description: 了解如何使用適用於 Eclipse 的 Azure 工具組，將 Web 應用程式發佈至 Microsoft Azure 作為 Docker 容器。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: be76733bffa36160d6e366c383672a15374a9996
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090841"
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a>使用 Azure Toolkit for Eclipse 將 Web 應用程式發佈作為 Docker 容器

Docker 容器是常用的 Web 應用程式部署方法。 藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。 Azure Toolkit for Eclipse 可為 Java 開發人員簡化此部署程序，其憑借的方法是為 Microsoft Azure 的部署新增「發佈作為 Docker 容器」功能。 本文會引導您完成所需步驟，以便將應用程式發佈至 Azure 來作為 Docker 容器。

> [!NOTE]
> Docker 的詳細資訊位於 [Docker 的官方網站]。
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>使用 Docker 容器將您的 Web 應用程式發佈至 Azure

1. 在 Eclipse 中開啟 web 應用程式專案。

2. 若要啟動**發佈作為 Docker 容器**精靈，請執行下列其中一個動作：

   * 在 [導覽器] 檢視中，以滑鼠右鍵按一下您的專案，然後依序按一下 [Azure] 和 [發佈作為 Docker 容器]：

      ![導覽器檢視 [發佈作為 Docker 容器] 命令][PUB01]

   * 在 Eclipse 工具列中按一下 [發佈] 按鈕，然後按一下 [發佈作為 Docker 容器]。

      ![Eclipse 工具列 [發佈作為 Docker 容器] 命令][PUB02]
      
   [在 Azure 上部署 Docker 容器] 精靈隨即開啟。

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB03]

3. 在 [輸入映像名稱、選取構件路徑並確認要使用的 Docker 主機] 視窗中，執行下列動作︰

   a. 在 [Docker 映像名稱] 方塊中，輸入 Docker 主機的唯一名稱  (精靈會自動建立一個名稱，但您可以修改)。

   b. [主機] 區域會顯示您已建立的任何 Docker 主機。 執行下列其中一個動作：

   * 如果您有現存的 Docker 主機，您可以對該主機部署 Web 應用程式。
   * 若要建立新的 Docker 主機，請按一下 [新增]。  
      
      [建立 Docker 主機] 對話方塊會隨即開啟。

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB04a]

4. 在 [設定新的虛擬機器] 視窗中，對您的 Docker 主機指定下列資訊。 (精靈會自動為您產生大部分的選項，但您可以對這些資訊進行修改)。

   a. **名稱**︰輸入 Docker 主機的唯一名稱。 (此名稱與您稍早指定的 Docker 映像名稱不同)。

   b. **訂用帳戶**：輸入您用於主機的 Azure 訂用帳戶。

   c. **區域**：輸入主機所在的地理區域。

   d. 在 [主機作業系統和大小] 索引標籤上︰ 
   * **主機作業系統**︰輸入主機所在虛擬機器的作業系統。
   * **大小**：輸入主機的虛擬機器大小。

   e. 在 [資源群組] 索引標籤上： 
   * **新的資源群組**︰為主機建立新的資源群組。
   * **現有的資源群組**︰輸入 Azure 帳戶中的現有資源群組。

   f. 在 [網路] 索引標籤上︰ 
   * **新的虛擬網路**︰為主機建立新的虛擬網路。
   * **現有的虛擬網路**︰輸入 Azure 帳戶中的現有虛擬網路。

   g. 在 [儲存體] 索引標籤上： 
   * **新的儲存體帳戶**︰為主機建立新的儲存體帳戶。
   * **現有的儲存體帳戶**︰輸入 Azure 帳戶中的現有儲存體帳戶。

5. 按 [下一步] 。

6. 在 [設定登入認證和連接埠設定] 視窗中，選取下列其中一個選項：

   * **從 Azure Key Vault 匯入認證**︰指定先前儲存在 Azure 訂用帳戶中的一組認證。 

   >[!NOTE]
   >使用特定帳戶或服務主體所建立的 Azure Key Vault，無法供另一個共用該訂用帳戶的帳戶或服務主體自動存取。 若要讓另一個帳戶或服務主體使用 Key Vault，您必須使用 Azure 入口網站來新增該帳戶或服務主體。
   >

   * **新的登入認證**︰建立一組新的登入認證。 如果您選取此選項，請執行下列作業︰ 
    
     * 針對 Docker 主機的虛擬機器登入認證，請在 [VM 認證] 索引標籤上選擇下列其中一個選項： 

       * **使用者名稱**︰輸入虛擬機器登入認證的使用者名稱。 
       * **密碼**和**確認**︰輸入虛擬機器登入認證的密碼。 
       * **SSH**︰輸入 Docker 主機的安全殼層 (SSH) 設定。 您可選擇下列選項： 
          * **無**︰指定虛擬機器將不允許 SSH 連線。 
          * **自動產生**︰自動建立透過 SSH 連線的必要設定。 
          * **從目錄匯入**︰指定內含一組先前儲存之 SSH 設定的目錄。 此目錄必須包含下列兩個檔案︰ 
             * id_rsa︰包含使用者的 RSA 識別。 
             * id_rsa.pub︰包含用於驗證的 RSA 公開金鑰。 
        
         ![建立 Docker 主機][PUB05]

     * 在 [Docker 精靈認證] 索引標籤上，指定下列選項︰ 

       * **Docker 精靈連接埠**︰輸入 Docker 主機的唯一 TCP 通訊埠。 
       * **TLS 安全性**︰輸入 Docker 主機的傳輸層安全性設定。 您可選擇下列選項： 
          * **無**︰指定虛擬機器將不允許 TLS 連線。 
          * **自動產生**︰自動建立透過 TLS 連線的必要設定。 
          * **從目錄匯入**︰指定內含一組先前儲存之 TLS 設定的目錄。 更具體來說，目錄必須包含下列六個檔案︰ 
             * ca.pem 和 ca key.pem︰包含 TLS 憑證授權單位的憑證和公開金鑰。 
             * cert.pem 和 key.pem︰包含用於 TLS 驗證的用戶端憑證和公開金鑰。 
             * server.pem 和 server-key.pem︰包含主機的伺服器憑證和公開金鑰。 

         ![建立 Docker 主機][PUB06]

7. 輸入以上所有資訊之後，按一下 [完成]。

8. 在 [在 Azure 上部署 Docker 容器] 精靈中，按 [下一步]。

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB07]

9. 在 [設定要建立的 Docker 容器] 視窗中，執行下列動作：

   a. 在 [Docker 容器名稱] 方塊中，輸入 Docker 容器的唯一名稱。

   b. 選擇下列其中一個 Docker 映像： 

   * **預先定義的 Docker 映像**︰指定 Azure 中的既存 Docker 映像。 

     >[!NOTE]
     >此方塊中的 Docker 映像清單包含數個映像，而我們已將 Azure 工具組設定為要修補這些映像，因此系統會自動部署您的構件。
     >

   * **自訂 Dockerfile**︰指定本機電腦中先前儲存的 Dockerfile。

     >[!NOTE]
     >這個更進階的功能適用於想要部署專屬 Dockerfile 的開發人員。 不過，這取決於使用此選項來確保已正確建置其 Dockerfile 的開發人員。 Azure 工具組並不會驗證自訂 Dockerfile 中的內容，因此如果 Dockerfile 有問題，則部署可能會失敗。 此外，Azure 工具組預期自訂的 Dockerfile 會包含 Web 應用程式構件，而且會嘗試開啟 HTTP 連線。 如果開發人員發佈不同類型的構件，他們可能會在部署後收到無關緊要的錯誤。
     >

   c. **連接埠設定**：輸入 Docker 容器的唯一 TCP 通訊埠繫結。

      ![[設定要建立的 Docker 容器] 視窗][PUB08]

10. 完成上述所有步驟之後，按一下 [完成]。

Azure 工具組會開始將您的 Web 應用程式部署至 Azure 中的 Docker 容器。 

## <a name="next-steps"></a>後續步驟

如需 Docker 的其他資源，請參閱 [Docker 的官方網站]。

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Docker 的官方網站]: https://www.docker.com/

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png