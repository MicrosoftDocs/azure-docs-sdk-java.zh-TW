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
ms.service: active-directory
ms.openlocfilehash: 4a610e2f0d9fb2e219c42155e2b0cb76fc78b09a
ms.sourcegitcommit: 5bfb3af5778167500a061157cbd0ad1cede8f90e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/04/2018
ms.locfileid: "37799696"
---
# <a name="azure-active-directory-libraries-for-java"></a><span data-ttu-id="0de04-104">適用於 Java 的 Azure Active Directory 程式庫</span><span class="sxs-lookup"><span data-stu-id="0de04-104">Azure Active Directory libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="0de04-105">概觀</span><span class="sxs-lookup"><span data-stu-id="0de04-105">Overview</span></span>

<span data-ttu-id="0de04-106">使用 [Azure Active Directory](/azure/active-directory/active-directory-whatis) 登入使用者並控制應用程式及 API 的存取。</span><span class="sxs-lookup"><span data-stu-id="0de04-106">Sign-on users and control access to applications and APIs with [Azure Active Directory](/azure/active-directory/active-directory-whatis).</span></span>

<span data-ttu-id="0de04-107">若要開始使用 Azure AD，請參閱[搭配 Azure AD 的 Java Web 應用程式登入和登出](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java)。</span><span class="sxs-lookup"><span data-stu-id="0de04-107">To get started with Azure AD, see [Java web app sign-in and sign-out with Azure AD](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="0de04-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="0de04-108">Client library</span></span>

<span data-ttu-id="0de04-109">使用 [適用於 Java 的 Azure Active Directory 驗證程式庫 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-java) 來設定 OAuth2、OpenID Connect 或 Active Directory Graph 驗證和 [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) 單一登入。</span><span class="sxs-lookup"><span data-stu-id="0de04-109">Configure OAuth2, OpenID Connect, or Active Directory Graph authentication and [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) single-sign on with the [Azure Active Directory authentication library (ADAL) for Java](https://github.com/AzureAD/azure-activedirectory-library-for-java).</span></span>

<span data-ttu-id="0de04-110">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="0de04-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="0de04-111">範例</span><span class="sxs-lookup"><span data-stu-id="0de04-111">Example</span></span>

<span data-ttu-id="0de04-112">使用 Azure Active Directory 的 [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) 來為 Active Directory 租用戶中的使用者擷取 JSON Web 權杖 (JWT)。</span><span class="sxs-lookup"><span data-stu-id="0de04-112">Retrieve a JSON Web Token (JWT) for a user in your an Active Directory tenant using Azure Active Directory's [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api).</span></span> <span data-ttu-id="0de04-113">此權杖接著可供用來對應用程式或 API 驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="0de04-113">This token can then be used to authenticate the user with an application or API.</span></span>

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

## <a name="management-api"></a><span data-ttu-id="0de04-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="0de04-114">Management API</span></span>

<span data-ttu-id="0de04-115">使用管理 API 設定[角色型存取控制](/azure/active-directory/role-based-access-control-what-is)並將身分識別 (例如使用者和[服務主體](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)) 指派給這些角色。</span><span class="sxs-lookup"><span data-stu-id="0de04-115">Configure [role based access control](/azure/active-directory/role-based-access-control-what-is) and assign identities (such as users and [service principals](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)) to those roles with the management API.</span></span> 

<span data-ttu-id="0de04-116">[新增相依性](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)至 Maven 的 `pom.xml` 檔案，以在專案中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="0de04-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-graph-rbac</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="0de04-117">範例</span><span class="sxs-lookup"><span data-stu-id="0de04-117">Example</span></span> 

<span data-ttu-id="0de04-118">建立新的服務主體，並對它指派參與者角色。</span><span class="sxs-lookup"><span data-stu-id="0de04-118">Create a new service principal and assign it the Contributor role.</span></span>

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
> [<span data-ttu-id="0de04-119">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="0de04-119">Explore the Management APIs</span></span>](/java/api/activedirectory/management)


## <a name="samples"></a><span data-ttu-id="0de04-120">範例</span><span class="sxs-lookup"><span data-stu-id="0de04-120">Samples</span></span>

<span data-ttu-id="0de04-121">[管理群組、使用者和角色](https://github.com/Azure-Samples/aad-java-manage-users-groups-and-roles)  </span><span class="sxs-lookup"><span data-stu-id="0de04-121">[Manage groups, users, and roles](https://github.com/Azure-Samples/aad-java-manage-users-groups-and-roles)  </span></span>  
<span data-ttu-id="0de04-122">[在 Java Web 應用程式中登入和登出使用者](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  </span><span class="sxs-lookup"><span data-stu-id="0de04-122">[Sign-in and sign-out users in a Java web app](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  </span></span>  
<span data-ttu-id="0de04-123">[使用命令列應用程式透過 Azure AD 存取 API](https://github.com/Azure-Samples/active-directory-java-native-headless) </span><span class="sxs-lookup"><span data-stu-id="0de04-123">[Access an API with Azure AD using a command line app](https://github.com/Azure-Samples/active-directory-java-native-headless) </span></span>  
[<span data-ttu-id="0de04-124">從 Java Web 應用程式呼叫作用中 AD Graph API</span><span class="sxs-lookup"><span data-stu-id="0de04-124">Call the Active AD Graph API from your Java web app</span></span>](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  

<span data-ttu-id="0de04-125">深入探索可在應用程式中使用的 [Azure AD Java 程式碼範例](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java)。</span><span class="sxs-lookup"><span data-stu-id="0de04-125">Explore more [sample Java code for Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java) you can use in your apps.</span></span>
