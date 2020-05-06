---
title: Azure CLI에서 Azure 서비스 주체 사용
description: Azure CLI를 사용하여 서비스 주체를 만들고 사용하는 방법을 알아봅니다.
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 02/15/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c18adbee84fd3e5c73367b07bbd0b03ac61008cd
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030735"
---
# <a name="create-an-azure-service-principal-with-the-azure-cli"></a>Azure CLI를 사용하여 Azure 서비스 주체 만들기

Azure 서비스를 사용하도록 자동화된 도구에는 항상 제한된 권한이 있어야 합니다. Azure는 애플리케이션에서 모든 권한이 있는 사용자로 로그인하도록 하는 대신 서비스 주체를 제공합니다.

Azure 서비스 주체는 애플리케이션, 호스팅된 서비스 및 자동화된 도구에서 사용하여 Azure 리소스에 액세스하기 위해 만든 ID입니다. 이 액세스는 서비스 주체에 할당된 역할로 제한되므로 액세스할 수 있는 리소스와 해당 수준을 제어할 수 있습니다. 보안상의 이유로 사용자 ID를 통해 로그인할 수 있게 하는 대신, 항상 자동화된 도구에서 서비스 주체를 사용하는 것이 좋습니다.

이 문서에서는 Azure CLI를 사용하여 서비스 주체를 만들고, 정보를 가져오고, 다시 설정하는 단계를 보여 줍니다.

## <a name="create-a-service-principal"></a>서비스 주체 만들기

[az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) 명령을 사용하여 서비스 주체를 만듭니다. 서비스 주체를 만드는 경우 사용하는 로그인 인증 유형을 선택합니다.

> [!NOTE]
>
> 계정에 서비스 주체를 만들 수 있는 권한이 없는 경우 `az ad sp create-for-rbac`에서 "권한이 부족하여 작업을 완료할 수 없습니다."라는 오류 메시지를 반환합니다. 서비스 주체를 만들려면 Azure Active Directory 관리자에게 문의하십시오.

서비스 주체에 사용할 수 있는 인증 유형에는 암호 기반 인증 및 인증서 기반 인증의 두 가지 유형이 있습니다.

### <a name="password-based-authentication"></a>암호 기반 인증

인증 매개 변수가 없으면 임의로 만든 암호를 통한 암호 기반 인증이 사용됩니다.

  ```azurecli-interactive
  az ad sp create-for-rbac --name ServicePrincipalName
  ```

> [!IMPORTANT]
> Azure CLI 2.0.68부터 약한 암호를 실수로 사용하는 것을 방지하기 위해 사용자 정의 암호로 서비스 주체를 만드는 `--password` 매개 변수가 __더 이상 지원되지 않습니다__.

