---
title: 使用 Fabric8 Maven 外掛程式部署 Spring Boot 應用程式
description: 本教學課程將會逐步引導您使用適用於 Apache Maven 的 Fabric8 外掛程式在 Microsoft Azure 上部署 Spring Boot 應用程式。
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: yuwzho;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: f05dca50f84b27f157892d63cda02286c6755795
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090811"
---
# <a name="deploy-a-spring-boot-app-using-the-fabric8-maven-plugin"></a>使用 Fabric8 Maven 外掛程式部署 Spring Boot 應用程式

**[Fabric8]** 是一個以 **[Kubernetes]** 為基礎建置的開放原始碼解決方案，它可協助開發人員在 Linux 容器中建立應用程式。

本教學課程會逐步引導您使用適用於 Maven 的 Fabric8 外掛程式來開發應用程式以部署至 [Azure Container Service (AKS)] 中的 Linux 主機。

## <a name="prerequisites"></a>先決條件

若要完成本教學課程中的步驟，您必須具備下列必要條件：

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
   ```shell
   md /home/GenaSoto/SpringBoot
   cd /home/GenaSoto/SpringBoot
   ```
   -- 或 --
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```

1. 將 [Spring Boot on Docker Getting Started] 範例專案複製到目錄。
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. 將目錄變更至已完成的專案；例如：
   ```shell
   cd gs-spring-boot-docker/complete
   ```
   -- 或 --
   ```shell
   cd gs-spring-boot-docker\complete
   ```

1. 使用 Maven 來建置及執行範例應用程式。
   ```shell
   mvn clean package spring-boot:run
   ```

1. 瀏覽至 http://localhost:8080 來測試 Web 應用程式，或使用下列 `curl` 命令：
   ```shell
   curl http://localhost:8080
   ```

   您應該會看到「Hello Docker World」的訊息。

   ![於本機瀏覽範例應用程式][SB01]


## <a name="install-the-kubernetes-command-line-interface-and-create-an-azure-resource-group-using-the-azure-cli"></a>安裝 Kubernetes 命令列介面，並使用 Azure CLI 建立 Azure 資源群組

1. 開啟命令提示字元。

1. 輸入下列命令來登入您的 Azure 帳戶：
   ```azurecli
   az login
   ```
   依照指示完成登入程序

   Azure CLI 將會顯示您的帳戶清單，例如：

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "00000000-0000-0000-0000-000000000000",
       "isDefault": false,
       "name": "Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "00000000-0000-0000-0000-000000000000",
       "user": {
         "name": "Gena.Soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. 如果您尚未安裝 Kubernetes 命令列介面 (`kubectl`)，您可以使用 Azure CLI 來安裝，例如：
   ```azurecli
   az acs kubernetes install-cli
   ```

   > [!NOTE]
   >
   > Linux 使用者在此命令前可能要加上 `sudo`，因為它會將 Kubernetes CLI 部署到 `/usr/local/bin`。
   >
   > 如果您已經安裝了 `kubectl`，但您的 `kubectl` 版本太舊，當您嘗試完成本文章稍後所列出的步驟時，可能會看到類似下列範例的錯誤訊息：
   >
   > ```
   > error: group map[autoscaling:0x0000000000 batch:0x0000000000 certificates.k8s.io
   > :0x0000000000 extensions:0x0000000000 storage.k8s.io:0x0000000000 apps:0x0000000
   > 000 authentication.k8s.io:0x0000000000 policy:0x0000000000 rbac.authorization.k8
   > s.io:0x0000000000 federation:0x0000000000 authorization.k8s.io:0x0000000000 comp
   > onentconfig:0x0000000000] is already registered
   > ```
   >
   > 如果發生這種情況，您將需要重新安裝 `kubectl` 以更新您的版本。
   >

1. 為您將在本教學課程中使用的 Azure 資源建立資源群組，例如：
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=westeurope
   ```
   其中：  
      * *wingtiptoys-kubernetes* 是您資源群組的唯一名稱  
      * *westeurope* 是您應用程式的適當地理位置  

   Azure CLI 將會顯示建立資源群組的結果，例如：  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes",
     "location": "westeurope",
     "managedBy": null,
     "name": "wingtiptoys-kubernetes",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```


## <a name="create-a-kubernetes-cluster-using-the-azure-cli"></a>使用 Azure CLI 建立 Kubernetes 叢集

1. 在您的新資源群組中建立 Kubernetes 叢集，例如：  
   ```azurecli 
   az acs create --orchestrator-type kubernetes --resource-group wingtiptoys-kubernetes --name wingtiptoys-cluster --generate-ssh-keys --dns-prefix=wingtiptoys
   ```
   其中：  
      * *wingtiptoys-kubernetes* 是本文先前所提及之您的資源群組名稱  
      * *wingtiptoys-cluster* 是您 Kubernetes 叢集的唯一名稱
      * *wingtiptoys* 是您應用程式 DNS 名稱的唯一名稱

   Azure CLI 將會顯示建立叢集的結果，例如：  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.Resources/deployments/azurecli0000000000.00000000000",
     "name": "azurecli0000000000.00000000000",
     "properties": {
       "correlationId": "00000000-0000-0000-0000-000000000000",
       "debugSetting": null,
       "dependencies": [],
       "mode": "Incremental",
       "outputs": {
         "masterFQDN": {
           "type": "String",
           "value": "wingtiptoysmgmt.westeurope.cloudapp.azure.com"
         },
         "sshMaster0": {
           "type": "String",
           "value": "ssh azureuser@wingtiptoysmgmt.westeurope.cloudapp.azure.com -A -p 22"
         }
       },
       "parameters": {
         "clientSecret": {
           "type": "SecureString"
         }
       },
       "parametersLink": null,
       "providers": [
         {
           "id": null,
           "namespace": "Microsoft.ContainerService",
           "registrationState": null,
           "resourceTypes": [
             {
               "aliases": null,
               "apiVersions": null,
               "locations": [
                 "westeurope"
               ],
               "properties": null,
               "resourceType": "containerServices"
             }
           ]
         }
       ],
       "provisioningState": "Succeeded",
       "template": null,
       "templateLink": null,
       "timestamp": "2017-09-15T01:00:00.000000+00:00"
     },
     "resourceGroup": "wingtiptoys-kubernetes"
   }
   ```

