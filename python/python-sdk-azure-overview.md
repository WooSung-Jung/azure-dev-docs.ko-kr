---
title: Python용 Azure SDK
description: 개발자가 Azure 서비스를 사용하는 경우 생산성을 높일 수 있도록 지원하는 Python용 Azure SDK의 특징과 기능에 대해 간략히 설명합니다.
ms.date: 10/30/2019
ms.topic: conceptual
ms.openlocfilehash: fb81b743de8332d18aeb815d1ed1efa09e6e3305
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466337"
---
# <a name="azure-sdk-for-python"></a>Python용 Azure SDK

Python용 Azure SDK는 Python 애플리케이션 코드에서 Azure 리소스를 사용하고 관리하는 작업을 간소화합니다. SDK는 Python 2.7 및 Python 3.5.3 이상을 지원합니다.

`pip install <library>`를 사용하여 개별 구성 요소 라이브러리를 설치하여 SDK를 설치합니다. [Python용 Azure SDK 패키지 인덱스](https://github.com/Azure/azure-sdk-for-python/blob/master/packages.md)에서 라이브러리 목록을 볼 수 있습니다.

라이브러리를 설치하고 프로젝트로 가져오는 방법에 대한 자세한 지침은 [SDK 설치](python-sdk-azure-install.md)를 참조하세요. 그런 다음, [SDK 시작](python-sdk-azure-get-started.yml)을 검토하여 인증을 설정하고 사용자 고유의 Azure 구독에 대해 샘플 코드를 실행합니다.

> [!TIP]
> SDK의 변경에 대한 자세한 내용은 [SDK 릴리스 정보](https://azure.github.io/azure-sdk/)를 참조하세요.

## <a name="connect-and-use-azure-services"></a>Azure 서비스 연결 및 사용

SDK에서 많은 *클라이언트 라이브러리*를 사용하면 기존 Azure 리소스에 연결하여 앱에서 이러한 리소스를 사용할 수 있습니다(예: 파일 업로드, 테이블 데이터 액세스 또는 다양한 Azure Cognitive Services 작업). SDK를 사용하면 서비스의 일반 REST API를 사용하는 대신 익숙한 Python 프로그래밍 패러다임을 사용하여 이러한 리소스를 사용할 수 있습니다.

예를 들어 Blob을 이전에 프로비저닝한 Azure Storage 계정에 업로드한다고 가정합니다. 첫 번째 단계는 적절한 라이브러리를 설치하는 것입니다.

```bash
pip install azure-storage-blob
```

다음으로, 코드에서 라이브러리를 가져옵니다.

```python
from azure.storage.blob import BlobClient
```

마지막으로, 라이브러리의 API를 사용하여 데이터에 연결하고 이를 업로드합니다. 다음 예제에서는 연결 문자열과 컨테이너 이름이 이미 스토리지 계정에 프로비저닝되어 있습니다. Blob 이름은 업로드된 데이터에 할당한 이름입니다.

```python
blob = BlobClient.from_connection_string("my_connection_string", container="mycontainer", blob="my_blob")

with open("./SampleSource.txt", "rb") as data:
    blob.upload_blob(data)
```

각 특정 코드 라이브러리를 사용하는 방법에 대한 자세한 내용은 [GitHub 리포지토리](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)의 라이브러리 프로젝트 폴더에 있는 *README.md* 또는 *README.rst* 파일을 참조하세요. 사용 가능한 [Azure 샘플](https://docs.microsoft.com/samples/browse/?languages=python)도 참조하세요.

또한 추가 코드 조각은 [참조 설명서](/python/api?view=azure-python)에서 찾을 수 있습니다.

### <a name="the-azure-core-library"></a>Azure 핵심 라이브러리

현재 다시 시도, 로깅, 전송 프로토콜, 인증 프로토콜 등과 같은 핵심 기능을 공유하도록 Python 클라이언트 라이브러리를 업데이트하고 있습니다. 이 공유 기능은 [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) 라이브러리에 포함되어 있습니다. 라이브러리 및 해당 지침에 대한 자세한 내용은 [Python 지침: 소개](https://azure.github.io/azure-sdk/python_introduction.html)를 참조하세요.

현재 핵심 라이브러리에서 작동하는 라이브러리는 다음과 같습니다.

- `azure-storage-blob`
- `azure-storage-queue`
- `azure-keyvault-keys`
- `azure-keyvault-secrets`

## <a name="manage-azure-resources"></a>Azure 리소스 관리

Python용 Azure SDK에는 Azure 리소스를 직접 만들고, 프로비저닝하고, 관리하는 데 도움이 되는 라이브러리도 많이 포함되어 있습니다. 이를 *관리 라이브러리*라고 합니다. 각 관리 라이브러리의 이름은 `azure-mgmt-<service name>`입니다. 관리 라이브러리를 사용하면 [Azure Portal](https://portal.azure.com) 또는 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)에서 수행할 수 있는 것과 동일한 작업을 수행하도록 Python 코드를 작성할 수 있습니다.

예를 들어 SQL Server 인스턴스를 만들려고 한다고 가정합니다. 먼저, 적절한 관리 라이브러리를 설치합니다.

```bash
pip install azure-mgmt-sql
```

Python 코드에서 라이브러리를 가져옵니다.

```python
from azure.mgmt.sql import SqlManagementClient

```

다음으로, 자격 증명과 Azure 구독 ID를 사용하여 관리 클라이언트 개체를 만듭니다.

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

마지막으로, 해당 클라이언트 개체를 사용하여 적절한 리소스 그룹 이름, 서버 이름, 위치 및 관리자 자격 증명을 사용하는 리소스를 만듭니다.

```python
server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

클라이언트 라이브러리와 마찬가지로 [GitHub 리포지토리](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)의 라이브러리 프로젝트 폴더에 있는 *README.md* 또는 *README.rst* 파일에서 각 관리 라이브러리를 사용하는 방법에 대한 자세한 내용을 확인할 수 있습니다.

추가 코드 조각은 [참조 설명서](/python/api?view=azure-python)에서도 찾을 수 있습니다. 

## <a name="get-help-and-give-feedback"></a>도움말 가져오기 및 피드백 제공

- [Python용 Azure SDK 설명서](https://aka.ms/python-docs)를 참조합니다.
- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python)의 커뮤니티에 질문을 게시합니다.
- [GitHub](https://github.com/Azure/azure-sdk-for-python/issues)에서 SDK에 대한 문제를 엽니다.
- [@AzureSDK](https://twitter.com/AzureSdk/)에 트윗합니다.
