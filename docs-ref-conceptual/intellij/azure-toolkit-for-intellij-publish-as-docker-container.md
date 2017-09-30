---
title: "使用適用於 IntelliJ 的 Azure 工具組來發佈 Docker 容器"
description: "了解如何使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈至 Microsoft Azure 作為 Docker 容器。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 2d94dfa4467bc79d6155321a9966358b79e9e960
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a>使用適用於 IntelliJ 的 Azure 工具組，將 Web 應用程式發佈作為 Docker 容器

Docker 容器是常見的 Web 應用程式部署方法。 藉由使用 Docker 容器，開發人員可將所有的專案檔和相依性合併至單一套件，以供部署至伺服器。 適用於 IntelliJ 的 Azure 工具組可為 Java 開發人員簡化此部署程序，其憑借的方法是為 Microsoft Azure 的部署新增「發佈作為 Docker 容器」功能。 本文會引導您完成所需步驟，以便將應用程式發佈至 Azure 來作為 Docker 容器。

> [!NOTE]
>
> Docker 的詳細資訊位於 [Docker 的官方網站]。
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>使用 Docker 容器將您的 Web 應用程式發佈至 Azure

> [!NOTE]
> * 若要發佈 Web 應用程式，您必須建立已可供部署的構件。 若要深入了解，請參閱[建立構件的其他資訊](#artifacts)一節。
>
> * 至少完成部署精靈一次之後，其中的大部分設定便會在您再次執行精靈時作為預設值。
>

1. 在 IntelliJ 中開啟 web 應用程式專案。

2. 若要啟動**發佈作為 Docker 容器**精靈，請執行下列其中一個動作：

   * 在 [專案] 工具視窗中，以滑鼠右鍵按一下您的專案，然後依序按一下 [Azure] 和 [發佈作為 Docker 容器]：

      ![[發佈作為 Docker 容器] 命令][PUB01]

   * 在 IntelliJ 工具列中按一下 [發佈群組] 按鈕，然後按一下 [發佈作為 Docker 容器]：

      ![[發佈作為 Docker 容器] 命令][PUB02]  
    [在 Azure 上部署 Docker 容器] 精靈隨即開啟。

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB03]

3. 在 [輸入映像名稱、選取構件路徑並確認要使用的 Docker 主機] 視窗中，執行下列動作︰ 

   a. 在 [Docker 映像名稱] 方塊中，輸入 Docker 主機的唯一名稱  (精靈會自動建立一個名稱，但您可以修改)。 

   b.這是另一個 C# 主控台應用程式。 [主機] 區域會顯示您已建立的任何 Docker 主機。 執行下列其中一個動作： 
      * 如果您有現存的 Docker 主機，您可以對該主機部署 Web 應用程式。
      * 若要建立 Docker 主機，則請按一下綠色加號 (**+**)。  
       [建立 Docker 主機] 對話方塊會隨即開啟。 

      ![[在 Azure 上部署 Docker 容器] 精靈][PUB04a]

4. 在 [設定新的虛擬機器] 視窗中，提供下列有關 Docker 主機的資訊  (精靈會自動為您產生大部分的資訊，但您可以對這些資訊進行修改)。 

   a. 在 [名稱] 方塊中，輸入 Docker 主機的唯一名稱  (此名稱與您稍早指定的 Docker 映像名稱不同)。 
    
   b.這是另一個 C# 主控台應用程式。 在 [訂用帳戶] 方塊中，輸入您為主機使用的 Azure 訂用帳戶。 
      
   c. 在 [區域] 方塊中，輸入主機所在的地理區域。
      
   d. 在 [作業系統和大小] 索引標籤上，執行下列動作︰      
      * **主機作業系統**︰輸入主機所在虛擬機器的作業系統。 
      * **大小**：輸入主機的虛擬機器大小。   
       
   e. 在 [資源群組] 索引標籤上，選取下列其中一項︰      
      * **新的資源群組**︰為主機建立資源群組。
      * **現有的資源群組**︰指定 Azure 帳戶中的現有資源群組。 
       
   f. 在 [網路] 索引標籤上，選取下列其中一項︰      
      * **新的虛擬網路**︰為主機建立虛擬網路。
      * **現有的虛擬網路**︰指定 Azure 帳戶中的現有虛擬網路。 
       
   g. 在 [儲存體] 索引標籤上，選取下列其中一項︰      
      * **新的儲存體帳戶**︰為主機建立儲存體帳戶。
      * **現有的儲存體帳戶**︰指定 Azure 帳戶中的現有儲存體帳戶。
       
5. 按一下 [下一步] 。  
     [設定登入認證和連接埠設定] 視窗隨即開啟。

      ![[設定登入認證和連接埠設定] 視窗][PUB05]

