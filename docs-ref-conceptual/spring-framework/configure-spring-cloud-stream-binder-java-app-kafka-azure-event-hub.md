---
title: 如何搭配 Azure 事件中樞使用適用於 Apache Kafka 的 Spring Boot Starter
description: 了解如何將使用 Spring Boot Initializer 所建立的應用程式設定為搭配使用 Apache Kafka 與 Azure 事件中樞。
services: event-hubs
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: na
ms.openlocfilehash: f2cf66a4e0ac113406781b4859869ff4edab527e
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991532"
---
# <a name="how-to-use-the-spring-boot-starter-for-apache-kafka-with-azure-event-hubs"></a><span data-ttu-id="38519-103">如何搭配 Azure 事件中樞使用適用於 Apache Kafka 的 Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="38519-103">How to use the Spring Boot Starter for Apache Kafka with Azure Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="38519-104">概觀</span><span class="sxs-lookup"><span data-stu-id="38519-104">Overview</span></span>

<span data-ttu-id="38519-105">本文會示範如何將使用 Spring Boot Initializer 建立的 Java 架構 Spring Cloud Stream Binder 設定為搭配使用 [Apache Kafka] 與 Azure 事件中樞。</span><span class="sxs-lookup"><span data-stu-id="38519-105">This article demonstrates how to configure a Java-based Spring Cloud Stream Binder created with the Spring Boot Initializer to use [Apache Kafka] with Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="38519-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="38519-106">Prerequisites</span></span>

