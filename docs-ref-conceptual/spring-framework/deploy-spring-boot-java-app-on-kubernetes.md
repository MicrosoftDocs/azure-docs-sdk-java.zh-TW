---
title: "在 Azure Container Service 的 Kubernetes 上部署 Spring Boot 應用程式"
description: "本教學課程會逐步引導您將 Spring Boot 應用程式部署為 Microsoft Azure 上之 Kubernetes 叢集的步驟。"
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: ee8d5fecc31df427645c1552e27996592eaf27af
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a>將 Spring Boot 應用程式部署到 Azure Container Service 中的 Kubernetes 叢集

**[Kubernetes]** 和 **[Docker]** 是開放原始碼解決方案，可協助開發人員自動化部署、調整及管理在容器中執行的應用程式。

本教學課程會逐步引導您結合這兩項受歡迎的開放原始碼技術，以開發 Spring Boot 應用程式並把它部署到 Microsoft Azure。 更具體來說，您使用 *[Spring Boot]* 開發應用程式、用 *[Kubernetes]* 部署容器，以及用 [Azure Container Service (AKS)] 裝載應用程式。

### <a name="prerequisites"></a>必要條件

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* [Azure 命令列介面 (CLI)]。
* 最新的 [Java 開發工具組 (JDK)]。
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

1. 瀏覽至 http://localhost:8080 來測試 Web 應用程式，或使用下列 `curl` 命令：
   ```
   curl http://localhost:8080
   ```

