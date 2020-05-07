---
title: Python용 Azure 관리 라이브러리를 사용하여 앱 인증
description: Azure 관리 SDK 라이브러리를 사용하여 Azure 서비스로 Python 앱 인증
ms.date: 01/16/2020
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: e972d0159f97feddf4dd773d69b422634c3966c1
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441798"
---
# <a name="authenticate-by-using-the-azure-management-libraries-for-python"></a>Python용 Azure 관리 라이브러리를 사용하여 인증

이 문서에서는 Python SDK 관리 라이브러리를 사용하여 Azure AD(Azure Active Directory)에서 서비스 주체를 사용하여 애플리케이션을 인증하는 방법을 알아봅니다. 서비스 주체는 Azure AD에 등록된 애플리케이션의 ID이며, 서비스 주체를 통해 애플리케이션은 해당 권한에 따라 리소스에 액세스하거나 리소스를 수정할 수 있습니다.

애플리케이션을 등록하려면 먼저 조직에 적합한 테넌트로 Active Directory를 만들어야 합니다. 이 작업은 [Azure Active Directory에서 새 테넌트 만들기](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)의 지침에 따라 수행할 수 있습니다. Active Directory가 준비되면 [방법: 포털을 사용하여 리소스에 액세스할 수 있는 Azure AD 애플리케이션 및 서비스 주체 만들기](/azure/active-directory/develop/howto-create-service-principal-portal) 문서에 따라, 애플리케이션을 등록하고, [서비스 주체의 테넌트 및 애플리케이션(클라이언트) ID를 검색](/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)하고 Python 코드에서 인증하는 [애플리케이션 비밀](/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret)을 설정합니다.

이러한 값이 있으면 해당 자격 증명을 사용하여, Python SDK 라이브러리를 통해 여러 가지 방법으로 인증할 수 있습니다. 각 방법의 결과는 코드에서 다른 리소스에 액세스할 때 사용하는 SDK 클라이언트 개체입니다.

테넌트 ID, 클라이언트 ID 및 비밀을 [Azure KeyVault](/azure/key-vault/)에 저장하여 이러한 값이 시스템 또는 소스 제어의 어느 위치에도 존재하지 않도록 하는 것이 좋습니다. 필요할 때마다 값을 쉽게 검색할 수 있습니다.

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="authenticate-with-a-json-file"></a><a name="mgmt-auth-file"></a>JSON 파일로 인증

이 방법에서는 서비스 주체에 필요한 자격 증명이 포함된 JSON 파일을 만든 다음, 파일의 정보를 사용하여 SDK 클라이언트 개체를 만듭니다.