6. 選取下列其中一個選項：

      * **從 Azure Key Vault 匯入認證**︰指定先前儲存的一組認證，這些認證儲存在 Azure 訂用帳戶中。

          > [!NOTE]
          > 使用特定帳戶或服務主體所建立的 Azure Key Vault，無法供另一個共用該訂用帳戶的帳戶或服務主體自動存取。 若要讓另一個帳戶或服務主體使用 Key Vault，您必須使用 Azure 入口網站來新增該帳戶或服務主體。

      * **新的登入認證**︰建立一組新的登入認證。 如果您選取此選項，請執行下列作業︰

        a. 在 **VM 認證** 索引標籤上提供下列資訊，以供 Docker 主機的虛擬機器登入認證使用︰ * **使用者名稱**︰ 輸入虛擬機器登入認證的使用者名稱。
             * **密碼**和**確認**︰輸入虛擬機器登入認證的密碼。
             * **SSH**︰輸入 Docker 主機的安全殼層 (SSH) 設定。 您可以選取下列其中一個選項： * **無**︰ 將虛擬機器指定為不允許 SSH 連線。
                * **自動產生**︰自動建立透過 SSH 連線的必要設定。
                * **從目錄匯入**︰可讓您指定內含一組先前儲存之 SSH 設定的目錄。 此目錄必須包含下列兩個檔案︰
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        b.這是另一個 C# 主控台應用程式。 在 [Docker 精靈存取] 索引標籤上，提供下列資訊︰

          ![建立 Docker 主機][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. 在輸入必要資訊之後，按一下 [完成]。  
    [在 Azure 上部署 Docker 容器] 精靈隨即重新開啟。

   ![[在 Azure 上部署 Docker 容器] 精靈][PUB07]

8. 按一下 [下一步] 。  
    [設定要建立的 Docker 容器] 視窗隨即開啟。

   ![[設定要建立的 Docker 容器] 視窗][PUB08]

9. 在 [設定要建立的 Docker 容器] 視窗中，提供下列資訊︰ 

   a. 在 [Docker 容器名稱] 方塊中，輸入 Docker 容器的唯一名稱。

   b.這是另一個 C# 主控台應用程式。 選擇下列其中一個 Docker 映像： 

      * **預先定義的 Docker 映像**︰指定 Azure 中的既存 Docker 映像。 

        > [!NOTE]
        > 此方塊中的 Docker 映像清單包含數個映像，而我們已將 Azure 工具組設定為要修補這些映像，因此系統會自動部署您的構件。 

      * **自訂 Dockerfile**︰指定本機電腦中先前儲存的 Dockerfile。

        > [!NOTE]
        > 這個更進階的功能適用於想要部署專屬 Dockerfile 的開發人員。 不過，這取決於使用此選項來確保已正確建置其 Dockerfile 的開發人員。 Azure 工具組並不會驗證自訂 Dockerfile 中的內容，因此如果 Dockerfile 有問題，則部署可能會失敗。 此外，由於 Azure 工具組預期自訂的 Dockerfile 會包含 Web 應用程式構件，因此會嘗試開啟 HTTP 連線。 如果開發人員發佈不同類型的構件，他們可能會在部署後收到無關緊要的錯誤。

   c. 在 [通訊埠設定] 方塊中，輸入 Docker 容器的唯一 TCP 通訊埠繫結。 

10. 完成上述步驟之後，按一下 [完成]。 

Azure 工具組會開始將您的 Web 應用程式部署至 Azure 中的 Docker 容器。 除非您已將 IntelliJ 設定為在背景中部署，否則畫面上會出現 [部署至 Azure] 進度列。 

![部署進度列][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a>建立構件的其他資訊

若要建立已可供部署的構件，請執行下列步驟︰

1. 在 IntelliJ 中開啟 web 應用程式專案。

2. 依序按一下 [檔案] 及 [專案結構]。

   ![[專案結構] 命令][ART01]

3. 若要新增構件，請按一下綠色加號 (**+**)，然後按一下 [Web 應用程式: 封存]。

   ![[Web 應用程式: 封存] 命令][ART02]

4. 在 [名稱] 方塊中，輸入構件的名稱 (請勿加上「.war」副檔名)，然後按一下 [確定]。

   ![構件的 [名稱] 方塊][ART03]

如需在 IntelliJ 中建立構件的詳細資訊，請參閱 JetBrains 網站上的[設定構件]。

## <a name="next-steps"></a>後續步驟

如需 Docker 的其他資源，請參閱 [Docker 的官方網站]。

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Docker 的官方網站]: https://www.docker.com/
[設定構件]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
