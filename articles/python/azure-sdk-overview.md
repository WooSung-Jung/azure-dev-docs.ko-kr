---
title: Python용 Azure SDK
description: 개발자가 Azure 리소스를 프로비저닝, 사용 및 관리하는 경우 생산성을 높이는 데 도움이 되는 Python용 Azure SDK의 특징과 기능에 대해 간략히 설명합니다.
ms.date: 03/17/2020
ms.topic: conceptual
ms.openlocfilehash: 3d24a512420610f37285a03fe6a39d81e97510ee
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82026116"
---
# <a name="azure-sdk-for-python"></a>Python용 Azure SDK

Python용 오픈 소스 Azure SDK는 Python 애플리케이션 코드에서 Azure 리소스를 프로비저닝하고, 사용하고, 관리하는 작업을 간소화합니다. SDK는 Python 2.7 및 Python 3.5.3 이상을 지원합니다.

SDK는 각각 `pip install <library>`를 사용하여 설치하는 개별 구성 요소 라이브러리로 구성됩니다. 자세한 지침은 [SDK 설치](azure-sdk-install.md)에 있습니다. [Python용 Azure SDK 설명서](https://azure.github.io/azure-sdk-for-python/)는 라이브러리의 이름을 제공합니다.

또한 [Python용 Azure SDK 시작](azure-sdk-get-started.yml) 연습을 수행하여 라이브러리를 직접 경험할 수도 있습니다.

> [!TIP]
> SDK의 변경에 대한 자세한 내용은 [SDK 릴리스 정보](https://azure.github.io/azure-sdk/)를 참조하세요.

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="management-and-client-libraries"></a>관리 및 클라이언트 라이브러리

Python용 Azure SDK에는 다음 두 가지 유형의 라이브러리가 있습니다.

- *관리* 라이브러리를 사용하면 [Azure Portal](https://portal.azure.com) 또는 명령줄 도구(예: [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 및 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/))를 통해 Azure 리소스를 프로비저닝하고 관리할 수 있습니다. 관리 라이브러리는 구성 및 배포 스크립트에서 가장 일반적으로 사용됩니다. (Azure CLI는 실제로 Python 관리 라이브러리를 사용하여 작성되었습니다.)

- *클라이언트* 라이브러리는 자연스러운 Python 관용구를 사용하여 애플리케이션 코드에서 이미 프로비저닝된 서비스와 상호 작용할 수 있는 방법을 제공합니다. 클라이언트 라이브러리는 내부적으로 Azure의 REST API를 사용하지만, SDK를 사용하면 REST 세부 정보에 대해 걱정할 필요가 없습니다.

다음 섹션에서는 각 범주에 대한 자세한 정보와 예제를 제공합니다.

## <a name="manage-azure-resources-with-management-libraries"></a>관리 라이브러리를 사용하여 Azure 리소스 관리

각각 `azure-mgmt-<service name>`이라는 Python용 Azure SDK의 관리 라이브러리는 Azure 리소스를 만들고, 프로비저닝하고, 관리하는 데 도움이 됩니다.

예를 들어 SQL Server 인스턴스를 만들려고 한다고 가정합니다. 먼저, 적절한 관리 라이브러리를 설치합니다.

```bash
pip install azure-mgmt-sql
```

Python 코드에서 라이브러리를 가져옵니다.

```python
from azure.mgmt.sql import SqlManagementClient
```

다음으로, 자격 증명([SDK를 사용하여 인증](azure-sdk-authenticate.md) 참조) 및 Azure 구독 ID를 사용하여 관리 클라이언트 개체를 만듭니다.

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

마지막으로, 해당 관리 클라이언트 개체를 사용하여 적절한 리소스 그룹 이름, 서버 이름, 위치 및 관리자 자격 증명을 사용하는 리소스를 만듭니다.

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

각 관리 라이브러리를 사용하는 방법에 대한 자세한 내용은 SDK의 [GitHub 리포지토리](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)에서 라이브러리의 프로젝트 폴더에 있는 *README.md* 또는 *README.rst* 파일을 참조하세요. 추가 코드 조각은 [참조 설명서](/python/api?view=azure-python) 및 [Azure 샘플](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)에서 찾을 수도 있습니다.

## <a name="connect-and-use-azure-services-with-client-libraries"></a>클라이언트 라이브러리를 사용하여 Azure 서비스 연결 및 사용

Azure SDK의 *클라이언트 라이브러리*를 사용하면 기존 Azure 리소스에 연결하여 앱에서 이러한 리소스를 사용할 수 있습니다(예: 파일 업로드, 테이블 데이터 액세스 또는 다양한 Azure Cognitive Services 작업). SDK를 사용하면 서비스의 일반 REST API를 사용하는 대신 익숙한 Python 프로그래밍 패러다임을 사용하여 이러한 리소스를 사용할 수 있습니다. (REST API가 없는 서비스는 클라이언트 라이브러리로 표시되지 않습니다.)

예를 들어 웹앱을 Azure 앱 서비스에 배포하고 파일을 Azure Storage 계정에 업로드하는 기능을 추가하려고 한다고 가정합니다. 첫 번째 단계는 적절한 라이브러리를 설치하는 것입니다.

```bash
pip install azure-storage-blob
```

다음으로, 코드에서 라이브러리를 가져옵니다.

```python
from azure.storage.blob import BlobClient
```

마지막으로, 라이브러리의 API를 사용하여 데이터에 연결하고 이를 업로드합니다. 다음 예제에서는 연결 문자열과 컨테이너 이름이 이미 스토리지 계정에 프로비저닝되어 있습니다. Blob 이름은 업로드된 데이터에 할당한 이름입니다.

```python
blob = BlobClient.from_connection_string("my_connection_string", container_name="mycontainer", blob_name="my_blob")

with open("./SampleSource.txt", "rb") as data:
    blob.upload_blob(data)
```

각 관리 라이브러리를 사용하는 방법에 대한 자세한 내용은 SDK의 [GitHub 리포지토리](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)에서 라이브러리의 프로젝트 폴더에 있는 *README.md* 또는 *README.rst* 파일을 참조하세요. 추가 코드 조각은 [참조 설명서](/python/api?view=azure-python) 및 [Azure 샘플](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)에서 찾을 수도 있습니다.

### <a name="the-azure-core-library"></a>Azure 핵심 라이브러리

현재 인증 프로토콜, 로깅, 추적, 전송 프로토콜, 버퍼링된 응답 및 재시도와 같은 일반적인 클라우드 패턴을 공유하도록 Python용 Azure SDK 클라이언트 라이브러리를 업데이트하고 있습니다. 이 공유 기능은 [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) 라이브러리에 포함되어 있습니다. 현재 핵심 라이브러리에서 작동하는 라이브러리는 [Azure SDK 최신 릴리스](https://azure.github.io/azure-sdk/releases/latest/#python-packages) 페이지에 나열되어 있습니다.

Azure 핵심 라이브러리 및 해당 지침에 대한 자세한 내용은 [Python 지침: 소개](https://azure.github.io/azure-sdk/python_introduction.html)를 참조하세요.

## <a name="get-help-and-give-feedback"></a>도움말 가져오기 및 피드백 제공

- [Python용 Azure SDK 설명서](https://aka.ms/python-docs)를 참조합니다.
- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python)의 커뮤니티에 질문을 게시합니다.
- [GitHub](https://github.com/Azure/azure-sdk-for-python/issues)에서 SDK에 대한 문제를 엽니다.
- Twitter에서 [@AzureSDK](https://twitter.com/AzureSdk/)를 언급합니다.