1. 下載您針對新 Kubernetes 叢集的認證，例如：  
   ```azurecli 
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes --name wingtiptoys-cluster
   ```

1. 使用下列命令驗證您的連線：
   ```shell 
   kubectl get nodes
   ```

   您應該會看到一份節點和狀態的清單，如下列範例所示：

   ```shell
   NAME                    STATUS                     AGE       VERSION
   k8s-agent-00000000-0    Ready                      5h        v1.6.6
   k8s-agent-00000000-1    Ready                      5h        v1.6.6
   k8s-agent-00000000-2    Ready                      5h        v1.6.6
   k8s-master-00000000-0   Ready,SchedulingDisabled   5h        v1.6.6
   ```


## <a name="create-a-private-azure-container-registry-using-the-azure-cli"></a>使用 Azure CLI 建立私人 Azure 容器登錄

1. 在您的資源群組中建立私人 Azure 容器登錄來裝載您的 Docker 映像，例如：
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes --location westeurope --name wingtiptoysregistry --sku Basic
   ```
   其中：

   | 參數 | 說明 |
   |---|---|
   | `wingtiptoys-kubernetes` | 使用本文先前所提及的資源群組來指定名稱。 |
   | `wingtiptoysregistry` | 指定私人登錄的唯一名稱。 |
   | `westeurope` | 指定適當的應用程式地理位置。 |

   Azure CLI 將會顯示建立登錄的結果，例如：  

   ```json
   {
     "adminUserEnabled": true,
     "creationDate": "2017-09-15T01:00:00.000000+00:00",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.ContainerRegistry/registries/wingtiptoysregistry",
     "location": "westeurope",
     "loginServer": "wingtiptoysregistry.azurecr.io",
     "name": "wingtiptoysregistry",
     "provisioningState": "Succeeded",
     "resourceGroup": "wingtiptoys-kubernetes",
     "sku": {
       "name": "Basic",
       "tier": "Basic"
     },
     "storageAccount": {
       "name": "wingtiptoysregistr000000"
     },
     "tags": {},
     "type": "Microsoft.ContainerRegistry/registries"
   }
   ```

2. 從 Azure CLI 擷取容器登錄的密碼。
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   Azure CLI 將會顯示登錄的密碼，例如：  

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

3. 巡覽至您 Maven 安裝的設定目錄 (預設為 ~/.m2/ 或 C:\Users\使用者名稱\.m2)，並使用文字編輯器開啟 *settings.xml* 檔案。

4. 將您的 Azure Container Registry URL、使用者名稱和密碼新增至 *settings.xml* 檔案的新 `<server>` 集合中。
   `id` 和 `username` 是登錄的名稱。 使用上一個命令的 `password` 值 (不含引號)。

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry.azurecr.io</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

5. 瀏覽至 Spring Boot 應用程式的已完成專案目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 *pom.xml* 檔案。

6. 使用 Azure Container Registry 的登入伺服器值來更新 *pom.xml* 檔案中的 `<properties>` 集合。

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

7. 更新 *pom.xml* 檔案中的 `<plugins>` 集合，以便 `<plugin>` 包含 Azure Container Registry 的登入伺服器位址和登錄名稱。

   ```xml
   <plugin>
     <groupId>com.spotify</groupId>
     <artifactId>dockerfile-maven-plugin</artifactId>
     <version>1.3.4</version>
     <configuration>
       <repository>${docker.image.prefix}/${project.artifactId}</repository>
       <serverId>${docker.image.prefix}</serverId>
       <registryUrl>https://${docker.image.prefix}</registryUrl>
     </configuration>
   </plugin>
   ```

8. 瀏覽至 Spring Boot 應用程式的已完成專案目錄，然後執行下列 Maven 命令來建置 Docker 容器，並將映像推送到您的登錄：

   ```shell
   mvn package dockerfile:build -DpushImage
   ```

   Maven 將會顯示建置的結果，例如：  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 38.113 s
   [INFO] Finished at: 2017-09-15T02:00:00-07:00
   [INFO] Final Memory: 47M/338M
   [INFO] ----------------------------------------------------
   ```


