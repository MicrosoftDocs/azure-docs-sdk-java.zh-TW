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
ms.date: 05/23/2018
ms.author: robmcm
ms.openlocfilehash: 29b2b598968c9a3a896fffee3ce56f9b0cb4b1ee
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090731"
---
# <a name="adding-a-root-certificate-to-the-java-ca-certificates-store"></a><span data-ttu-id="b61fc-103">新增根憑證至 Java CA 憑證存放區</span><span class="sxs-lookup"><span data-stu-id="b61fc-103">Adding a root certificate to the Java CA certificates store</span></span>

<span data-ttu-id="b61fc-104">使用 Azure 服務 (例如 Azure 服務匯流排) 的應用程式需要信任 Baltimore CyberTrust 根憑證。</span><span class="sxs-lookup"><span data-stu-id="b61fc-104">Applications that use Azure services (such as Azure Service Bus) need to trust the Baltimore CyberTrust root certificate.</span></span> <span data-ttu-id="b61fc-105">此憑證可能已安裝在您的系統上，但如果沒有，本教學課程的步驟會示範如何使用 Oracle **keytool**，將所需的憑證授權單位 (CA) 根憑證新增至 Java CA 憑證 (cacerts) 存放區，以供 Azure 服務使用。</span><span class="sxs-lookup"><span data-stu-id="b61fc-105">This certificate may already be installed on your system, but if it is not, the steps in this tutorial will show you how to use Oracle's **keytool** to add the required certificate authority (CA) root certificate to the Java CA certificate (cacerts) store that you will use for Azure services.</span></span>

<span data-ttu-id="b61fc-106">Oracle 的 keytool 公用程式是_金鑰和憑證管理工具_，可讓開發人員管理信任的憑證清單，以搭配 JAVA 使用。</span><span class="sxs-lookup"><span data-stu-id="b61fc-106">Oracle's keytool utility is a _Key and Certificate Management Tool_, which allows developers to manage the list of trusted certificates for use with Java.</span></span> <span data-ttu-id="b61fc-107">您可以先使用 keytool 新增 CA 憑證，再將 JDK 壓縮並新增到 Azure 專案的 **approot** 資料夾，或者也可以執行使用了 keytool 來新增憑證的 Azure 啟動工作。</span><span class="sxs-lookup"><span data-stu-id="b61fc-107">You can use keytool to add the CA certificate before zipping your JDK and adding it to your Azure project's **approot** folder, or you could run an Azure start-up task that uses keytool to add the certificate.</span></span>

