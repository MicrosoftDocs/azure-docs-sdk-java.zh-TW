---
title: "如何對 Azure 儲存體使用 Spring Boot Starter"
description: "了解如何使用 Azure 儲存體 Starter 來設定 Spring Boot Initializer 應用程式。"
services: storage
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: yungez;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: 50c8475c66250c8e872849007349277fd3fe797b
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a>如何對 Azure 儲存體使用 Spring Boot Starter

## <a name="overview"></a>概觀

本文會逐步引導您使用 **Spring Initializr** 建立自訂應用程式，然後使用該應用程式存取 Azure 儲存體。

## <a name="prerequisites"></a>先決條件

若要遵循本文中的步驟，需要具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或註冊[免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/)。
* [Azure 命令列介面 (CLI)](http://docs.microsoft.com/cli/azure/overview)。
* 最新的 [Java 開發套件 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更新版本。
* Apache 的 [Maven](http://maven.apache.org/) 3.0 版或更新版本。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>使用 Spring Initializr 建立自訂應用程式

1. 瀏覽至 <https://start.spring.io/>。

1. 指定您想要使用 [Java] 產生 [Maven 專案]、輸入應用程式的 [群組] 和 [成品] 名稱，然後按一下 Spring Initializr 的 [切換至完整版本] 連結。

   ![Spring Initializr 的基本選項](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-basic.png)

   > [!NOTE]
   >
   > Spring Initializr 會使用 [群組] 和 [成品] 名稱來建立套件名稱；例如：com.contoso.wingtiptoysdemo。
   >

1. 向下捲動至 [Azure] 區段，然後核取 [Azure 儲存體] 的方塊。

   ![Spring Initializr 的完整選項](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-advanced.png)

1. 捲動到頁面底部，然後按一下按鈕以**產生專案**。

   ![Spring Initializr 的完整選項](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-generate.png)

1. 出現提示時，將專案下載至本機電腦上的路徑。

   ![下載自訂的 Spring Boot 專案](media/configure-spring-boot-starter-java-app-with-azure-storage/download-app.png)

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a>登入 Azure，並選取要使用的訂用帳戶

1. 開啟命令提示字元。

1. 使用 Azure CLI 登入您的 Azure 帳戶：

   ```azurecli
   az login
   ```
   依照指示完成登入程序。

1. 列出您的訂用帳戶：

   ```azurecli
   az account list
   ```
   Azure 會傳回訂用帳戶清單，而且您必須複製所要使用之訂用帳戶的 GUID；例如：

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]

1. Specify the GUID for the account you want to use with Azure; for example:

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-an-azure-storage-account"></a>建立 Azure 儲存體帳戶

1. 為您將在這篇文章中使用的 Azure 資源建立資源群組；例如：
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   其中：
   | 參數 | 說明 |
   |---|---|
   | `name` | 指定資源群組的唯一名稱。 |
   | `location` | 指定要裝載資源群組的 [Azure 區域](https://azure.microsoft.com/regions/)。 |

   Azure CLI 將會顯示建立資源群組的結果，例如：  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/wingtiptoysresources",
     "location": "westus",
     "managedBy": null,
     "name": "wingtiptoysresources",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

1. 在 Spring Boot 應用程式的資源群組中建立 Azure 儲存體帳戶；例如：
   ```azurecli
   az storage account create --name wingtiptoysstorage --resource-group wingtiptoysresources --location westus --sku Standard_LRS
   ```
   其中：
   | 參數 | 說明 |
   |---|---|
   | `name` | 指定儲存體帳戶的唯一名稱。 |
   | `resource-group` | 指定您在上一個步驟中建立的資源群組名稱。 |
   | `location` | 指定要裝載儲存體帳戶的 [Azure 區域](https://azure.microsoft.com/regions/)。 |
   | `sku` | 指定下列其中之一：`Premium_LRS`、`Standard_GRS`、`Standard_LRS`、`Standard_RAGRS`、`Standard_ZRS`。 |

   Azure 會傳回 JSON 長字串，其中包含佈建狀態；例如：|
   
   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "identity": null,
     "kind": "Storage"
       ...
       ... (A long list of values will be displayed here.)
       ...
     "statusOfPrimary": "available",
     "statusOfSecondary": null,
     "tags": {},
     "type": "Microsoft.Storage/storageAccounts"
   }
   ```

1. 擷取儲存體帳戶的連接字串；例如：
   ```azurecli
   az storage account show-connection-string --name wingtiptoysstorage --resource-group wingtiptoysresources
   ```
   其中：
   | 參數 | 說明 |
   | ---|---|
   | `name` | 指定您在先前的步驟中建立的儲存體帳戶唯一名稱。 |
   | `resource-group` | 指定您在先前的步驟中建立的資源群組名稱。 |

   Azure 會傳回 JSON 字串，其中包含儲存體帳戶的連接字串；例如：

   ```json
   {
     "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz=="
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a>設定及編譯 Spring Boot 應用程式

1. 從下載的專案封存檔將檔案解壓縮到某個目錄。

1. 瀏覽至專案中的 src/main/resources 資料夾，然後在文字編輯器中開啟 application.properties 檔案。

1. 新增儲存體帳戶的金鑰；例如：
   ```yaml
   azure.storage.connection-string=DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz==
   ```

1. 瀏覽至專案中的 /src/main/java/com/example/wingtiptoysdemo 資料夾，然後在文字編輯器中開啟 *WingtiptoysdemoApplication.java* 檔案。

1. 使用列出容器中 Blob 的下列範例取代現有的 Java 程式碼：

   ```java
   package com.example.wingtiptoysdemo;

   import com.microsoft.azure.storage.*;
   import com.microsoft.azure.storage.blob.*;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   import java.net.URISyntaxException;

   @SpringBootApplication
   public class WingtiptoysdemoApplication implements CommandLineRunner {

      @Autowired
      private CloudStorageAccount cloudStorageAccount;

      final String containerName = "mycontainer";

      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysdemoApplication.class, args);
      }

      public void run(String... var1)
             throws URISyntaxException, StorageException {
          // Create a container (if it does not exist).
          createContainerIfNotExists(containerName);
          // Upload a blob to the container.
          uploadTextBlob(containerName);
      }

      private void createContainerIfNotExists(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Create the container if it does not exist.
            container.createIfNotExists();
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }

      private void uploadTextBlob(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Get a blob reference for a text file.
            CloudBlockBlob blob = container.getBlockBlobReference("test.txt");
            // Upload some text into the blob.
            blob.uploadText("Hello World!");
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }
   }
   ```
   > [!NOTE]
   >
   > 上述範例會自動裝配您在 application.properties 檔案中所定義的儲存體帳戶設定。
   >

1. 編譯和執行應用程式：
   ```shell
   mvn clean package spring-boot:run
   ```
   
   應用程式會建立容器，並將 Blob 形式的文字檔上傳至容器，其將會列在您於 [Azure 入口網站](https://portal.azure.com)的儲存體帳戶底下。

   ![在 Azure 入口網站中列出 Blob](media/configure-spring-boot-starter-java-app-with-azure-storage/list-blobs-in-portal.png)

   > [!NOTE]
   > 
   > 在編譯應用程式時，您可能會看到下列錯誤訊息：
   > 
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] BUILD FAILURE`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] Total time: 2.616 s`<br/>
   > `[INFO] Finished at: 2017-11-11T13:14:15Z`<br/>
   > `[INFO] Final Memory: 26M/213M`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2`<br/>
   > `.18.1:test (default-test) on project wingtiptoysdemo: Execution default-test of`<br/>
   > `goal org.apache.maven.plugins:maven-surefire-plugin:2.18.1:test failed: The for`<br/>
   > `ked VM terminated without properly saying goodbye. VM crash or System.exit called?`<br/>
   > `[ERROR] Command was /bin/sh -c cd /home/robert/SpringBoot/wingtiptoysdemo && /u`<br/>
   > `sr/lib/jvm/java-8-openjdk-amd64/jre/bin/java -jar /home/robert/SpringBoot/wingt`<br/>
   > `iptoysdemo/target/surefire/surefirebooter6371623993063346766.jar /home/robert/S`<br/>
   > `pringBoot/wingtiptoysdemo/target/surefire/surefire5107893623933537917tmp /home/`<br/>
   > `robert/SpringBoot/wingtiptoysdemo/target/surefire/surefire_01414159391084128068tmp`<br/>
   > `[ERROR] -> [Help 1]`<br/>
   > 
   > 如果發生這種情況，請停用 Maven Surefire 測試；若要這樣做，請在 pom.xml 檔案中新增下列外掛程式項目：
   > 
   > ```xml
   > <plugin>
   >   <groupId>org.apache.maven.plugins</groupId>
   >   <artifactId>maven-surefire-plugin</artifactId>
   >   <version>2.20.1</version>
   >   <configuration>
   >     <skipTests>true</skipTests>
   >   </configuration>
   > </plugin>
   > ```
   > 

## <a name="next-steps"></a>後續步驟

如需可供 Microsoft Azure 使用之其他 Spring Boot Starter 的詳細資訊，請參閱[適用於 Azure 的 Spring Boot Starter](spring-boot-starters-for-azure.md)。

如需將 Azure 功能整合到 Spring 架構應用程式的其他資訊，請參閱 [Azure 上的 Spring Framework](/java/azure/spring-framework/)。

如需可從 Spring Boot 應用程式呼叫之其他 Azure 儲存體 API 的詳細資訊，請參閱下列文章：
* [如何從 Java 使用 Azure Blob 儲存體](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [如何從 Java 使用 Azure 佇列儲存體](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [如何從 Java 使用 Azure 資料表儲存體](/azure/cosmos-db/table-storage-how-to-use-java)
* [如何從 Java 使用 Azure 檔案儲存體](/azure/storage/files/storage-java-how-to-use-file-storage)
