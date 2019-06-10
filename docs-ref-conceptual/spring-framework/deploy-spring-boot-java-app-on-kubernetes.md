---
title: 在 Azure Kubernetes Service 中的 Kubernetes 上部署 Spring Boot 應用程式
description: 本教學課程會逐步引導您將 Spring Boot 應用程式部署為 Microsoft Azure 上之 Kubernetes 叢集的步驟。
services: container-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 9ab781d27e8968ab867efc65f3ac422ac6253a6a
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270851"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a>在 Azure Kubernetes Service 中的 Kubernetes 叢集上部署 Spring Boot 應用程式

**[Kubernetes]** 和 **[Docker]** 是開放原始碼解決方案，可協助開發人員自動化部署、調整及管理在容器中執行的應用程式。

本教學課程會逐步引導您結合這兩項受歡迎的開放原始碼技術，以開發 Spring Boot 應用程式並把它部署到 Microsoft Azure。 更具體來說，您使用 *[Spring Boot]* 開發應用程式、用 *[Kubernetes]* 部署容器，以及用 [Azure Kubernetes Service (AKS)] 裝載應用程式。

### <a name="prerequisites"></a>必要條件

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* [Azure 命令列介面 (CLI)]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven] 建置工具 (第 3 版)。
* [Git] 用戶端。
* [Docker] 用戶端。

> [!NOTE]
>
> 由於本教學課程的虛擬化需求，您無法遵循本文中關於虛擬機器的步驟；您必須在啟用虛擬化功能的情況下使用實體電腦。
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>建立 Spring Boot on Docker Getting Started Web 應用程式

下列步驟會引導您建置 Spring Boot Web 應用程式，並在本機加以測試。

1. 開啟命令提示字元並建立本機目錄來保存您的應用程式，然後變更至該目錄；例如：
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- 或 --
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. 將 [Spring Boot on Docker Getting Started] 範例專案複製到目錄。
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. 將目錄變更至已完成的專案。
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. 使用 Maven 來建置及執行範例應用程式。
   ```
   mvn package spring-boot:run
   ```

1. 瀏覽至 `http://localhost:8080` 來測試 Web 應用程式，或使用下列 `curl` 命令：
   ```
   curl http://localhost:8080
   ```

