---
title: "開始使用適用於 Java 的 Azure 程式庫"
description: "了解如何建立 Azure 雲端資源，並在您的 Java 應用程式中連線及使用。"
keywords: "Azure, Java, SDK, API, 驗證, 開始使用"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: get-started-article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: b1e10b79-f75e-4605-aecd-eed64873e2d3
ms.openlocfilehash: 69c75984f6274b5423614bd51c40957d3d509802
ms.sourcegitcommit: 1f6a80e067a8bdbbb4b2da2e2145fda73d5fe65a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2017
---
# <a name="get-started-with-cloud-development-using-the-azure-libraries-for-java"></a><span data-ttu-id="759f5-104">開始使用適用於 Java 的 Azure 程式庫進行雲端開發</span><span class="sxs-lookup"><span data-stu-id="759f5-104">Get started with cloud development using the Azure libraries for Java</span></span>

<span data-ttu-id="759f5-105">本指南會逐步引導您設定開發環境，以便在 Java 中進行 Azure 開發。</span><span class="sxs-lookup"><span data-stu-id="759f5-105">This guide walks you through setting up a development environment for Azure development in Java.</span></span> <span data-ttu-id="759f5-106">接著，您會建立一些 Azure 資源，並將它們連線以執行某些基本工作，例如上傳檔案或部署 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="759f5-106">You'll then create some Azure resources and connect them to to perform some basic tasks, like uploading a file or deploying a web application.</span></span> <span data-ttu-id="759f5-107">當您完成時，您就已做好準備可以開始在自有的 Java 應用程式中使用 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="759f5-107">When you're done, you'll be ready to start using Azure services in your own Java applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="759f5-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="759f5-108">Prerequisites</span></span>

