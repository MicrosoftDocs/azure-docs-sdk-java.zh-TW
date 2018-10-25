---
title: 適用於 Java 的 Azure Active Directory 程式庫
description: Java 用戶端和管理程式庫 Azure Active Directory 的參考文件
keywords: Azure, Java, SDK, API, SQL, 驗證, AAD, Active Directory , Graph, OAuth 2.0
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.openlocfilehash: 6147fc959727b53cd796534cefc927145b52d81d
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799834"
---
# <a name="azure-active-directory-libraries-for-java"></a>適用於 Java 的 Azure Active Directory 程式庫

## <a name="overview"></a>概觀

使用 [Azure Active Directory](/azure/active-directory/active-directory-whatis) 登入使用者並控制應用程式及 API 的存取。

若要開始使用 Azure AD，請參閱[搭配 Azure AD 的 Java Web 應用程式登入和登出](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java)。

## <a name="client-library"></a>用戶端程式庫

使用 [適用於 Java 的 Azure Active Directory 驗證程式庫 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-java) 來設定 OAuth2、OpenID Connect 或 Active Directory Graph 驗證和 [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) 單一登入。

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="example"></a>範例

使用 Azure Active Directory 的 [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) 來為 Active Directory 租用戶中的使用者擷取 JSON Web 權杖 (JWT)。 此權杖接著可供用來對應用程式或 API 驗證使用者。

```java
ExecutorService service = Executors.newFixedThreadPool(1);
AuthenticationContext context = new AuthenticationContext(AUTHORITY, false, service);
Future<AuthenticationResult> future = context.acquireToken(
    "https://graph.windows.net", YOUR_TENANT_ID, username, password,
    null);
AuthenticationResult result = future.get();
System.out.println("Access Token - " + result.getAccessToken());
System.out.println("Refresh Token - " + result.getRefreshToken());
System.out.println("ID Token - " + result.getIdToken());
```

## <a name="management-api"></a>管理 API

使用管理 API 設定[角色型存取控制](/azure/active-directory/role-based-access-control-what-is)並將身分識別 (例如使用者和[服務主體](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)) 指派給這些角色。 

[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-graph-rbac</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a>範例 

建立新的服務主體，並對它指派參與者角色。

```java
ServicePrincipal sp = Azure.servicePrincipals().define(spName)
    .withNewApplication("http://" + spName)
    .create();
RoleAssignment roleAssignment2 = authenticated.roleAssignments()
    .define("contribRoleAssignment")
    .forServicePrincipal(sp)
    .withBuiltInRole(BuiltInRole.CONTRIBUTOR)
    .withSubscriptionScope("862f67bc-d3ae-4243-bec7-3da6dca77717")
    .create();
```

> [!div class="nextstepaction"]
> [探索管理 API](/java/api/overview/azure/activedirectory/management)


## <a name="samples"></a>範例

[管理群組、使用者和角色](https://github.com/Azure-Samples/aad-java-manage-users-groups-and-roles)    
[在 Java Web 應用程式中登入和登出使用者](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)    
[使用命令列應用程式透過 Azure AD 存取 API](https://github.com/Azure-Samples/active-directory-java-native-headless)   
[從 Java Web 應用程式呼叫作用中 AD Graph API](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  

深入探索可在應用程式中使用的 [Azure AD Java 程式碼範例](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java)。