1. 다음 형식으로 JSON 파일을(*app_credentials.json*과 같이 원하는 이름으로) 만듭니다. 4개의 자리 표시자를 Azure 구독 ID, Azure AD 테넌트 ID, 애플리케이션(클라이언트) ID 및 비밀로 바꿉니다.

    ```json
    {
        "subscriptionId": "<azure_aubscription_id>",
        "tenantId": "<tenant_id>",
        "clientId": "<application_id>",
        "clientSecret": "<application_secret>",
        "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
        "resourceManagerEndpointUrl": "https://management.azure.com/",
        "activeDirectoryGraphResourceId": "https://graph.windows.net/",
        "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
        "galleryEndpointUrl": "https://gallery.azure.com/",
        "managementEndpointUrl": "https://management.core.windows.net/"
    }
    ```

    > [!TIP]
    > [az login](/cli/azure/reference-index#az-login) 명령 다음에 [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) 명령을 사용하여 Azure에 로그인하면 이미 있는 구독 ID로 자격 증명 파일을 검색할 수 있습니다.
    >
    > ```azurecli
    > az login
    > az ad sp create-for-rbac --sdk-auth > credentials.json
    > ```
    >
    > 그런 다음, 일반 사용 값을 사용하는 대신 특정 애플리케이션의 `tenantId`, `clientId` 및 `clientSecret` 값을 바꿀 수 있습니다.

1. 이 파일을 코드가 액세스할 수 있는 안전한 위치에 저장합니다.

1. [get_client_from_auth_file](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-auth-file-client-class--auth-path-none----kwargs-) 메서드를 사용하여 클라이언트 개체를 만들고 `<path_to_file>`을 JSON 파일의 경로로 바꿉니다.

    ```python
    from azure.common.client_factory import get_client_from_auth_file
    from azure.mgmt.compute import ComputeManagementClient

    client = get_client_from_auth_file(ComputeManagementClient, auth_path=<path_to_file>)
    ```

1. 또는 `AZURE_AUTH_LOCATION`이라는 환경 변수에 파일 경로를 저장하고 `auth_path` 인수를 생략할 수 있습니다.

## <a name="authenticate-with-a-json-dictionary"></a>JSON 사전으로 인증

이전 섹션에서 설명한대로 파일을 사용하는 대신, 변수에 필요한 JSON을 작성하고 [get_client_from_json_dict](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-json-dict-client-class--config-dict----kwargs-)를 호출할 수 있습니다. 이런 경우 테넌트 ID, 클라이언트 ID 및 비밀을 항상 [Azure KeyVault](/azure/key-vault/)와 같은 안전한 위치에 저장해야 합니다.

```python
   from azure.common.client_factory import get_client_from_auth_file
   from azure.mgmt.compute import ComputeManagementClient

    # Retrieve tenant_id, client_id, and client_secret from Azure KeyVault

   config_dict = {
       "subscriptionId": "bfc42d3a-65ca-11e7-95cf-ecb1d756380e",
        "tenantId": tenant_id,
       "clientId": client_id,
       "clientSecret": client_secret,
       "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
       "resourceManagerEndpointUrl": "https://management.azure.com/",
       "activeDirectoryGraphResourceId": "https://graph.windows.net/",
       "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
       "galleryEndpointUrl": "https://gallery.azure.com/",
       "managementEndpointUrl": "https://management.core.windows.net/"
   }
   client = get_client_from_json_dict(ComputeManagementClient, config_dict)
```

## <a name="authenticate-with-token-credentials"></a><a name="mgmt-auth-token"></a>토큰 자격 증명을 사용하여 인증

[Azure KeyVault](/azure/key-vault/)와 같은 안전한 스토리지에서 자격 증명을 검색하는 경우, 먼저 [ServicePrincipalCredentials] 개체를 만든 다음, 해당 자격 증명과 구독 ID를 사용하여 클라이언트 인스턴스를 만듭니다.

```python
from azure.mgmt.compute import ComputeManagementClient
from azure.common.credentials import ServicePrincipalCredentials

# Retrieve credentials from secure storage. Never hard-code credentials into code.

credentials = ServicePrincipalCredentials(tenant = <tenant_id>,
    client_id = <client_id>, secret = <secret>)

client = ComputeManagementClient(credentials, <subscription_id>)
```

컨트롤이 더 필요하면 [Python용 Azure ADAL(Active Directory 인증 라이브러리)](https://github.com/AzureAD/azure-activedirectory-library-for-python)와 SDK ADAL 래퍼를 사용합니다.

```python
from azure.mgmt.compute import ComputeManagementClient
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

# Retrieve credentials from secure storage. Never hard-code credentials into code.

LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)

credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    RESOURCE, <client_id>, <secret>)

client = ComputeManagementClient(credentials, <subscription_id>)
```

> [!NOTE]
> Azure 소버린 클라우드를 사용하는 경우 관리 클라이언트를 만들 때 적절한 기본 URL을 지정합니다(`msrestazure.azure_cloud`의 상수를 사용).
>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```

### <a name="authenticate-with-token-credentials-deprecated"></a><a name="mgmt-auth-legacy"></a>토큰 자격 증명으로 인증(사용되지 않음)

[Python용 Azure ADAL(Active Directory 인증 라이브러리)](https://github.com/AzureAD/azure-activedirectory-library-for-python)을 사용할 수 있기 전에는 `UserPassCredentials` 클래스를 사용했습니다. 이 클래스는 더 이상 사용되지 않으며 2단계 인증을 지원하지 않으므로 더 이상 사용해서는 안됩니다.

```python
from azure.common.credentials import UserPassCredentials

# DEPRECATED - legacy purposes only - use ADAL instead
credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```

## <a name="authenticate-with-azure-managed-identities"></a><a name="mgmt-auth-msi"></a>Azure 관리 ID로 인증

Azure 관리 ID는 Azure의 리소스가 특정 자격 증명을 사용하지 않고 인증하는 간단한 방법입니다.

관리 ID를 사용하려면 Azure 함수 또는 가상 머신과 같은 다른 Azure 리소스에서 Azure에 연결해야 합니다. 리소스의 관리 ID를 구성하는 방법을 알아보려면 [Azure 리소스의 관리 ID 구성](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm) 및 [Azure 리소스의 관리 ID를 사용하는 방법](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)을 참조하세요.

```python
from msrestazure.azure_active_directory import MSIAuthentication
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient

# Create MSI Authentication
credentials = MSIAuthentication()

# Create a Subscription Client
subscription_client = SubscriptionClient(credentials)
subscription = next(subscription_client.subscriptions.list())
subscription_id = subscription.subscription_id

# Create a Resource Management client
client = ResourceManagementClient(credentials, subscription_id)

# List resource groups as an example. The only limit is what role and policy are assigned to this MSI token.
for resource_group in client.resource_groups.list():
    print(resource_group.name)
```

## <a name="cli-based-authentication-development-purposes-only"></a><a name="mgmt-auth-cli"></a>CLI 기반 인증(개발 전용)

SDK는 `az login`을 실행한 후 Azure CLI 활성 구독을 사용하여 클라이언트를 만들 수 있습니다. SDK는 기본 구독 ID를 사용하거나 [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)를 사용하여 구독을 설정할 수 있습니다.

이 옵션은 개발 목적으로만 사용해야 합니다.

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