1. 您應該會看到下列訊息：**Hello Docker World**

   ![在本機瀏覽範例應用程式][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>使用 Azure CLI 建立 Azure Container Registry

1. 開啟命令提示字元。

1. 登入您的 Azure 帳戶：
   ```azurecli
   az login
   ```

1. 選擇您的 Azure 訂用帳戶：
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. 為此教學課程中使用的 Azure 資源建立資源群組。
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. 在資源群組中建立私用的 Azure Container Registry。 本教學課程會在稍後的步驟中，將範例應用程式推送為此登錄的 Docker 映像。 以登錄的唯一名稱取代 `wingtiptoysregistry`。
   ```azurecli
   az acr create --resource-group wingtiptoys-kubernetes --location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a>透過 Jib 將應用程式推送至容器登錄

1. 從 Azure CLI登入您的 Azure Container Registry。
   ```azurecli
   # set the default name for Azure Container Registry, otherwise you will need to specify the name in "az acr login"
   az configure --defaults acr=wingtiptoysregistry
   az acr login
   ```

1. 巡覽至 Spring Boot 應用程式已完成的專案目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 " */users/robert/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 *pom.xml* 檔案。

1. 用 Azure Container Registry 的登錄名稱和最新版本的 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) 更新 *pom.xml* 檔案中的 `<properties>` 集合。

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. 更新 *pom.xml* 檔案中的 `<plugins>` 集合，讓 `<plugin>` 中包含 `jib-maven-plugin`。

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
   </plugin>
   ```

1. 瀏覽至 Spring Boot 應用程式已完成的專案目錄，然後執行下列命令來建置映像，並將映像推送到登錄：

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> 基於 Azure Cli 和 Azure Container Registry 的安全考量，`az acr login` 所建立的認證具有 1 小時的有效期，如果發生 *401 未授權*錯誤，您可以再次執行 `az acr login -n <your registry name>` 命令以重新驗證。
>

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a>使用 Azure CLI 在 AKS 上建立 Kubernetes 叢集

1. 在 Azure Kubernetes Service 中建立 Kubernetes 叢集。 下列命令會在 *wingtiptoys-kubernetes* 資源群組中建立*kubernetes* 叢集，以 *wingtiptoys-akscluster* 為叢集名稱，*wingtiptoys-kubernetes* 為 DNS 前置詞：
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   此命令可能需要一些時間才能完成。

1. 當您使用 Azure Container Registry (ACR) 搭配 Azure Kubernetes Service (AKS) 時，需要將 Azure Container Registry 的提取存取權授與 Azure Kubernetes Service。 Azure 會在您建立 Azure Kubernetes Service 時，建立預設的服務主體。 在 Bash 或 Powershell 中執行下列指令碼，將 AKS 存取權授與 ACR；[從 Azure Kubernetes Service 對 Azure Container Registry 進行驗證]中另有詳細資訊供您參考。

```bash
   # Get the id of the service principal configured for AKS
   CLIENT_ID=$(az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv)
   
   # Get the ACR registry resource id
   ACR_ID=$(az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv)
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

  -- 或 --

```PowerShell
   # Get the id of the service principal configured for AKS
   $CLIENT_ID = az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv
   
   # Get the ACR registry resource id
   $ACR_ID = az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

1. 使用 Azure CLI 安裝 `kubectl`。 Linux 使用者在此命令前可能要加上 `sudo`，因為它會將 Kubernetes CLI 部署到 `/usr/local/bin`。
   ```azurecli
   az aks install-cli
   ```

1. 下載叢集設定資訊，以便從 Kubernetes Web 介面和 `kubectl` 管理您的叢集。 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>部署 Kubernetes 叢集的映像

本教學課程使用 `kubectl` 部署應用程式，然後讓您透過 Kubernetes web 介面瀏覽部署。

### <a name="deploy-with-kubectl"></a>使用 kubectl 部署

1. 開啟命令提示字元。

1. 使用 `kubectl run` 命令在 Kubernetes 叢集中執行容器。 為您在 Kubernetes 中的應用程式提供服務名稱和完整的映像名稱。 例如︰
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   在這個命令中：

   * 容器名稱 `gs-spring-boot-docker` 直接指定在 `run` 命令後面

   * `--image` 參數指定合併的登入伺服器和映像名稱為 `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. 使用 `kubectl expose` 命令向外部公開您的 Kubernetes 叢集。 指定您的服務名稱、用來存取應用程式的公開 TCP 通訊埠，以及應用程式接聽的內部目標連接埠。 例如︰
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   在這個命令中：

   * 容器名稱 `gs-spring-boot-docker` 直接指定在 `expose deployment` 命令後面

   * `--type` 參數指定叢集使用負載平衡器

   * `--port` 參數指定公開的 TCP 通訊埠 80。 您會在此連接埠存取應用程式。

   * `--target-port` 參數指定內部 TCP 通訊埠 8080。 負載平衡器會在此連接埠上將要求轉送到您的應用程式。

1. 一旦應用程式部署至叢集，請查詢外部 IP 位址，並在網頁瀏覽器中開啟它：

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=default
   ```

   ![在 Azure 上瀏覽範例應用程式][SB02]



### <a name="deploy-with-the-kubernetes-web-interface"></a>使用 Kubernetes Web 介面部署

1. 開啟命令提示字元。

1. 在預設瀏覽器中開啟 Kubernetes 叢集的設定網站：
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. 當 Kubernetes 設定網站在您的瀏覽器中開啟時，請按一下連結以**部署容器化應用程式**：

   ![Kubernetes 設定網站][KB01]

1. 當 [資源建立]  頁面出現時，請指定下列選項：

   a. 選取 [建立應用程式]  。

   b. 在 [應用程式名稱]  中輸入您的 Spring Boot 應用程式名稱，例如："*gs-spring-boot-docker*"。

   c. 在 [容器映像]  中輸入先前的登入伺服器和容器映像，例如："*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"。

   d. 針對 [服務]  選擇 [外部]  。

   e. 在 [連接埠]  和 [目標連接埠]  文字方塊中指定您的外部和內部連接埠。

   ![Kubernetes 設定網站][KB02]


1. 按一下 [部署]  來部署容器。

   ![Kubernetes 部署][KB05]

1. 應用程式一經部署，您就會看到 [服務]  底下列出您的 Spring Boot 應用程式。

   ![Kubernetes 服務][KB06]

1. 如果按一下**外部端點**連結，您會看到您的 Spring Boot 應用程式在 Azure 上執行。

   ![Kubernetes 服務][KB07]

   ![在 Azure 上瀏覽範例應用程式][SB02]

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他資源

如需進一步了解在 Azure 上使用 Spring Boot 的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)

如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。

如需使用 Visual Studio Code 將 Java 應用程式部署至 Kubernetes 的詳細資訊，請參閱 [Visual Studio Code Java 教學課程]。

如需 Spring Boot on Docker 範例專案的詳細資訊，請參閱 [Spring Boot on Docker Getting Started]。

下列連結提供建立 Spring Boot 應用程式的其他資訊：

* 如需建立簡易 Spring Boot 應用程式的詳細資訊，請參閱 Spring Initializr，網址為 https://start.spring.io/。

下列連結提供以 Azure 使用 Kubernetes 的其他資訊：

* [在 Azure Kubernetes Service 中開始使用 Kubernetes 叢集](/azure/aks/intro-kubernetes)

如需使用 Kubernetes 命令列介面的詳細資訊，請參閱 **kubectl** 使用者指南，網址為 <https://kubernetes.io/docs/user-guide/kubectl/> 。

Kubernetes 網站有幾篇文章討論在私用登錄中使用映像：

* [設定 Pod 的服務帳戶]
* [命名空間]
* [從私用登錄提取映像]

如需如何搭配 Azure 使用自訂 Docker 映像的其他範例，請參閱[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]。

如需直接在 Azure Kubernetes Service (AKS) 中搭配 Azure Dev Spaces 反覆執行和偵錯容器的詳細資訊，請參閱[開始透過 Java 使用 Azure Dev Spaces]

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Kubernetes Service (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[適用於 Java 開發人員的 Azure]: /java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[使用 Azure DevOps 和 Java]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[設定 Pod 的服務帳戶]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[命名空間]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[從私用登錄提取映像]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- Newly added -->
[從 Azure Kubernetes Service 對 Azure Container Registry 進行驗證]: /azure/container-registry/container-registry-auth-aks/
[Visual Studio Code Java 教學課程]: https://code.visualstudio.com/docs/java/java-kubernetes/
[開始透過 Java 使用 Azure Dev Spaces]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB07.png
