<span data-ttu-id="8637a-101">建立[驗證檔案](../java-sdk-azure-authenticate.md#mgmt-file)，並在命令列上使用檔案的完整路徑來匯出 `AZURE_AUTH_LOCATION` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="8637a-101">Create an [authentication file](../java-sdk-azure-authenticate.md#mgmt-file) and export an environment variable `AZURE_AUTH_LOCATION` on the command line with the full path to the file.</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

<span data-ttu-id="8637a-102">驗證檔案可用來設定進入點 `Azure` 物件，以供管理程式庫用來定義、建立及設定 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="8637a-102">The authentication file is used to configure the entry point `Azure` object used by the management libraries to define, create, and configure Azure resources.</span></span>

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

<span data-ttu-id="8637a-103">[深入了解](../java-sdk-azure-authenticate.md#mgmt-auth)使用適用於 Java 的 Azure 管理程式庫時的驗證選項。</span><span class="sxs-lookup"><span data-stu-id="8637a-103">[Learn more](../java-sdk-azure-authenticate.md#mgmt-auth) about authentication options when using the Azure management libraries for Java.</span></span>