1. 您應該會看到顯示下列訊息：**Hello Docker World**

   ![在本機瀏覽範例應用程式][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>使用 Azure CLI 建立 Azure Container Registry

1. 開啟命令提示字元。

1. 登入您的 Azure 帳戶：
   ```azurecli
   az login
   ```

1. 為此教學課程中使用的 Azure 資源建立資源群組。
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. 在資源群組中建立私用的 Azure Container Registry。 本教學課程會在稍後的步驟中，將範例應用程式推送為此登錄的 Docker 映像。 以登錄的唯一名稱取代 `wingtiptoysregistry`。
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a>將應用程式推送至容器登錄

1. 巡覽至您 Maven 安裝的設定目錄 (預設為 ~/.m2/ 或 C:\Users\使用者名稱\.m2)，並使用文字編輯器開啟 *settings.xml* 檔案。

1. 從 Azure CLI 擷取容器登錄的密碼。
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. 將您的 Azure Container Registry 識別碼和密碼新增至 *settings.xml* 檔案的新 `<server>` 集合中。
`id` 和 `username` 是登錄的名稱。 使用上一個命令的 `password` 值 (不含引號)。

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. 巡覽至 Spring Boot 應用程式已完成的專案目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 *pom.xml* 檔案。

1. 使用 Azure Container Registry 的登入伺服器值來更新 *pom.xml* 檔案中的 `<properties>` 集合。

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. 更新 *pom.xml* 檔案中的 `<plugins>` 集合，以便 `<plugin>` 包含 Azure Container Registry 的登入伺服器位址和登錄名稱。

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. 巡覽至 Spring Boot 應用程式已完成的專案目錄，然後執行下列命令來建置 Docker 容器，並將映像推送到登錄：

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  當 Maven 將映像推送至 Azure 時，您可能會收到與下列其中之一相似的錯誤訊息：
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> 如果發生此錯誤，請從 Docker 命令列登入 Azure。
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> 然後推送您的容器：
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a>使用 Azure CLI 在 AKS 上建立 Kubernetes 叢集

1. 在 Azure Container Service 中建立 Kubernetes 叢集。 下列命令會在 *wingtiptoys-kubernetes* 資源群組中建立*kubernetes* 叢集，以 *wingtiptoys-containerservice* 為叢集名稱，*wingtiptoys-kubernetes* 為 DNS 前置詞：
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   此命令可能需要一些時間才能完成。

1. 使用 Azure CLI 安裝 `kubectl`。 Linux 使用者在此命令前可能要加上 `sudo`，因為它會將 Kubernetes CLI 部署到 `/usr/local/bin`。
   ```azurecli
   az acs kubernetes install-cli
   ```

1. 下載叢集設定資訊，以便從 Kubernetes Web 介面和 `kubectl` 管理您的叢集。 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>部署 Kubernetes 叢集的映像

本教學課程使用 `kubectl` 部署應用程式，然後讓您透過 Kubernetes web 介面瀏覽部署。

### <a name="deploy-with-the-kubernetes-web-interface"></a>使用 Kubernetes Web 介面部署

1. 開啟命令提示字元。

1. 在預設瀏覽器中開啟 Kubernetes 叢集的設定網站：
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. 當 Kubernetes 設定網站在您的瀏覽器中開啟時，請按一下連結以**部署容器化應用程式**：

   ![Kubernetes 設定網站][KB01]

1. 當 [Deploy a containerized app] (部署容器化應用程式) 頁面出現時，請指定下列選項：

   a. 選取 [Specify app details below] (指定以下的應用程式詳細資料)。

   b.這是另一個 C# 主控台應用程式。 在 [應用程式名稱] 中輸入您的 Spring Boot 應用程式名稱，例如："*gs-spring-boot-docker*"。

   c. 在 [容器映像] 中輸入先前的登入伺服器和容器映像，例如："*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"。

   d. 針對 [服務] 選擇 [外部]。

   e. 在 [連接埠] 和 [目標連接埠] 文字方塊中指定您的外部和內部連接埠。

   ![Kubernetes 設定網站][KB02]


1. 按一下 [部署] 來部署容器。

   ![部署容器][KB05]

1. 應用程式一經部署，您就會看到 [服務] 底下列出您的 Spring Boot 應用程式。

   ![Kubernetes 服務][KB06]

1. 如果按一下**外部端點**連結，您會看到您的 Spring Boot 應用程式在 Azure 上執行。

   ![Kubernetes 服務][KB07]

   ![在 Azure 上瀏覽範例應用程式][SB02]


### <a name="deploy-with-kubectl"></a>使用 kubectl 部署

1. 開啟命令提示字元。

1. 使用 `kubectl run` 命令在 Kubernetes 叢集中執行容器。 為您在 Kubernetes 中的應用程式提供服務名稱和完整的映像名稱。 例如：
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   在這個命令中：

   * 容器名稱 `gs-spring-boot-docker` 直接指定在 `run` 命令後面

   * `--image` 參數指定合併的登入伺服器和映像名稱為 `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. 使用 `kubectl expose` 命令向外部公開您的 Kubernetes 叢集。 指定您的服務名稱、用來存取應用程式的公開 TCP 通訊埠，以及應用程式接聽的內部目標連接埠。 例如：
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
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![在 Azure 上瀏覽範例應用程式][SB02]


## <a name="next-steps"></a>後續步驟

如需在 Azure 上使用 Spring Boot 的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)
* [將 Spring Boot 應用程式部署到 Azure Container Service 中的 Linux](deploy-spring-boot-java-app-on-linux.md)

如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。

如需 Spring Boot on Docker 範例專案的詳細資訊，請參閱 [Spring Boot on Docker Getting Started]。

下列連結提供建立 Spring Boot 應用程式的其他資訊：

* 如需建立簡易 Spring Boot 應用程式的詳細資訊，請參閱 Spring Initializr (網址為 https://start.spring.io/)。

下列連結提供以 Azure 使用 Kubernetes 的其他資訊：

* [在 Container Service 中開始使用 Kubernetes 叢集](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [使用 Kubernetes Web UI 和 Azure Container Service ](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

使用 Kubernetes 命令列介面的詳細資訊，請參閱 **kubectl** 使用者指南，網址為 <https://kubernetes.io/docs/user-guide/kubectl/>。

Kubernetes 網站有幾篇文章討論在私用登錄中使用映像：

* [設定 Pod 的服務帳戶]
* [命名空間]
* [從私用登錄提取映像]

如需如何以 Azure 使用自訂 Docker 映像的其他範例，請參閱[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]。

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java 開發工具組 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
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