## <a name="configure-your-spring-boot-app-to-use-the-fabric8-maven-plugin"></a>設定 Spring Boot 應用程式以使用 Fabric8 Maven 外掛程式

1. 瀏覽至 Spring Boot 應用程式的已完成專案目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete*" 或 "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*")，並使用文字編輯器開啟 *pom.xml* 檔案。

1. 更新 *pom.xml* 檔案中的 `<plugins>` 集合以新增 Fabric8 Maven 外掛程式：

   ```xml
   <plugin>
     <groupId>io.fabric8</groupId>
     <artifactId>fabric8-maven-plugin</artifactId>
     <version>3.5.30</version>
     <configuration>
       <ignoreServices>false</ignoreServices>
       <registry>${docker.image.prefix}</registry>
     </configuration>
   </plugin>
   ```

1. 瀏覽至 Spring Boot 應用程式的主要來源目錄 (例如，"*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*" 或 "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete/src/main*")，並建立名稱為 "*fabric8*" 的新資料夾。

1. 在新的 *fabric8* 資料夾中建立三個 YAML 片段檔案：

   a. 建立一個含有下列內容且名為 **deployment.yml** 的檔案：
      ```yaml
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
        name: ${project.artifactId}
        labels:
          run: gs-spring-boot-docker
      spec:
        replicas: 1
        selector:
          matchLabels:
            run: gs-spring-boot-docker
        strategy:
          rollingUpdate:
            maxSurge: 1
            maxUnavailable: 1
          type: RollingUpdate
        template:
          metadata:
            labels:
              run: gs-spring-boot-docker
          spec:
            containers:
            - image: ${docker.image.prefix}/${project.artifactId}:latest
              name: gs-spring-boot-docker
              imagePullPolicy: Always
              ports:
              - containerPort: 8080
            imagePullSecrets:
              - name: mysecrets
      ```

   b. 建立一個含有下列內容且名為 **secrets.yml** 的檔案：
      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: mysecrets
        namespace: default
        annotations:
          maven.fabric8.io/dockerServerId:  ${docker.image.prefix}
      type: kubernetes.io/dockercfg
      ```

   c. 建立一個含有下列內容且名為 **service.yml** 的檔案：
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: ${project.artifactId}
        labels:
          expose: "true"
      spec:
        ports:
        - port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

1. 執行下列 Maven 命令以建置 Kubernetes 資源清單檔案：

   ```shell
   mvn fabric8:resource
   ```

   此命令會將 *src/main/fabric8* 資料夾下的所有 Kubernetes 資源 YAML 檔案合併為包含 Kubernetes 資源清單的 YAML 檔案，此檔案可以直接套用到 Kubernetes 叢集，或是匯出至 Helm 圖表。

   Maven 將會顯示建置的結果，例如：  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 16.744 s
   [INFO] Finished at: 2017-09-15T02:35:00-07:00
   [INFO] Final Memory: 30M/290M
   [INFO] ----------------------------------------------------
   ```

