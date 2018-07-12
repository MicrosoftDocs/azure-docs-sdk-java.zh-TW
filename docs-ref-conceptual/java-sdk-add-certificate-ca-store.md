---
title: 將適用於 Azure 的根憑證新增至 Java CA 存放區
description: 了解如何將憑證授權單位 (CA) 根憑證新增至 Java CA 憑證 (cacerts) 存放區，以供 Microsoft Azure 使用。
services: ''
documentationcenter: java
author: rmcmurray
manager: mbaldwin
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 07/02/2018
ms.author: robmcm
ms.openlocfilehash: 3f2de63f7eb1422ff1dd6db45d68e02f4af188b8
ms.sourcegitcommit: 0ed7c5af0152125322ff1d265c179f35028f3c15
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2018
ms.locfileid: "37864038"
---
# <a name="adding-a-root-certificate-to-the-java-ca-certificates-store"></a>新增根憑證至 Java CA 憑證存放區

使用 Azure 服務 (例如 Azure 服務匯流排) 的應用程式需要信任 Baltimore CyberTrust 根憑證。 此憑證可能已安裝在您的系統上，但如果沒有，本教學課程的步驟會示範如何使用 Oracle **keytool**，將所需的憑證授權單位 (CA) 根憑證新增至 Java CA 憑證 (cacerts) 存放區，以供 Azure 服務使用。

Oracle 的 keytool 公用程式是_金鑰和憑證管理工具_，可讓開發人員管理信任的憑證清單，以搭配 JAVA 使用。 您可以先使用 keytool 新增 CA 憑證，再將 JDK 壓縮並新增到 Azure 專案的 **approot** 資料夾，或者也可以執行使用了 keytool 來新增憑證的 Azure 啟動工作。

自 2013 年 4 月 15 日起，Azure 就已開始從 GTE CyberTrust 全域根憑證移轉到 Baltimore CyberTrust 根憑證。 下列步驟會示範如何使用 keytool 將 Baltimore CyberTrust 根憑證新增至 Java CA 憑證 (cacerts) 存放區。

> [!NOTE]
> 
> 您可以使用本文步驟來設定您的 Java SDK，以信任來自其他受信任憑證授權單位的根憑證。 例如，您可能會從 [GeoTrust 根憑證](http://www.geotrust.com/resources/root-certificates/)的憑證清單選擇根憑證。
> 

## <a name="determining-which-root-certificates-are-installed"></a>判斷已安裝哪些根憑證

Baltimore 憑證可能已安裝在您的 cacerts 存放區中，因此您需要使用下列步驟判斷是否已安裝。

1. 在管理員命令提示字元中，導覽至 JDK 的 **jdk\jre\lib\security** 資料夾，然後執行下列命令以列出安裝在系統中的憑證：

   ```shell
   keytool -list -keystore cacerts
   ```

1. 如果系統提示您輸入存放區密碼，預設密碼為 **changeit**。

   > [!NOTE]
   > 
   > 如果您想要變更存放區密碼，請參閱 keytool 文件，網址為 <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>。
   > 

1. 如果您沒有看到具有 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` 指紋的憑證，請使用下節步驟來下載及安裝憑證。

## <a name="to-add-a-root-certificate-to-the-cacerts-store"></a>新增根憑證至 cacerts 存放區

1. 從 <https://cacert.omniroot.com/bc2025.crt> 下載 Baltimore CyberTrust 根憑證，並在 **jdk\jre\lib\security** 資料夾中儲存為副檔名為 **.cer** 的本機檔案。 以這個例子而言，假設您已將 Baltimore CyberTrust 根憑證檔案下載為 **bc2025.cer**。

   > [!NOTE]
   > 
   > Baltimore CyberTrust 根憑證具有 `02:00:00:b9` 的序號，以及 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` 的 SHA1 指紋。
   > 

2. 使用下列命令將憑證匯入 cacerts 存放區：

   ```shell
   keytool -keystore cacerts -importcert -alias bc2025ca -file bc2025.cer
   ```
   其中：

   |  參數   |                              說明                               |
   |--------------|------------------------------------------------------------------------|
   | `keystore`   | 指定憑證存放區。                                       |
   | `importcert` | 指定您正在匯入憑證。                        |
   | `alias`      | 指定憑證別名。                                |
   | `file`       | 指定您正在匯入的根憑證檔案名稱。 |


3. 如果系統提示您信任憑證，請以 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` 驗證指紋，如果指紋正確則輸入 **y**。

4. 執行下列命令確定成功匯入 CA 憑證：

   ```shell
   keytool -list -keystore cacerts
   ```

在您成功將根憑證新增至 JDK 後，您可以壓縮 JDK 內容，並將其新增至 Azure 專案的 **approot** 資料夾。

## <a name="next-steps"></a>後續步驟

如需關於 keytool 公用程式的詳細資訊，請參閱 <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>。

如需 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure](/java/azure)。

<!-- For more information about the root certificates used by Azure, see [Azure Root Certificate Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx). -->
