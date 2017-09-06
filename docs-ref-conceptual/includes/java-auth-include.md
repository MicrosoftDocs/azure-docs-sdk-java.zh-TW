建立[驗證檔案](../java-sdk-azure-authenticate.md#mgmt-file)，並在命令列上使用檔案的完整路徑來匯出 `AZURE_AUTH_LOCATION` 環境變數。

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

驗證檔案可用來設定進入點 `Azure` 物件，以供管理程式庫用來定義、建立及設定 Azure 資源。

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

[深入了解](../java-sdk-azure-authenticate.md#mgmt-auth)使用適用於 Java 的 Azure 管理程式庫時的驗證選項。