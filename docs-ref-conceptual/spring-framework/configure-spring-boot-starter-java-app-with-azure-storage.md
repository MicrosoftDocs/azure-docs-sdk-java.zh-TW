---
title: 如何對 Azure 儲存體使用 Spring Boot Starter
description: 了解如何使用 Azure 儲存體 Starter 來設定 Spring Boot Initializer 應用程式。
services: storage
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: a1f35df5939a51fa5ce50c29752226b6c7649a9d
ms.sourcegitcommit: 1c1412ad5d8960975c3fc7fd3d1948152ef651ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57335381"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a>如何對 Azure 儲存體使用 Spring Boot Starter

## <a name="overview"></a>概觀

本文將逐步引導您使用 **Spring Initializr** 建立自訂應用程式，並將 Azure 儲存體 Starter 新增至您的應用程式，然後使用您的應用程式將 Blob 上傳至 Azure 儲存體帳戶。

## <a name="prerequisites"></a>必要條件

若要遵循本文中的步驟，需要具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或註冊[免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/)。
* [Azure 命令列介面 (CLI)](http://docs.microsoft.com/cli/azure/overview)。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven](http://maven.apache.org/) 3.0 版或更新版本。

> [!IMPORTANT]
>
> 您需要 Spring Boot 2.0 版或更新版本才能完成本文中的步驟。
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a>為您的應用程式建立 Azure 儲存體帳戶和 Blob 容器

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 依序按一下 [+ 建立資源]、[儲存體] 和 [儲存體帳戶]。

   ![建立 Azure 儲存體帳戶][IMG01]

1. 在 [建立命名空間] 頁面上，輸入下列資訊：

   * 輸入唯一**名稱**，這將成為儲存體帳戶 URI 的一部分。 例如：如果您輸入 **wingtiptoysstorage** 作為**名稱**，則 URI 會是 wingtiptoysstorage.core.windows.net。
   * 選擇 [Blob 儲存體] 作為 [帳戶種類]。
   * 指定儲存體帳戶的**位置**。
   * 選取您想要用於儲存體帳戶的**訂用帳戶**。
   * 指定是否要為儲存體帳戶建立新的**資源群組**，或選擇現有的資源群組。

   ![指定 Azure 儲存體帳戶選項][IMG02]

1. 當您指定上面列出的選項之後，按一下 [建立] 以建立您的儲存體帳戶。

1. 在 Azure 入口網站建立儲存體帳戶之後，按一下 [Blob]，然後按一下 [+ 容器]。

   ![建立 Blob 容器][IMG03]

1. 請輸入 Blob 容器的**名稱**，然後按一下 [確定]。

   ![指定 Blob 容器選項][IMG04]

1. Blob 容器會在建好後出現在 Azure 入口網站中。

   ![檢視 Blob 容器的清單][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 建立簡單的 Spring Boot 應用程式

1. 瀏覽至 <https://start.spring.io/> 。

1. 指定下列選項：

   * 使用 **Java** 產生 **Maven** 專案。
   * 指定 **Spring Boot** 版本，應等於或大於 2.0。
   * 指定應用程式的**群組**和**成品**名稱。
   * 新增 **Web** 相依性。

      ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：com.wingtiptoys.storage。
   >

1. 當您指定上面列出的選項之後，按一下 [產生專案]。

1. 出現提示時，將專案下載至本機電腦上的路徑。

   ![下載 Spring 專案][SI02]

1. 當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a>設定 Spring Boot 應用程式以使用 Azure Storage Starter

1. 在應用程式的根目錄中尋找 pom.xml 檔案；例如：

   `C:\SpringBoot\storage\pom.xml`

   -或-

   `/users/example/home/storage/pom.xml`

1. 在文字編輯器中開啟 pom.xml 檔案，並將 Spring Cloud Azure 儲存體 Starter 新增至 `<dependencies>` 的清單中：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-azure-starter-storage</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][SI03]

1. 儲存並關閉 *pom.xml* 檔案。

## <a name="create-an-azure-credential-file"></a>建立 Azure 認證檔案

1. 開啟命令提示字元。

1. 瀏覽至 Spring Boot 應用程式的 resources 目錄；例如：

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   -或-

   ```shell
   cd /users/example/home/storage/src/main/resources
   ```

1. 登入您的 Azure 帳戶：

   ```azurecli
   az login
   ```

1. 列出您的訂用帳戶：

   ```azurecli
   az account list
   ```
   Azure 會傳回訂用帳戶清單，而且您必須複製所要使用之訂用帳戶的 GUID；例如：

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. 指定您希望在 Azure 中使用的訂用帳戶 GUID，例如：

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. 建立 Azure 認證檔案：

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   此命令會在您的 resources 目錄中建立 my.azureauth 檔案，內含會類似下列範例：

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a>設定 Spring Boot 應用程式以使用您的 Azure 儲存體帳戶

1. 在應用程式的 resources 目錄中尋找 *application.properties*；例如：

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   -或-

   `/users/example/home/storage/src/main/resources/application.properties`

2. 在文字編輯器中開啟 application.properties 檔案、新增下列數行，然後使用您儲存體帳戶的適當屬性來取代範例值：

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.storage.account=wingtiptoysstorage
   ```
   其中：

   |                   欄位                   |                                            說明                                            |
   |-------------------------------------------|---------------------------------------------------------------------------------------------------|
   | `spring.cloud.azure.credential-file-path` |            指定您稍早在本教學課程中建立的 Azure 認證檔案。             |
   |    `spring.cloud.azure.resource-group`    |           指定包含您 Azure 儲存體帳戶的 Azure 資源群組。            |
   |        `spring.cloud.azure.region`        | 指定建立您 Azure 儲存體帳戶時指定的地理區域。 |
   |   `spring.cloud.azure.storage.account`    |            指定您稍早在本教學課程中建立的 Azure 儲存體帳戶。             |


3. 儲存並關閉 *application.properties* 檔案。

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a>新增範例程式碼來實作基本的 Azure 儲存體功能

在本節中，您會建立必要的 Java 類別，以便將 Blob 儲存在 Azure 儲存體帳戶中。

### <a name="modify-the-main-application-class"></a>修改主要應用程式類別

1. 在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   -或-

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. 在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class StorageApplication {
      public static void main(String[] args) {
         SpringApplication.run(StorageApplication.class, args);
      }
   }
   ```

1. 儲存並關閉主要應用程式 Java 檔案。

### <a name="add-a-web-controller-class"></a>新增 Web 控制器類別

1. 在應用程式的 package 目錄中建立名為 *WebController.java* 的新 Java 檔案；例如：

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\WebController.java`

   -或-

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/WebController.java`

1. 在文字編輯器中開啟Web 控制器 Java 檔案，並將下列數行新增至檔案中：

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.core.io.Resource;
   import org.springframework.core.io.WritableResource;
   import org.springframework.util.StreamUtils;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;

   import java.io.IOException;
   import java.io.OutputStream;
   import java.nio.charset.Charset;

   @RestController
   public class WebController {

      @Value("blob://test/myfile.txt")
      private Resource blobFile;

      @GetMapping(value = "/")
      public String readBlobFile() throws IOException {
         return StreamUtils.copyToString(
            this.blobFile.getInputStream(),
            Charset.defaultCharset()) + "\n";
      }

      @PostMapping(value = "/")
      public String writeBlobFile(@RequestBody String data) throws IOException {
         try (OutputStream os = ((WritableResource) this.blobFile).getOutputStream()) {
            os.write(data.getBytes());
         }
         return "File was updated.\n";
      }
   }
   ```

   其中 `@Value("blob://[container]/[blob]")` 語法會分別為用來儲存資料的容器和 Blob 定義名稱。

1. 儲存並關閉 Web 控制器 Java 檔案。

1. 開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：

   `cd C:\SpringBoot\storage`

   -或-

   `cd /users/example/home/storage`

1. 使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 當您的應用程式執行時，可以使用cURL 來測試您的應用程式；例如：

   a. 傳送 POST 要求來更新檔案的內容：

      ```shell
      curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
      ```

      您應該會看到檔案已更新的回應。

   b. 傳送 GET 要求來驗證檔案的內容：

      ```shell
      curl -X GET http://localhost:8080/
      ```

     您應該會看到您所發佈的 "Hello World" 文字。

## <a name="summary"></a>總結

在此教學課程中，您已使用 **[Spring Initializr]** 建立了新的 Java 應用程式、也對應用程式新增了 Azure 儲存體入門版，同時已將應用程式設定為要將 Blob 上傳至 Azure 儲存體帳戶。

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他資源

如需可供 Microsoft Azure 使用之其他 Spring Boot Starter 的詳細資訊，請參閱[適用於 Azure 的 Spring Boot Starter](spring-boot-starters-for-azure.md)。

如需可從 Spring Boot 應用程式呼叫之其他 Azure 儲存體 API 的詳細資訊，請參閱下列文章：
* [如何從 Java 使用 Azure Blob 儲存體](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [如何從 Java 使用 Azure 佇列儲存體](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [如何從 Java 使用 Azure 資料表儲存體](/azure/cosmos-db/table-storage-how-to-use-java)
* [如何從 Java 使用 Azure 檔案儲存體](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-03.png