- <span data-ttu-id="759f5-109">一個 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="759f5-109">An Azure account.</span></span> <span data-ttu-id="759f5-110">如果您沒有帳戶，請[取得免費試用帳戶](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="759f5-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/)</span></span>
- <span data-ttu-id="759f5-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) 或 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。</span><span class="sxs-lookup"><span data-stu-id="759f5-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) or [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>
- <span data-ttu-id="759f5-112">[Java 8](https://www.azul.com/downloads/zulu/) (包含在 Azure Cloud Shell 內)</span><span class="sxs-lookup"><span data-stu-id="759f5-112">[Java 8](https://www.azul.com/downloads/zulu/) (included in Azure Cloud Shell)</span></span>
- <span data-ttu-id="759f5-113">[Maven 3](http://maven.apache.org/download.cgi) (包含在 Azure Cloud Shell 內)</span><span class="sxs-lookup"><span data-stu-id="759f5-113">[Maven 3](http://maven.apache.org/download.cgi) (included in Azure Cloud Shell)</span></span>

## <a name="set-up-authentication"></a><span data-ttu-id="759f5-114">設定驗證</span><span class="sxs-lookup"><span data-stu-id="759f5-114">Set up authentication</span></span>

<span data-ttu-id="759f5-115">Java 應用程式必須有 Azure 訂用帳戶的讀取和建立權限，才能在此教學課程中執行程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="759f5-115">Your Java application needs read and create permissions in your Azure subscription to run the sample code in this tutorial.</span></span> <span data-ttu-id="759f5-116">請建立服務主體，並將應用程式設定為使用其認證來執行。</span><span class="sxs-lookup"><span data-stu-id="759f5-116">Create a service principal and configure your application to run with its credentials.</span></span> <span data-ttu-id="759f5-117">服務主體可讓您建立與身分識別相關聯的非互動式帳戶，而且對於此身分識別，您只賦予它應用程式執行時所需的權限。</span><span class="sxs-lookup"><span data-stu-id="759f5-117">Service principals provide a way to create a non-interactive account associated with your identity to which you grant only the privileges your app needs to run.</span></span>

<span data-ttu-id="759f5-118">[使用 Azure CLI 2.0 建立服務主體](/cli/azure/create-an-azure-service-principal-azure-cli)並擷取輸出。</span><span class="sxs-lookup"><span data-stu-id="759f5-118">[Create a service principal using the Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) and capture the output.</span></span> <span data-ttu-id="759f5-119">在 password 引數中提供[安全的密碼](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy)，而不是提供 `MY_SECURE_PASSWORD`。</span><span class="sxs-lookup"><span data-stu-id="759f5-119">Provide a [secure password](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) in the password argument instead of `MY_SECURE_PASSWORD`.</span></span> <span data-ttu-id="759f5-120">密碼必須介於 8 到 16 個字元，而且至少符合下列 4 個準則中的其中 3 個：</span><span class="sxs-lookup"><span data-stu-id="759f5-120">Your password must be 8 to 16 characters and match at least 3 out of the 4 following criteria:</span></span>

* <span data-ttu-id="759f5-121">包含小寫字元</span><span class="sxs-lookup"><span data-stu-id="759f5-121">Include lowercase characters</span></span>
* <span data-ttu-id="759f5-122">包含大寫字元</span><span class="sxs-lookup"><span data-stu-id="759f5-122">Include uppercase characters</span></span>
* <span data-ttu-id="759f5-123">包含數字</span><span class="sxs-lookup"><span data-stu-id="759f5-123">Include numbers</span></span>
* <span data-ttu-id="759f5-124">包含下列其中一個符號：@ # $ % ^ & * - _ !</span><span class="sxs-lookup"><span data-stu-id="759f5-124">Include one of the following symbols: @ # $ % ^ & * - _ !</span></span> <span data-ttu-id="759f5-125">+ = [ ] { } | \ : ‘ , .</span><span class="sxs-lookup"><span data-stu-id="759f5-125">+ = [ ] { } | \ : ‘ , .</span></span> <span data-ttu-id="759f5-126">?</span><span class="sxs-lookup"><span data-stu-id="759f5-126">?</span></span> <span data-ttu-id="759f5-127">/ \` ~ “ ( ) ;</span><span class="sxs-lookup"><span data-stu-id="759f5-127">/ \` ~ “ ( ) ;</span></span>


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

<span data-ttu-id="759f5-128">這會提供您下列格式的回覆：</span><span class="sxs-lookup"><span data-stu-id="759f5-128">Which gives you a reply in the following format:</span></span>

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

<span data-ttu-id="759f5-129">接下來，將下列內容複製到您系統上的文字檔：</span><span class="sxs-lookup"><span data-stu-id="759f5-129">Next, copy the following into a text file on your system:</span></span>

```text
# sample management library properties file
subscription=ssssssss-ssss-ssss-ssss-ssssssssssss
client=cccccccc-cccc-cccc-cccc-cccccccccccc
key=kkkkkkkkkkkkkkkk
tenant=tttttttt-tttt-tttt-tttt-tttttttttttt
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

<span data-ttu-id="759f5-130">將前四個值替換為以下內容：</span><span class="sxs-lookup"><span data-stu-id="759f5-130">Replace the top four values with the following:</span></span>

- <span data-ttu-id="759f5-131">subscription：使用在 Azure CLI 2.0 中透過 `az account show` 所得到的 id 值。</span><span class="sxs-lookup"><span data-stu-id="759f5-131">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="759f5-132">client：使用從服務主體輸出所擷取之輸出中得到的 appId 值。</span><span class="sxs-lookup"><span data-stu-id="759f5-132">client: use the *appId* value from the output taken from a service principal output.</span></span>
- <span data-ttu-id="759f5-133">key：使用從服務主體輸出所得到的 password 值。</span><span class="sxs-lookup"><span data-stu-id="759f5-133">key: use the *password* value from the service principal output.</span></span>
- <span data-ttu-id="759f5-134">tenant：使用從服務主體輸出所得到的 tenant 值。</span><span class="sxs-lookup"><span data-stu-id="759f5-134">tenant: use the *tenant* value from the service principal output.</span></span>

<span data-ttu-id="759f5-135">將此檔案儲存在系統上可供程式碼讀取且安全的位置。</span><span class="sxs-lookup"><span data-stu-id="759f5-135">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="759f5-136">您可以將此檔案用於日後撰寫的程式碼，因此建議您將其儲存在本文應用程式以外的位置。</span><span class="sxs-lookup"><span data-stu-id="759f5-136">You may use this file for future code so it's recommended to store it somewhere external to the application in this article.</span></span>

<span data-ttu-id="759f5-137">在殼層中使用驗證檔案的完整路徑來設定環境變數 `AZURE_AUTH_LOCATION`。</span><span class="sxs-lookup"><span data-stu-id="759f5-137">Set an environment variable `AZURE_AUTH_LOCATION` with the full path to the authentication file in your shell.</span></span>   

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="759f5-138">如果您是在 Windows 環境中工作，請在系統屬性中新增該變數。</span><span class="sxs-lookup"><span data-stu-id="759f5-138">If you're working in a windows environment, add the variable to your system properties.</span></span> <span data-ttu-id="759f5-139">開啟 PowerShell，並在以檔案路徑取代第二個變數之後，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="759f5-139">Open PowerShell and, after replacing the second variable with the path to your file, enter the following command:</span></span>

```powershell
[Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\<fullpath>\azureauth.properties", "Machine")
```

## <a name="create-a-new-maven-project"></a><span data-ttu-id="759f5-140">建立新的 Maven 專案</span><span class="sxs-lookup"><span data-stu-id="759f5-140">Create a new Maven project</span></span>

> [!NOTE]
> <span data-ttu-id="759f5-141">本指南使用 Maven 建置工具來建置及執行程式碼範例，但 Gradle 等其他建置工具也能與適用於 Java 的 Azure 程式庫搭配運作。</span><span class="sxs-lookup"><span data-stu-id="759f5-141">This guide uses Maven build tool to build and run the sample code, but other build tools such as Gradle also work with the Azure libraries for Java.</span></span> 

<span data-ttu-id="759f5-142">透過命令列在系統上的新目錄中建立 Maven 專案：</span><span class="sxs-lookup"><span data-stu-id="759f5-142">Create a Maven project from the command line in a new directory on your system:</span></span>

```
mkdir java-azure-test
cd java-azure-test
mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=AzureApp  \ 
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

<span data-ttu-id="759f5-143">這會在 `testAzureApp` 資料夾內建立基本的 Maven 專案。</span><span class="sxs-lookup"><span data-stu-id="759f5-143">This creates a basic Maven project under the `testAzureApp` folder.</span></span> <span data-ttu-id="759f5-144">在 `pom.xml` 專案中新增下列項目以匯入本教學課程之程式碼範例所使用的程式庫。</span><span class="sxs-lookup"><span data-stu-id="759f5-144">Add the following entries into the project `pom.xml` to import the libraries used in the sample code in this tutorial.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.0.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

<span data-ttu-id="759f5-145">在最上層 `project` 元素底下新增 `build` 項目以使用 [maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) 來執行這些範例：</span><span class="sxs-lookup"><span data-stu-id="759f5-145">Add a `build` entry under the top-level `project` element to use the [maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) to run the samples:</span></span>

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.AzureApp</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
 ```
   
## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="759f5-146">建立 Linux 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="759f5-146">Create a Linux virtual machine</span></span>

<span data-ttu-id="759f5-147">在專案的 `src/main/java` 目錄中建立名為 `AzureApp.java` 的新檔案，然後貼上以下程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="759f5-147">Create a new file named `AzureApp.java` in the project's `src/main/java` directory and paste in the following block of code.</span></span> <span data-ttu-id="759f5-148">使用機器的實際值來更新 `userName` 和 `sshKey` 變數。</span><span class="sxs-lookup"><span data-stu-id="759f5-148">Update the `userName` and `sshKey` variables with real values for your machine.</span></span> <span data-ttu-id="759f5-149">此程式碼會在美國東部 Azure 區域執行的 `sampleResourceGroup` 資源群組中，建立名為 `testLinuxVM` 的新 Linux VM。</span><span class="sxs-lookup"><span data-stu-id="759f5-149">The code creates a new Linux VM with name `testLinuxVM` in a resource group `sampleResourceGroup` running in the US East Azure region.</span></span>

```java
package com.fabrikam.AzureApp;

import com.microsoft.azure.management.Azure;
import com.microsoft.azure.management.compute.VirtualMachine;
import com.microsoft.azure.management.compute.KnownLinuxVirtualMachineImage;
import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
import com.microsoft.azure.management.appservice.WebApp;
import com.microsoft.azure.management.storage.StorageAccount;
import com.microsoft.azure.management.storage.SkuName;
import com.microsoft.azure.management.storage.StorageAccountKey;
import com.microsoft.azure.management.sql.SqlDatabase;
import com.microsoft.azure.management.sql.SqlServer;
import com.microsoft.azure.management.resources.fluentcore.arm.Region;
import com.microsoft.azure.management.resources.fluentcore.utils.SdkContext;

import com.microsoft.rest.LogLevel;

import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

import java.io.File;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class AzureApp {

    public static void main(String[] args) {

        final String userName = "YOUR_VM_USERNAME";
        final String sshKey = "YOUR_PUBLIC_SSH_KEY";

        try {

            // use the properties file with the service principal information to authenticate
            // change the name of the environment variable if you used a different name in the previous step
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));    
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();
           
            // create a Ubuntu virtual machine in a new resource group 
            VirtualMachine linuxVM = azure.virtualMachines().define("testLinuxVM")
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleVmResourceGroup")
                    .withNewPrimaryNetwork("10.0.0.0/24")
                    .withPrimaryPrivateIPAddressDynamic()
                    .withoutPrimaryPublicIPAddress()
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withUnmanagedDisks()
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();   

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```

<span data-ttu-id="759f5-150">從命令列執行範例：</span><span class="sxs-lookup"><span data-stu-id="759f5-150">Run the sample from the command line:</span></span>

```
mvn compile exec:java
```

<span data-ttu-id="759f5-151">您會在主控台中看到某些 REST 要求和回應，因為 SDK 會對 Azure REST API 進行基礎呼叫，以設定虛擬機器和其資源。</span><span class="sxs-lookup"><span data-stu-id="759f5-151">You'll see some REST requests and responses in the console as the SDK makes the underlying calls to the Azure REST API to configure the virtual machine and its resources.</span></span> <span data-ttu-id="759f5-152">當程式完成時，請使用 Azure CLI 2.0 在訂用帳戶中確認虛擬機器：</span><span class="sxs-lookup"><span data-stu-id="759f5-152">When the program finishes, verify the virtual machine in your subscription with the Azure CLI 2.0:</span></span>

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

<span data-ttu-id="759f5-153">在確認程式碼有效後，請使用 CLI 來刪除 VM 和其資源。</span><span class="sxs-lookup"><span data-stu-id="759f5-153">Once you've verified that the code worked, use the CLI to delete the VM and its resources.</span></span>

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a><span data-ttu-id="759f5-154">從 GitHub 存放庫部署 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="759f5-154">Deploy a web app from a GitHub repo</span></span>

<span data-ttu-id="759f5-155">將 `AzureApp.java` 中的主要方法替換為下列內容，並將 `appName` 變數更新為唯一值再執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="759f5-155">Replace the main method in `AzureApp.java` with the one below, updating the `appName` variable to a unique value before running the code.</span></span> <span data-ttu-id="759f5-156">此程式碼會將公用 GitHub 存放庫中 `master` 分支內的 Web 應用程式，部署至執行於免費定價層的新的 [Azure App Service Web 應用程式](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview)。</span><span class="sxs-lookup"><span data-stu-id="759f5-156">This code deploys a web application from the `master` branch in a public GitHub repo into a new [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) running in the free pricing tier.</span></span>

```java
    public static void main(String[] args) {
        try {

            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            final String appName = "YOUR_APP_NAME";

            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            WebApp app = azure.webApps().define(appName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleWebResourceGroup")
                    .withNewWindowsPlan(PricingTier.FREE_F1)
                    .defineSourceControl()
                    .withPublicGitRepository(
                            "https://github.com/Azure-Samples/app-service-web-java-get-started")
                    .withBranch("master")
                    .attach()
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

<span data-ttu-id="759f5-157">使用 Maven 如往常一樣地執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="759f5-157">Run the code as before using Maven:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="759f5-158">使用 CLI 開啟瀏覽器並讓其指向該應用程式：</span><span class="sxs-lookup"><span data-stu-id="759f5-158">Open a browser pointed to the application using the CLI:</span></span>

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

<span data-ttu-id="759f5-159">確認過部署後，請從訂用帳戶中移除 Web 應用程式和方案。</span><span class="sxs-lookup"><span data-stu-id="759f5-159">Remove the web app and plan from your subscription once you've verified the deployment.</span></span>

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a><span data-ttu-id="759f5-160">連線到 Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="759f5-160">Connect to an Azure SQL database</span></span>

<span data-ttu-id="759f5-161">使用下列程式碼取代 `AzureApp.java` 中目前的主要方法，並為 `dbPassword` 變數設定實際的值。</span><span class="sxs-lookup"><span data-stu-id="759f5-161">Replace the current main method in `AzureApp.java` with the code below, setting a real value for the `dbPassword` variable.</span></span>
<span data-ttu-id="759f5-162">此程式碼會使用允許遠端存取的防火牆規則來建立新的 SQL 資料庫，然後使用 SQL Database JBDC 驅動程式來與該資料庫連線。</span><span class="sxs-lookup"><span data-stu-id="759f5-162">This code creates a new SQL database with a firewall rule allowing remote access,  and then connects to it using the SQL Database JBDC driver.</span></span> 

```java

    public static void main(String args[])
    {
        // create the db using the management libraries
        try {
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            final String adminUser = SdkContext.randomResourceName("db",8);
            final String sqlServerName = SdkContext.randomResourceName("sql",10);
            final String sqlDbName = SdkContext.randomResourceName("dbname",8);
            final String dbPassword = "YOUR_PASSWORD_HERE";


            SqlServer sampleSQLServer = azure.sqlServers().define(sqlServerName)
                            .withRegion(Region.US_EAST)
                            .withNewResourceGroup("sampleSqlResourceGroup")
                            .withAdministratorLogin(adminUser)
                            .withAdministratorPassword(dbPassword)
                            .withNewFirewallRule("0.0.0.0","255.255.255.255")
                            .create();

            SqlDatabase sampleSQLDb = sampleSQLServer.databases().define(sqlDbName).create();

            // assemble the connection string to the database
            final String domain = sampleSQLServer.fullyQualifiedDomainName();
            String url = "jdbc:sqlserver://"+ domain + ":1433;" +
                    "database=" + sqlDbName +";" +
                    "user=" + adminUser+ "@" + sqlServerName + ";" +
                    "password=" + dbPassword + ";" +
                    "encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";

            // connect to the database, create a table and insert a entry into it
            Connection conn = DriverManager.getConnection(url);

            String createTable = "CREATE TABLE CLOUD ( name varchar(255), code int);";
            String insertValues = "INSERT INTO CLOUD (name, code ) VALUES ('Azure', 1);";
            String selectValues = "SELECT * FROM CLOUD";
            Statement createStatement = conn.createStatement();
            createStatement.execute(createTable);
            Statement insertStatement = conn.createStatement();
            insertStatement.execute(insertValues);
            Statement selectStatement = conn.createStatement();
            ResultSet rst = selectStatement.executeQuery(selectValues);

            while (rst.next()) {
                System.out.println(rst.getString(1) + " "
                        + rst.getString(2));
            }


        } catch (Exception e) {
            System.out.println(e.getMessage());
            System.out.println(e.getStackTrace().toString());
        }
    }
```
<span data-ttu-id="759f5-163">從命令列執行範例：</span><span class="sxs-lookup"><span data-stu-id="759f5-163">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="759f5-164">然後使用 CLI 清除資源：</span><span class="sxs-lookup"><span data-stu-id="759f5-164">Then clean up the resources using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a><span data-ttu-id="759f5-165">將 blob 寫入到新的儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="759f5-165">Write a blob into a new storage account</span></span>

<span data-ttu-id="759f5-166">使用下列程式碼取代 `AzureApp.java` 中目前的主要方法。</span><span class="sxs-lookup"><span data-stu-id="759f5-166">Replace the current main method in `AzureApp.java` with the code below.</span></span> <span data-ttu-id="759f5-167">此程式碼會建立 [Azure 儲存體帳戶](https://docs.microsoft.com/azure/storage/storage-introduction)，然後使用適用於 Java 的 Azure 儲存體程式庫在雲端建立新的文字檔。</span><span class="sxs-lookup"><span data-stu-id="759f5-167">This code creates an [Azure storage account](https://docs.microsoft.com/azure/storage/storage-introduction) and then uses the Azure Storage libraries for Java to create a new text file in the cloud.</span></span>

```java
public static void main(String[] args) {

    try {

        // use the properties file with the service principal information to authenticate
        // change the name of the environment variable if you used a different name in the previous step
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
                .withLogLevel(LogLevel.BASIC)
                .authenticate(credFile)
                .withDefaultSubscription();

        // create a new storage account
        String storageAccountName = SdkContext.randomResourceName("st",8);
        StorageAccount storage = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleStorageResourceGroup")
                    .create();

        // create a storage container to hold the file
        List<StorageAccountKey> keys = storage.getKeys();
        final String storageConnection = "DefaultEndpointsProtocol=https;"
                + "AccountName=" + storage.name()
                + ";AccountKey=" + keys.get(0).value()
                + ";EndpointSuffix=core.windows.net";

        CloudStorageAccount account = CloudStorageAccount.parse(storageConnection);
        CloudBlobClient serviceClient = account.createCloudBlobClient();

        // Container name must be lower case.
        CloudBlobContainer container = serviceClient.getContainerReference("helloazure");
        container.createIfNotExists();

        // Make the container public
        BlobContainerPermissions containerPermissions = new BlobContainerPermissions();
        containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
        container.uploadPermissions(containerPermissions);

        // write a blob to the container
        CloudBlockBlob blob = container.getBlockBlobReference("helloazure.txt");
        blob.uploadText("hello Azure");

    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }
    }
}
```

<span data-ttu-id="759f5-168">從命令列執行範例：</span><span class="sxs-lookup"><span data-stu-id="759f5-168">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="759f5-169">您可以透過 Azure 入口網站或 [Azure 儲存體總管](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) 來瀏覽儲存體帳戶中的 `helloazure.txt` 檔案。</span><span class="sxs-lookup"><span data-stu-id="759f5-169">You can browse for the `helloazure.txt` file in your storage account through the Azure portal or with [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).</span></span>

<span data-ttu-id="759f5-170">使用 CLI 清除儲存體帳戶：</span><span class="sxs-lookup"><span data-stu-id="759f5-170">Clean up the storage account using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a><span data-ttu-id="759f5-171">探索更多範例</span><span class="sxs-lookup"><span data-stu-id="759f5-171">Explore more samples</span></span>

<span data-ttu-id="759f5-172">若要深入了解如何使用適用於 Java 的 Azure 管理程式庫來管理資源和自動執行工作，請參閱我們針對[虛擬機器](java-sdk-azure-virtual-machine-samples.md)、[Web 應用程式](java-sdk-azure-web-apps-samples.md)和 [SQL 資料庫](java-sdk-azure-sql-database-samples.md)所提供的程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="759f5-172">To learn more about how to use the Azure management libraries for Java to manage resources and automate tasks, see our sample code for [virtual machines](java-sdk-azure-virtual-machine-samples.md), [web apps](java-sdk-azure-web-apps-samples.md) and [SQL database](java-sdk-azure-sql-database-samples.md).</span></span>

## <a name="reference-and-release-notes"></a><span data-ttu-id="759f5-173">參考資料和版本資訊</span><span class="sxs-lookup"><span data-stu-id="759f5-173">Reference and release notes</span></span>

<span data-ttu-id="759f5-174">我們針對所有套件提供了[參考資料](http://docs.microsoft.com/java/api)。</span><span class="sxs-lookup"><span data-stu-id="759f5-174">A [reference](http://docs.microsoft.com/java/api) is available for all packages.</span></span>

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="759f5-175">獲得協助及提供意見</span><span class="sxs-lookup"><span data-stu-id="759f5-175">Get help and give feedback</span></span>

<span data-ttu-id="759f5-176">您可以在 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java) 的社群中張貼問題。</span><span class="sxs-lookup"><span data-stu-id="759f5-176">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java).</span></span> <span data-ttu-id="759f5-177">若要針對適用於 Java 的 Azure 程式庫回報錯誤和建立問題，請至 [GitHub 專案](https://github.com/Azure/azure-sdk-for-java)。</span><span class="sxs-lookup"><span data-stu-id="759f5-177">Report bugs and open issues against the Azure libraries for Java on the [project GitHub](https://github.com/Azure/azure-sdk-for-java).</span></span>