1. 執行下列 Maven 命令以將資源清單檔案套用至您的 Kubernetes 叢集：

   ```shell
   mvn fabric8:apply
   ```

   Maven 將會顯示建置的結果，例如：  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 14.814 s
   [INFO] Finished at: 2017-09-15T02:41:00-07:00
   [INFO] Final Memory: 35M/288M
   [INFO] ----------------------------------------------------
   ```

1. 一旦應用程式部署至叢集，請使用 `kubectl` 應用程式查詢外部 IP 位址，例如：
   ```shell
   kubectl get svc -w
   ```

   `kubectl` 將會顯示您的內部和外部 IP 位址，例如：

   ```shell
   NAME                    CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
   kubernetes              10.0.0.1     <none>        443/TCP        19h
   gs-spring-boot-docker   10.0.242.8   13.65.196.3   80:31215/TCP   3m
   ```

   您可以使用外部 IP 位址來在網頁瀏覽器中開啟應用程式。

   ![以外部方式瀏覽範例應用程式][SB02]

## <a name="delete-your-kubernetes-cluster"></a>刪除 Kubernetes 叢集

當您不再需要 Kubernetes 叢集時，可以使用 `az group delete` 命令來移除資源群組，這將會移除與其相關聯的所有資源，例如：

   ```azurecli
   az group delete --name wingtiptoys-kubernetes --yes --no-wait
   ```

## <a name="next-steps"></a>後續步驟

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-web-app-on-azure.md)
* [將 Spring Boot 應用程式部署到 Azure Container Service 中的 Linux](deploy-spring-boot-java-app-on-linux.md)
* [將 Spring Boot 應用程式部署到 Azure Container Service 中的 Kubernetes 叢集](deploy-spring-boot-java-app-on-kubernetes.md)

如需有關使用 Azure 搭配 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[適用於 Visual Studio Team Services 的 Java 工具]。

如需 Spring Boot on Docker 範例專案的進一步詳細資訊，請參閱 [Spring Boot on Docker Getting Started]。

如需開始使用您自己的 Spring Boot 應用程式的說明，請參閱 **Spring Initializr**，網址為 <https://start.spring.io/>。

如需開始建立簡單 Spring Boot 應用程式的相關詳細資訊，請參閱 Spring Initializr，網址為 <https://start.spring.io/>。

如需如何搭配 Azure 使用自訂 Docker 映像的其他範例，請參閱[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]。

<!-- URL List -->

[Azure 命令列介面 (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[適用於 Java 開發人員的 Azure]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[針對 Linux 上的 Azure Web 應用程式使用自訂 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Fabric8]: https://fabric8.io/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java 開發工具組 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[適用於 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Maven]: http://maven.apache.org/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB02.png