<span data-ttu-id="38519-107">若要遵循本文中的步驟，需要具備下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="38519-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="38519-108">Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。</span><span class="sxs-lookup"><span data-stu-id="38519-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="38519-109">受支援的 Java 開發套件 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="38519-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="38519-110">如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="38519-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="38519-111">[Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="38519-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="38519-112">您需要 Spring Boot 2.0 版或更新版本才能完成本文中的步驟。</span><span class="sxs-lookup"><span data-stu-id="38519-112">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a><span data-ttu-id="38519-113">使用 Azure 入口網站建立 Azure 事件中樞</span><span class="sxs-lookup"><span data-stu-id="38519-113">Create an Azure Event Hub using the Azure portal</span></span>

### <a name="create-an-azure-event-hub-namespace"></a><span data-ttu-id="38519-114">建立 Azure 事件中樞命名空間</span><span class="sxs-lookup"><span data-stu-id="38519-114">Create an Azure Event Hub Namespace</span></span>

1. <span data-ttu-id="38519-115">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。</span><span class="sxs-lookup"><span data-stu-id="38519-115">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="38519-116">依序按一下 [+ 建立資源]、[物聯網] 和 [事件中樞]。</span><span class="sxs-lookup"><span data-stu-id="38519-116">Click **+Create a resource**, then **Internet of Things**, and then click **Event Hubs**.</span></span>

   ![建立 Azure 事件中樞命名空間][IMG01]

1. <span data-ttu-id="38519-118">在 [建立命名空間] 頁面上，輸入下列資訊：</span><span class="sxs-lookup"><span data-stu-id="38519-118">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="38519-119">輸入唯一**名稱**，這將成為事件中樞命名空間 URI 的一部分。</span><span class="sxs-lookup"><span data-stu-id="38519-119">Enter a unique **Name**, which will become part of the URI for your event hub namespace.</span></span> <span data-ttu-id="38519-120">例如：如果您輸入 **wingtiptoys** 作為**名稱**，則 URI 會是 wingtiptoys.servicebus.windows.net。</span><span class="sxs-lookup"><span data-stu-id="38519-120">For example: if you entered **wingtiptoys** for the **Name**, the URI would be *wingtiptoys.servicebus.windows.net*.</span></span>
   * <span data-ttu-id="38519-121">為事件中樞命名空間選擇 [定價層]。</span><span class="sxs-lookup"><span data-stu-id="38519-121">Choose a **Pricing tier** for your event hub namespace.</span></span>
   * <span data-ttu-id="38519-122">為命名空間指定 [啟用 Kafka]。</span><span class="sxs-lookup"><span data-stu-id="38519-122">Specify **Enable Kafka** for your namespace.</span></span>
   * <span data-ttu-id="38519-123">選取您想要用於命名空間的**訂用帳戶**。</span><span class="sxs-lookup"><span data-stu-id="38519-123">Choose the **Subscription** you want to use for your namespace.</span></span>
   * <span data-ttu-id="38519-124">指定是否要為命名空間建立新的**資源群組**，或選擇現有的資源群組。</span><span class="sxs-lookup"><span data-stu-id="38519-124">Specify whether to create a new **Resource group** for your namespace, or choose an existing resource group.</span></span>
   * <span data-ttu-id="38519-125">指定事件中樞命名空間的**位置**。</span><span class="sxs-lookup"><span data-stu-id="38519-125">Specify the **Location** for your event hub namespace.</span></span>

   ![指定 Azure 事件中樞命名空間選項][IMG02]

1. <span data-ttu-id="38519-127">指定上面列出的選項之後，按一下 [建立] 以建立您的命名空間。</span><span class="sxs-lookup"><span data-stu-id="38519-127">When you have specified the options listed above, click **Create** to create your namespace.</span></span>

### <a name="create-an-azure-event-hub-in-your-namespace"></a><span data-ttu-id="38519-128">在命名空間中建立 Azure 事件中樞</span><span class="sxs-lookup"><span data-stu-id="38519-128">Create an Azure Event Hub in your namespace</span></span>

1. <span data-ttu-id="38519-129">從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="38519-129">Browse to the Azure portal at <https://portal.azure.com/>.</span></span>

1. <span data-ttu-id="38519-130">按一下 [所有資源]，然後按一下您建立的命名空間。</span><span class="sxs-lookup"><span data-stu-id="38519-130">Click **All resources**, and then click the namespace that you created.</span></span>

   ![選取 Azure 事件中樞命名空間][IMG03]

1. <span data-ttu-id="38519-132">按一下 [事件中樞]，然後按一下 [+ 事件中樞]。</span><span class="sxs-lookup"><span data-stu-id="38519-132">Click **Event Hubs**, and then click **+Event Hub**.</span></span>

   ![新增 Azure 事件中樞][IMG04]

1. <span data-ttu-id="38519-134">在 [建立事件中樞] 頁面上，為您的事件中樞輸入唯一**名稱**，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="38519-134">On the **Create Event Hub** page, enter a unique **Name** for your Event Hub, and then click **Create**.</span></span>

   ![建立 Azure 事件中樞][IMG05]

1. <span data-ttu-id="38519-136">事件中樞建好之後，即會列在 [事件中樞] 頁面上。</span><span class="sxs-lookup"><span data-stu-id="38519-136">When your Event Hub has been created, it will be listed on the **Event Hubs** page.</span></span>

   ![建立 Azure 事件中樞][IMG06]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="38519-138">使用 Spring Initializr 建立簡單的 Spring Boot 應用程式</span><span class="sxs-lookup"><span data-stu-id="38519-138">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="38519-139">瀏覽至 <https://start.spring.io/> 。</span><span class="sxs-lookup"><span data-stu-id="38519-139">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="38519-140">指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="38519-140">Specify the following options:</span></span>

   * <span data-ttu-id="38519-141">使用 **Java** 產生 **Maven** 專案。</span><span class="sxs-lookup"><span data-stu-id="38519-141">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="38519-142">指定 **Spring Boot** 版本，應等於或大於 2.0。</span><span class="sxs-lookup"><span data-stu-id="38519-142">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="38519-143">指定應用程式的**群組**和**成品**名稱。</span><span class="sxs-lookup"><span data-stu-id="38519-143">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="38519-144">新增 **Web** 相依性。</span><span class="sxs-lookup"><span data-stu-id="38519-144">Add the **Web** dependency.</span></span>

      ![Spring Initializr 的基本選項][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="38519-146">Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：com.wingtiptoys.kafka。</span><span class="sxs-lookup"><span data-stu-id="38519-146">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.kafka*.</span></span>
   >

1. <span data-ttu-id="38519-147">當您指定上面列出的選項之後，按一下 [產生專案]。</span><span class="sxs-lookup"><span data-stu-id="38519-147">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="38519-148">出現提示時，將專案下載至本機電腦上的路徑。</span><span class="sxs-lookup"><span data-stu-id="38519-148">When prompted, download the project to a path on your local computer.</span></span>

   ![下載 Spring 專案][SI02]

1. <span data-ttu-id="38519-150">當您在本機系統上擷取檔案之後，就可以開始編輯簡單的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="38519-150">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-spring-cloud-kafka-stream-and-azure-event-hub-starters"></a><span data-ttu-id="38519-151">將 Spring Boot 應用程式設定為使用 Spring Cloud Kafka Stream 與 Azure 事件中樞 Starter</span><span class="sxs-lookup"><span data-stu-id="38519-151">Configure your Spring Boot app to use the Spring Cloud Kafka Stream and Azure Event Hub starters</span></span>

1. <span data-ttu-id="38519-152">在應用程式的根目錄中尋找 pom.xml 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="38519-152">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\kafka\pom.xml`

   <span data-ttu-id="38519-153">-或-</span><span class="sxs-lookup"><span data-stu-id="38519-153">-or-</span></span>

   `/users/example/home/kafka/pom.xml`

1. <span data-ttu-id="38519-154">在文字編輯器中開啟 pom.xml 檔案，並將 Spring Cloud Kafka Stream 和 Azure 事件中樞 Starter 新增至 `<dependencies>` 的清單：</span><span class="sxs-lookup"><span data-stu-id="38519-154">Open the *pom.xml* file in a text editor, and add the Spring Cloud Kafka Stream and Azure Event Hub starters to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-stream-kafka</artifactId>
      <version>2.0.1.RELEASE</version>
   </dependency>
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-starter-eventhub</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![編輯 pom.xml 檔案][SI03]

1. <span data-ttu-id="38519-156">儲存並關閉 *pom.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="38519-156">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="38519-157">建立 Azure 認證檔案</span><span class="sxs-lookup"><span data-stu-id="38519-157">Create an Azure Credential File</span></span>

1. <span data-ttu-id="38519-158">開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="38519-158">Open a command prompt.</span></span>

1. <span data-ttu-id="38519-159">瀏覽至 Spring Boot 應用程式的 resources 目錄；例如：</span><span class="sxs-lookup"><span data-stu-id="38519-159">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\eventhub\src\main\resources
   ```

   <span data-ttu-id="38519-160">-或-</span><span class="sxs-lookup"><span data-stu-id="38519-160">-or-</span></span>

   ```shell
   cd /users/example/home/eventhub/src/main/resources
   ```

1. <span data-ttu-id="38519-161">登入您的 Azure 帳戶：</span><span class="sxs-lookup"><span data-stu-id="38519-161">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="38519-162">列出您的訂用帳戶：</span><span class="sxs-lookup"><span data-stu-id="38519-162">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="38519-163">Azure 會傳回訂用帳戶清單，而且您必須複製所要使用之訂用帳戶的 GUID；例如：</span><span class="sxs-lookup"><span data-stu-id="38519-163">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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
   
1. <span data-ttu-id="38519-164">指定您希望在 Azure 中使用的訂用帳戶 GUID，例如：</span><span class="sxs-lookup"><span data-stu-id="38519-164">Specify the GUID for the subscription you want to use with Azure; for example:</span></span>

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. <span data-ttu-id="38519-165">建立 Azure 認證檔案：</span><span class="sxs-lookup"><span data-stu-id="38519-165">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="38519-166">此命令會在您的 resources 目錄中建立 my.azureauth 檔案，內含會類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="38519-166">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

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

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a><span data-ttu-id="38519-167">設定 Spring Boot 應用程式以使用您的 Azure 事件中樞</span><span class="sxs-lookup"><span data-stu-id="38519-167">Configure your Spring Boot app to use your Azure Event Hub</span></span>

1. <span data-ttu-id="38519-168">在應用程式的 resources 目錄中尋找 *application.properties*；例如：</span><span class="sxs-lookup"><span data-stu-id="38519-168">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\src\main\resources\application.properties`

   <span data-ttu-id="38519-169">-或-</span><span class="sxs-lookup"><span data-stu-id="38519-169">-or-</span></span>

   `/users/example/home/eventhub/src/main/resources/application.properties`

2. <span data-ttu-id="38519-170">在文字編輯器中開啟 application.properties 檔案、新增下列數行，然後使用您事件中樞的適當屬性來取代範例值：</span><span class="sxs-lookup"><span data-stu-id="38519-170">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your event hub:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace

   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   ```
   <span data-ttu-id="38519-171">其中：</span><span class="sxs-lookup"><span data-stu-id="38519-171">Where:</span></span>

   |                       <span data-ttu-id="38519-172">欄位</span><span class="sxs-lookup"><span data-stu-id="38519-172">Field</span></span>                       |                                                                                   <span data-ttu-id="38519-173">說明</span><span class="sxs-lookup"><span data-stu-id="38519-173">Description</span></span>                                                                                    |
   |---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `spring.cloud.azure.credential-file-path`     |                                                    <span data-ttu-id="38519-174">指定您稍早在本教學課程中建立的 Azure 認證檔案。</span><span class="sxs-lookup"><span data-stu-id="38519-174">Specifies Azure credential file that you created earlier in this tutorial.</span></span>                                                    |
   |        `spring.cloud.azure.resource-group`        |                                                      <span data-ttu-id="38519-175">指定包含您 Azure 事件中樞的 Azure 資源群組。</span><span class="sxs-lookup"><span data-stu-id="38519-175">Specifies the Azure Resource Group that contains your Azure Event Hub.</span></span>                                                      |
   |            `spring.cloud.azure.region`            |                                           <span data-ttu-id="38519-176">指定建立您 Azure 事件中樞時指定的地理區域。</span><span class="sxs-lookup"><span data-stu-id="38519-176">Specifies the geographical region that you specified when you created your Azure Event Hub.</span></span>                                            |
   |      `spring.cloud.azure.eventhub.namespace`      |                                          <span data-ttu-id="38519-177">指定建立您 Azure 事件中樞命名空間時指定的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="38519-177">Specifies the unique name that you specified when you created your Azure Event Hub Namespace.</span></span>                                           |
   | `spring.cloud.stream.bindings.input.destination`  |                            <span data-ttu-id="38519-178">指定作為輸入目的地的 Azure 事件中樞，在本教學課程中是指您稍早在本教學課程中建立的中樞。</span><span class="sxs-lookup"><span data-stu-id="38519-178">Specifies the input destination Azure Event Hub, which for this tutorial is the  hub you created earlier in this tutorial.</span></span>                            |
   |    `spring.cloud.stream.bindings.input.group `    | <span data-ttu-id="38519-179">指定 Azure 事件中樞中的取用者群組，可以設定為 '$Default'，以使用您建立 Azure 事件中樞時所建立的基本取用者群組。</span><span class="sxs-lookup"><span data-stu-id="38519-179">Specifies a Consumer Group from Azure Event Hub, which can be set to '$Default' in order to use the basic consumer group that was created when you created your Azure Event Hub.</span></span> |
   | `spring.cloud.stream.bindings.output.destination` |                               <span data-ttu-id="38519-180">指定作為輸出目的地的 Azure 事件中樞，這在本教學課程中會等同輸入目的地。</span><span class="sxs-lookup"><span data-stu-id="38519-180">Specifies the output destination Azure Event Hub, which for this tutorial will be the same as the input destination.</span></span>                               |


3. <span data-ttu-id="38519-181">儲存並關閉 *application.properties* 檔案。</span><span class="sxs-lookup"><span data-stu-id="38519-181">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a><span data-ttu-id="38519-182">新增範例程式碼來實作基本的事件中樞功能</span><span class="sxs-lookup"><span data-stu-id="38519-182">Add sample code to implement basic event hub functionality</span></span>

<span data-ttu-id="38519-183">在本節中，您會建立必要的 Java 類別，以便將事件傳送至事件中樞。</span><span class="sxs-lookup"><span data-stu-id="38519-183">In this section, you create the necessary Java classes for sending events to your event hub.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="38519-184">修改主要應用程式類別</span><span class="sxs-lookup"><span data-stu-id="38519-184">Modify the main application class</span></span>

1. <span data-ttu-id="38519-185">在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：</span><span class="sxs-lookup"><span data-stu-id="38519-185">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\kafka\src\main\java\com\wingtiptoys\kafka\KafkaApplication.java`

   <span data-ttu-id="38519-186">-或-</span><span class="sxs-lookup"><span data-stu-id="38519-186">-or-</span></span>

   `/users/example/home/kafka/src/main/java/com/wingtiptoys/kafka/KafkaApplication.java`

1. <span data-ttu-id="38519-187">在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：</span><span class="sxs-lookup"><span data-stu-id="38519-187">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class KafkaApplication {
      public static void main(String[] args) {
         SpringApplication.run(KafkaApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="38519-188">儲存並關閉主要應用程式 Java 檔案。</span><span class="sxs-lookup"><span data-stu-id="38519-188">Save and close the main application Java file.</span></span>


### <a name="create-a-new-class-for-the-source-connector"></a><span data-ttu-id="38519-189">建立新的來源連接器類別</span><span class="sxs-lookup"><span data-stu-id="38519-189">Create a new class for the source connector</span></span>

1. <span data-ttu-id="38519-190">在應用程式的 package 目錄中，建立名為 KafkaSource.java 的新 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：</span><span class="sxs-lookup"><span data-stu-id="38519-190">Create a new Java file named *KafkaSource.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;

   @EnableBinding(Source.class)
   @RestController
   public class KafkaSource {
      @Autowired
      private Source source;

      @PostMapping("/messages")
      public String sendMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```

1. <span data-ttu-id="38519-191">儲存並關閉 KafkaSource.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="38519-191">Save and close the *KafkaSource.java* file.</span></span>

### <a name="create-a-new-class-for-the-sink-connector"></a><span data-ttu-id="38519-192">建立新的接收連接器類別</span><span class="sxs-lookup"><span data-stu-id="38519-192">Create a new class for the sink connector</span></span>

1. <span data-ttu-id="38519-193">在應用程式的 package 目錄中，建立名為 KafkaSink.java 的新 Java 檔案，然後在文字編輯器中開啟檔案，並加入下列幾行：</span><span class="sxs-lookup"><span data-stu-id="38519-193">Create a new Java file named *KafkaSink.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.kafka;

   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;

   @EnableBinding(Sink.class)
   public class KafkaSink {
      private static final Logger LOGGER = LoggerFactory.getLogger(KafkaSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message) {
         LOGGER.info("New message received: " + message);
      }
   }
   ```

1. <span data-ttu-id="38519-194">儲存並關閉 KafkaSink.java 檔案。</span><span class="sxs-lookup"><span data-stu-id="38519-194">Save and close the *KafkaSink.java* file.</span></span>

## <a name="build-and-test-your-application"></a><span data-ttu-id="38519-195">建置與測試您的應用程式</span><span class="sxs-lookup"><span data-stu-id="38519-195">Build and test your application</span></span>

1. <span data-ttu-id="38519-196">開啟命令提示字元，並將目錄切換到 *pom.xml* 檔案所在的資料夾；例如：</span><span class="sxs-lookup"><span data-stu-id="38519-196">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\kafka`

   <span data-ttu-id="38519-197">-或-</span><span class="sxs-lookup"><span data-stu-id="38519-197">-or-</span></span>

   `cd /users/example/home/kafka`

1. <span data-ttu-id="38519-198">使用 Maven 建置 Spring Boot 應用程式並加以執行；例如：</span><span class="sxs-lookup"><span data-stu-id="38519-198">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="38519-199">當您的應用程式執行時，可以使用cURL 來測試您的應用程式；例如：</span><span class="sxs-lookup"><span data-stu-id="38519-199">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   <span data-ttu-id="38519-200">您應該會看到 "hello" 已發佈至應用程式記錄檔。</span><span class="sxs-lookup"><span data-stu-id="38519-200">You should see "hello" posted to your application's logs.</span></span> <span data-ttu-id="38519-201">例如︰</span><span class="sxs-lookup"><span data-stu-id="38519-201">For example:</span></span>

   ```shell
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka version : 1.0.2
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka commitId : 2a121f7b1d402825
   [wingtiptoyshub.container-0-C-1] INFO com.wingtiptoys.kafka.KafkaSink - New message received: hello
   ```


> [!NOTE]
> 
> <span data-ttu-id="38519-202">基於測試目的，您可以修改 KafkaSource.java，使其包含簡單的 HTML 表單，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="38519-202">For testing purposes, you could modify your *KafkaSource.java* so that it contains a simple HTML form like the following example:</span></span>
> 
> ```java
> package com.wingtiptoys.kafka;
>    
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.cloud.stream.annotation.EnableBinding;
> import org.springframework.cloud.stream.messaging.Source;
> import org.springframework.messaging.support.GenericMessage;
> import org.springframework.web.bind.annotation.GetMapping;
> import org.springframework.web.bind.annotation.PostMapping;
> import org.springframework.web.bind.annotation.RequestBody;
> import org.springframework.web.bind.annotation.RequestParam;
> import org.springframework.web.bind.annotation.RestController;
> 
> @EnableBinding(Source.class)
> @RestController
> public class KafkaSource {
>   @Autowired
>   private Source source;
> 
>   @GetMapping("/")
>   public String sendForm() {
>     return "<html><body>" +
>       "<form action=\"/messages\" method=\"post\">" +
>       "<input type=\"text\" name=\"text\">" +
>       "<input type=\"submit\">" +
>       "</form></body><html>";
>     }
> 
>   @PostMapping("/messages")
>   public String sendMessage(@RequestBody String message) {
>     this.source.output().send(new GenericMessage<>(message));
>     return message;
>   }
> }
> ```
> 
> <span data-ttu-id="38519-203">這可讓您使用網頁瀏覽器來測試您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="38519-203">This will allow you to use a web browser to test your application:</span></span>
> 
> ![使用網頁瀏覽器測試應用程式][TB01]
> 
> <span data-ttu-id="38519-205">當您提交表單時，應用程式會顯示結果：</span><span class="sxs-lookup"><span data-stu-id="38519-205">When you submit the form, your application will display the results:</span></span>
> 
> ![網頁瀏覽器中的應用程式回應][TB02]
> 

## <a name="next-steps"></a><span data-ttu-id="38519-207">後續步驟</span><span class="sxs-lookup"><span data-stu-id="38519-207">Next steps</span></span>

<span data-ttu-id="38519-208">若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。</span><span class="sxs-lookup"><span data-stu-id="38519-208">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="38519-209">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="38519-209">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="38519-210">其他資源</span><span class="sxs-lookup"><span data-stu-id="38519-210">Additional Resources</span></span>

<span data-ttu-id="38519-211">若要深入了解適用於事件中樞 Stream Binder 和 Apache Kafka 的 Azure 支援，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="38519-211">For more information about Azure support for Event Hub Stream Binder and Apache Kafka, see the following articles:</span></span>

* [<span data-ttu-id="38519-212">Azure 事件中樞是什麼？</span><span class="sxs-lookup"><span data-stu-id="38519-212">What is Azure Event Hubs?</span></span>](/azure/event-hubs/event-hubs-about)

* [<span data-ttu-id="38519-213">適用於 Apache Kafka 的 Azure 事件中樞</span><span class="sxs-lookup"><span data-stu-id="38519-213">Azure Event Hubs for Apache Kafka</span></span>](/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)

* [<span data-ttu-id="38519-214">使用 Azure 入口網站來建立事件中樞命名空間和事件中樞</span><span class="sxs-lookup"><span data-stu-id="38519-214">Create an Event Hubs namespace and an event hub using the Azure portal</span></span>](/azure/event-hubs/event-hubs-create)

* [<span data-ttu-id="38519-215">建立已啟用 Apache kafka 的事件中樞</span><span class="sxs-lookup"><span data-stu-id="38519-215">Create Apache Kafka enabled event hubs</span></span>](/azure/event-hubs/event-hubs-create-kafka-enabled)

<span data-ttu-id="38519-216">如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱 [適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="38519-216">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="38519-217">**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="38519-217">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="38519-218">[Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。</span><span class="sxs-lookup"><span data-stu-id="38519-218">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="38519-219">為了協助開發人員開始使用 Spring Boot， <https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。</span><span class="sxs-lookup"><span data-stu-id="38519-219">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="38519-220">除了從基本的 Spring Boot 專案清單中進行選擇，**[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。</span><span class="sxs-lookup"><span data-stu-id="38519-220">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Apache Kafka]: http://kafka.apache.org
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[IMG01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-01.png
[IMG02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-02.png
[IMG03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-03.png
[IMG04]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-04.png
[IMG05]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-05.png
[IMG06]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-06.png
[IMG07]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-07.png
[IMG08]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-08.png

[SI01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-01.png
[SI02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-02.png
[SI03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-03.png

[TB01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-01.png
[TB02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-02.png