<span data-ttu-id="b61fc-108">自 2013 年 4 月 15 日起，Azure 就已開始從 GTE CyberTrust 全域根憑證移轉到 Baltimore CyberTrust 根憑證。</span><span class="sxs-lookup"><span data-stu-id="b61fc-108">Beginning April 15, 2013, Azure began migrating from the GTE CyberTrust Global root certificate to the Baltimore CyberTrust root certificate.</span></span> <span data-ttu-id="b61fc-109">下列步驟會示範如何使用 keytool 將 Baltimore CyberTrust 根憑證新增至 Java CA 憑證 (cacerts) 存放區。</span><span class="sxs-lookup"><span data-stu-id="b61fc-109">The following steps show you how to use keytool to add the Baltimore CyberTrust root certificate to your Java CA certificate (cacerts) store.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="b61fc-110">您可以使用本文步驟來設定您的 Java SDK，以信任來自其他受信任憑證授權單位的根憑證。</span><span class="sxs-lookup"><span data-stu-id="b61fc-110">You can use the steps in this article to configure your Java SDK to trust the root certificates from other trusted certificate authorities.</span></span> <span data-ttu-id="b61fc-111">例如，您可能會從 [GeoTrust 根憑證](http://www.geotrust.com/resources/root-certificates/)的憑證清單選擇根憑證。</span><span class="sxs-lookup"><span data-stu-id="b61fc-111">For example, you might choose a root certificate from the list of certificates at [GeoTrust Root Certificates](http://www.geotrust.com/resources/root-certificates/).</span></span>
> 

## <a name="determining-which-root-certificates-are-installed"></a><span data-ttu-id="b61fc-112">判斷已安裝哪些根憑證</span><span class="sxs-lookup"><span data-stu-id="b61fc-112">Determining which root certificates are installed</span></span>

<span data-ttu-id="b61fc-113">Baltimore 憑證可能已安裝在您的 cacerts 存放區中，因此您需要使用下列步驟判斷是否已安裝。</span><span class="sxs-lookup"><span data-stu-id="b61fc-113">The Baltimore certificate might already be installed in your cacerts store, so you need to use the following steps to determine if it has already been installed.</span></span>

1. <span data-ttu-id="b61fc-114">在管理員命令提示字元中，導覽至 JDK 的 **jdk\jre\lib\security** 資料夾，然後執行下列命令以列出安裝在系統中的憑證：</span><span class="sxs-lookup"><span data-stu-id="b61fc-114">At an administrator command prompt, navigate to your JDK's **jdk\jre\lib\security** folder, and then run the following command to list the certificates that are installed on your system:</span></span>

   ```shell
   keytool -list -keystore cacerts
   ```

1. <span data-ttu-id="b61fc-115">如果系統提示您輸入存放區密碼，預設密碼為 **changeit**。</span><span class="sxs-lookup"><span data-stu-id="b61fc-115">If you are prompted for the store password, the default password is **changeit**.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="b61fc-116">如果您想要變更存放區密碼，請參閱 keytool 文件，網址為 <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>。</span><span class="sxs-lookup"><span data-stu-id="b61fc-116">If you want to change the store password, see the keytool documentation at <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>
   > 

1. <span data-ttu-id="b61fc-117">如果您沒有看到具有 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` 指紋的憑證，請使用下節步驟來下載及安裝憑證。</span><span class="sxs-lookup"><span data-stu-id="b61fc-117">If you do not see the certificate with the thumbprint of `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, use the steps in the following section to download and install the certificate.</span></span>

## <a name="to-add-a-root-certificate-to-the-cacerts-store"></a><span data-ttu-id="b61fc-118">新增根憑證至 cacerts 存放區</span><span class="sxs-lookup"><span data-stu-id="b61fc-118">To add a root certificate to the cacerts store</span></span>

1. <span data-ttu-id="b61fc-119">從 <https://cacert.omniroot.com/bc2025.crt> 下載 Baltimore CyberTrust 根憑證，並在 **jdk\jre\lib\security** 資料夾中儲存為副檔名為 **.cer** 的本機檔案。</span><span class="sxs-lookup"><span data-stu-id="b61fc-119">Download the Baltimore CyberTrust root certificate from <https://cacert.omniroot.com/bc2025.crt>, and save to a local file with extension **.cer** in your **jdk\jre\lib\security** folder.</span></span> <span data-ttu-id="b61fc-120">以這個例子而言，假設您已將 Baltimore CyberTrust 根憑證檔案下載為 **bc2025.cer**。</span><span class="sxs-lookup"><span data-stu-id="b61fc-120">For this example, assume that you downloaded the Baltimore CyberTrust root certificate file as **bc2025.cer**.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="b61fc-121">Baltimore CyberTrust 根憑證具有 `02:00:00:b9` 的序號，以及 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` 的 SHA1 指紋。</span><span class="sxs-lookup"><span data-stu-id="b61fc-121">The Baltimore CyberTrust root certificate has a serial number of `02:00:00:b9`, and a SHA1 thumbprint of `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`.</span></span>
   > 

2. <span data-ttu-id="b61fc-122">使用下列命令將憑證匯入 cacerts 存放區：</span><span class="sxs-lookup"><span data-stu-id="b61fc-122">Import the certificate to the cacerts store by using the following command:</span></span>

   ```shell
   keytool -keystore cacerts -importcert -alias bc2025ca -file bc2025.cer
   ```
   <span data-ttu-id="b61fc-123">其中：</span><span class="sxs-lookup"><span data-stu-id="b61fc-123">Where:</span></span>

   |  <span data-ttu-id="b61fc-124">參數</span><span class="sxs-lookup"><span data-stu-id="b61fc-124">Parameter</span></span>   |                              <span data-ttu-id="b61fc-125">說明</span><span class="sxs-lookup"><span data-stu-id="b61fc-125">Description</span></span>                               |
   |--------------|------------------------------------------------------------------------|
   |  `keystore`  |                    <span data-ttu-id="b61fc-126">指定憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="b61fc-126">Specifies the certificate store.</span></span>                    |
   | `importcert` |            <span data-ttu-id="b61fc-127">指定您正在匯入憑證。</span><span class="sxs-lookup"><span data-stu-id="b61fc-127">Specifies that you are importing a certificate.</span></span>             |
   |   `alias`    |                <span data-ttu-id="b61fc-128">指定憑證別名。</span><span class="sxs-lookup"><span data-stu-id="b61fc-128">Specifies an alias for the certificate.</span></span>                 |
   |    `file`    | <span data-ttu-id="b61fc-129">指定您正在匯入的根憑證檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="b61fc-129">Specifies the filename of the root certificate that you are importing.</span></span> |


3. <span data-ttu-id="b61fc-130">如果系統提示您信任憑證，請以 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` 驗證指紋，如果指紋正確則輸入 **y**。</span><span class="sxs-lookup"><span data-stu-id="b61fc-130">If you are prompted to trust the certificate, verify the thumbprint as `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, and type **y** if the thumbprint is correct.</span></span>

4. <span data-ttu-id="b61fc-131">執行下列命令確定成功匯入 CA 憑證：</span><span class="sxs-lookup"><span data-stu-id="b61fc-131">Run the following command to ensure the CA certificate has been successfully imported:</span></span>

   ```shell
   keytool -list -keystore cacerts
   ```

<span data-ttu-id="b61fc-132">在您成功將根憑證新增至 JDK 後，您可以壓縮 JDK 內容，並將其新增至 Azure 專案的 **approot** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b61fc-132">After you have successfully added the root certificate to your JDK, you can zip the contents of JDK and add it to your Azure project's **approot** folder.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b61fc-133">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b61fc-133">Next steps</span></span>

<span data-ttu-id="b61fc-134">如需關於 keytool 公用程式的詳細資訊，請參閱 <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>。</span><span class="sxs-lookup"><span data-stu-id="b61fc-134">For more information about the keytool utility, see <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>

<span data-ttu-id="b61fc-135">如需 Azure 所用根憑證的詳細資訊，請參閱 [Azure 根憑證移轉](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b61fc-135">For more information about the root certificates used by Azure, see [Azure Root Certificate Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).</span></span>

<span data-ttu-id="b61fc-136">如需 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure](/java/azure)。</span><span class="sxs-lookup"><span data-stu-id="b61fc-136">For more information about Java, see [Azure for Java developers](/java/azure).</span></span>