암호 인증을 사용하는 서비스 주체의 출력에는 `password` 키가 포함되어 있습니다. 이 값은 검색할 수 없으므로 __복사해야 합니다__. 암호를 잊어버린 경우 [서비스 주체 자격 증명을 다시 설정](#reset-credentials)하세요.

`appId` 및 `tenant` 키는 `az ad sp create-for-rbac` 출력에 표시되며 서비스 주체 인증에 사용됩니다.
해당 값은 기록하지만 언제든지 [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list)를 사용하여 검색할 수 있습니다.

### <a name="certificate-based-authentication"></a>인증서 기반 인증

인증서 기반 인증의 경우 `--cert` 인수를 사용합니다. 이 인수를 사용하려면 기존 인증서를 보유하고 있어야 합니다. 이 서비스 주체를 사용하는 모든 도구에서 인증서의 프라이빗 키에 액세스할 수 있는지 확인합니다. 인증서는 PEM, CER 또는 DER과 같은 ASCII 형식이어야 합니다. 인증서를 문자열로 전달하거나 `@path` 형식을 사용하여 파일에서 인증서를 로드합니다.

> [!NOTE]
> PEM 파일을 사용하는 경우 **인증서**는 파일 내의 **프라이빗 키**에 추가해야 합니다.

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert "-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----"
```

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert @/path/to/cert.pem
```

Azure Key Vault에서 인증서를 사용하려면 `--keyvault` 인수를 추가할 수 있습니다. 이 경우 `--cert` 값은 인증서의 이름입니다.

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert CertName --keyvault VaultName
```

인증을 위해 _자체 서명된_ 인증서를 만들려면 `--create-cert` 인수를 사용합니다.

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --create-cert
```

콘솔 출력:

```
Creating a role assignment under the scope of "/subscriptions/myId"
Please copy C:\myPath\myNewFile.pem to a safe place.
When you run 'az login', provide the file path in the --password argument
{
  "appId": "myAppId",
  "displayName": "myDisplayName",
  "fileWithCertAndPrivateKey": "C:\\myPath\\myNewFile.pem",
  "name": "http://myName",
  "password": null,
  "tenant": "myTenantId"
}
```

새 PEM 파일의 콘텐츠:
```
-----BEGIN PRIVATE KEY-----
myPrivateKeyValue
-----END PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
myCertificateValue
-----END CERTIFICATE-----
```

> [!NOTE]
> `az ad sp create-for-rbac --create-cert` 명령은 서비스 사용자 및 PEM 파일을 만듭니다. PEM 파일에는 올바른 형식의 **프라이빗 키** 및 **인증서**가 포함됩니다.

`--keyvault` 인수를 추가하면 Azure Key Vault에 인증서를 저장할 수 있습니다. `--keyvault`를 사용하는 경우 `--cert` 인수가 __필요합니다__.

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --create-cert --cert CertName --keyvault VaultName
```

인증서를 Key Vault에 저장하지 않으면 출력에 `fileWithCertAndPrivateKey` 키가 포함됩니다. 이 키의 값은 생성된 인증서가 저장되는 위치를 알려줍니다.
인증서를 안전한 위치에 복사하거나 이 서비스 주체를 사용하여 로그인할 수 없는지 __확인해야 합니다__.

Key Vault에 저장된 인증서의 경우 [az keyvault secret show](/cli/azure/keyvault/secret#az-keyvault-secret-show)를 사용하여 인증서의 프라이빗 키를 검색합니다. Key Vault에서 인증서의 비밀 이름은 인증서 이름과 동일합니다. 인증서의 프라이빗 키에 액세스할 수 없는 경우 [서비스 주체 자격 증명](#reset-credentials)을 다시 설정하세요.

`appId` 및 `tenant` 키는 `az ad sp create-for-rbac` 출력에 표시되며 서비스 주체 인증에 사용됩니다.
해당 값은 기록하지만 언제든지 [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list)를 사용하여 검색할 수 있습니다.

## <a name="get-an-existing-service-principal"></a>기존 서비스 주체 가져오기

테넌트의 서비스 주체 목록은 [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list)를 사용하여 검색할 수 있습니다. 이 명령은 기본적으로 테넌트에 대한 처음 100개의 서비스 주체를 반환합니다. 테넌트의 서비스 주체를 모두 가져오려면 `--all` 인수를 사용합니다. 이 목록을 가져오는 데 시간이 오래 걸릴 수 있으므로 다음 인수 중 하나를 사용하여 목록을 필터링하는 것이 좋습니다.

* `--display-name`은 제공된 이름과 일치하는 _접두사_가 있는 서비스 주체를 요청합니다. 서비스 주체의 표시 이름은 만드는 중에 `--name` 매개 변수로 설정된 값입니다. 서비스 주체 만드는 중에 `--name`을 설정하지 않은 경우 이름 접두사는 `azure-cli-`입니다.
* `--spn`은 정확한 서비스 주체 이름 일치 기준으로 필터링합니다. 서비스 주체 이름은 항상 `https://`로 시작합니다.
  `--name`에 사용한 값이 URI가 아닌 경우 이 값은 `https://`이며 바로 뒤에 표시 이름이 나옵니다.
* `--show-mine`은 로그인한 사용자가 만든 서비스 주체만 요청합니다.
* `--filter`는 OData 필터를 사용하며 _서버 쪽_ 필터링을 수행합니다. 이 메서드는 CLI의 `--query` 인수를 사용하는 클라이언트 쪽 필터링보다 먼저 추천됩니다. OData 필터에 대한 자세한 내용은 [필터에 대한 OData 식 구문](/rest/api/searchservice/odata-expression-syntax-for-azure-search)을 참조하세요.

서비스 주체 개체에 대해 반환되는 정보는 자세한 정보입니다. 로그인하는 데 필요한 정보만 가져오려면 `[].{id:appId, tenant:appOwnerTenantId}` 쿼리 문자열을 사용합니다. 예를 들어 현재 로그인한 사용자가 만든 모든 서비스 주체에 대한 로그인 정보를 가져오려면 다음과 같습니다.

```azurecli-interactive
az ad sp list --show-mine --query "[].{id:appId, tenant:appOwnerTenantId}"
```

> [!IMPORTANT]
>
> `az ad sp list` 또는 [az ad sp show](/cli/azure/ad/sp#az-ad-sp-show)는 사용자와 테넌트를 가져오지만, 인증 비밀 _또는_ 인증 방법은 가져오지 않습니다.
> Key Vault에 있는 인증서에 대한 비밀은 [az keyvault secret show](/cli/azure/keyvault/secret#az-keyvault-secret-show)를 사용하여 검색할 수 있지만, 다른 비밀은 기본적으로 저장되지 않습니다.
> 인증 방법 또는 비밀을 잊어버린 경우 [서비스 주체 자격 증명을 다시 설정](#reset-credentials)하세요.

## <a name="manage-service-principal-roles"></a>서비스 주체 역할 관리

역할 할당을 관리하기 위해 Azure CLI에는 다음과 같은 명령이 있습니다.

* [az 역할 할당 나열](/cli/azure/role/assignment#az-role-assignment-list)
* [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create)
* [az 역할 할당 삭제](/cli/azure/role/assignment#az-role-assignment-delete)

서비스 주체의 기본 역할은 **참가자**입니다. 이 역할에는 Azure 계정에서 읽고 쓸 수 있는 모든 권한이 있습니다. **Reader**(읽기 권한자) 역할은 읽기 전용 액세스 권한으로 더 제한적입니다.  RBAC(역할 기반 액세스 제어)와 역할에 대한 자세한 내용은 [RBAC: 기본 제공 역할](/azure/active-directory/role-based-access-built-in-roles)을 참조하세요.

다음 예제에서는 **Reader** 역할을 추가하고 **Contributor**(기여자) 역할을 제거합니다.

```azurecli-interactive
az role assignment create --assignee APP_ID --role Reader
az role assignment delete --assignee APP_ID --role Contributor
```

> [!NOTE]
> 계정에 역할을 할당할 수 있는 권한이 없으면 계정에 "'Microsoft.Authorization/roleAssignments/write' 작업을 수행할 수 있는 권한이 없습니다."라는 오류 메시지가 표시됩니다. 역할을 관리하려면 Azure Active Directory 관리자에게 문의하십시오.

역할을 추가해도 이전에 할당된 권한은 _제한되지 않습니다_. 서비스 주체 권한을 제한할 때는 __Contributor__ 역할을 제거해야 합니다.

변경 내용은 할당된 역할을 나열하여 확인할 수 있습니다.

```azurecli-interactive
az role assignment list --assignee APP_ID
```

## <a name="sign-in-using-a-service-principal"></a>서비스 주체를 사용하여 로그인

로그인하여 새 서비스 주체의 자격 증명과 권한을 테스트합니다. 서비스 주체를 사용하여 로그인하려면 `appId`, `tenant` 및 자격 증명이 필요합니다.

암호를 사용하는 서비스 주체로 로그인하려면 다음과 같습니다.

```azurecli-interactive
az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID
```

인증서를 사용하여 로그인하려면 해당 인증서를 ASCII 형식의 PEM 또는 DER 파일로 로컬에서 사용할 수 있어야 합니다. PEM 파일을 사용하는 경우 **프라이빗 키**와 **인증서**는 파일 내에서 추가해야 합니다.

```azurecli-interactive
az login --service-principal --username APP_ID --tenant TENANT_ID --password /path/to/cert
```

서비스 주체를 사용하여 로그인하는 방법에 대한 자세한 내용은 [Azure CLI로 로그인](authenticate-azure-cli.md)을 참조하세요.

## <a name="reset-credentials"></a>자격 증명 다시 설정

서비스 주체에 대한 자격 증명을 잊어버린 경우 [az ad sp credential reset](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset)을 사용합니다. reset 명령은 `az ad sp create-for-rbac`와 동일한 인수를 사용합니다.

```azurecli-interactive
az ad sp credential reset --name APP_ID
```
