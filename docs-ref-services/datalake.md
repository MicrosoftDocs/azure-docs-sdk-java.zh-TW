---
title: 適用於 Java 的 Azure Data Lake Store 程式庫
description: Java Data Lake Store 程式庫的參考文件
keywords: Azure, Java, SDK, API, 巨量資料, data lake
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: bcd1fd17759f7d171006d7b2126019d00d06d1db
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823721"
---
# <a name="azure-data-lake-store-libraries-for-java"></a>適用於 Java 的 Azure Data Lake Store 程式庫

## <a name="overview"></a>概觀

使用 [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview) 在單一位置擷取任何大小、類型和擷取速度的資料以供分析。

若要開始使用 Data Lake Store，請參閱[使用 Java 開始使用 Azure Data Lake Store](/azure/data-lake-store/data-lake-store-get-started-java-sdk)。


## <a name="client-library"></a>用戶端程式庫

使用用戶端程式庫在 Data Lake Store 中讀取和寫入檔案、設定權限和中繼資料，以及管理檔案和目錄。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

## <a name="example"></a>範例

透過完整網域名稱和 OAuth2 存取權杖建立 Data Lake 用戶端，然後在 Data Lake 中建立檔案並將檔案寫入其中。

```java
// AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);
ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

// create directory
client.createDirectory("/a/b/w");
        
// create file and write some content
String filename = "/a/b/c.txt";
OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
PrintStream out = new PrintStream(stream);
for (int i = 1; i <= 10; i++) {
    out.println("This is line #" + i);
    out.format("This is the same line (%d), but using formatted output. %n", i);
}
out.close();
```

> [!div class="nextstepaction"]
> [探索用戶端 API](/java/api/overview/azure/datalakestore/client)


## <a name="management-api"></a>管理 API

使用管理 API 來管理 Data Lake Store 帳戶、防火牆規則和受信任的識別提供者。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-store</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/datalakestore/management)

## <a name="samples"></a>範例

[開始使用 Azure Data Lake][1] 

[1]: https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started

深入探索可在應用程式中使用的 [ Java 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=java&term=lake)。
