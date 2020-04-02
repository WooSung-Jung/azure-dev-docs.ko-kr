---
title: Python용 Azure SDK 설치
description: pip 또는 GitHub를 사용하여 Python용 Azure SDK를 설치하는 방법입니다. Azure SDK는 개별 라이브러리 또는 전체 패키지로 설치할 수 있습니다.
ms.date: 10/31/2019
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: bf74567e5018c7d48141a6992cafc91e90045a25
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441768"
---
# <a name="install-the-azure-sdk-for-python"></a>Python용 Azure SDK 설치

Python용 Azure SDK는 Python 코드에서 Azure와 상호 작용할 수 있는 API를 제공합니다. 필요에 따라 SDK에서 개별 라이브러리를 설치하세요.

Python용 Azure SDK는 CPython 버전 2.7 및 3.5.3 이상과 PyPy 5.4 이상에서 테스트되고 지원됩니다. 또한 개발자는 IronPython 및 Jython과 같은 다른 인터프리터에서 SDK를 사용하지만, 격리된 문제와 비호환성 문제가 발생할 수 있습니다. Python 인터프리터가 필요한 경우 [python.org/downloads](https://www.python.org/downloads)에서 최신 버전을 설치하세요.

## <a name="install-sdk-libraries-using-pip"></a>pip를 사용하여 SDK 라이브러리 설치

Python용 Azure SDK는 각각 특정 Azure 서비스를 프로비저닝하거나 사용하는 여러 개별 라이브러리로 구성됩니다. `pip install <library>`를 사용하여 각 항목을 설치할 수 있습니다. 각 라이브러리에 대한 자세한 지침 및 설명서는 [SDK 릴리스 페이지](https://azure.github.io/azure-sdk/releases/latest/python.html)를 참조하세요.

예를 들어 Azure Storage를 사용하는 경우 `azure-storage-file`, `azure-storage-blob` 또는 `azure-storage-queue` 라이브러리를 설치할 수 있습니다. Azure Cosmos DB 테이블을 사용하는 경우 `azure-cosmosdb-table`을 설치합니다. Azure Functions는 `azure-functions` 라이브러리 등을 통해 지원됩니다. `azure-mgmt-`로 시작하는 라이브러리는 Azure 리소스를 프로비저닝할 수 있는 API를 제공합니다.

### <a name="install-specific-library-versions"></a>특정 라이브러리 버전 설치

특정 버전의 라이브러리를 설치해야 하는 경우 명령줄에서 버전을 지정합니다.

```bash
pip install azure-storage-blob==12.0.0
```

> [!NOTE]
> Linux 시스템에서 SDK는 `sudo pip install`을 사용하여 모든 사용자를 위한 라이브러리를 설치하는 것을 지원하지 않습니다. 각 사용자는 `pip install`을 별도로 사용해야 합니다. 

### <a name="install-preview-packages"></a>미리 보기 패키지 설치

Microsoft는 예정된 기능을 지원하는 미리 보기 SDK 라이브러리를 정기적으로 릴리스합니다. 라이브러리의 최신 미리 보기를 설치하려면 명령줄에서 `--pre` 플래그를 포함시킵니다. 

```bash
# Install all preview versions of the Azure SDK for Python
pip install --pre azure

# Install the preview version for azure-storage-blob only.
pip install --pre azure-storage-blob
```

## <a name="verify-sdk-installation-details-with-pip"></a>pip를 사용하여 SDK 설치 세부 정보 확인

`pip show <library>` 명령을 사용하여 라이브러리가 설치되어 있는지 확인합니다. 라이브러리가 설치되어 있으면 명령에서 버전 및 기타 요약 정보를 표시합니다. 라이브러리가 설치되지 않으면 명령에서 아무 것도 표시하지 않습니다.

```bash
# Check installation of the Azure SDK for Python
pip show azure

# Check installation of a specific library
pip show azure-storage-blob
```

또한 `pip freeze` 또는 `pip list`를 사용하여 현재 Python 환경에 설치된 모든 라이브러리를 확인할 수도 있습니다.

## <a name="uninstall-azure-sdk-for-python-libraries"></a>Python용 Azure SDK 라이브러리 제거

개별 라이브러리를 제거하려면 `pip uninstall <library>`를 사용합니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [SDK를 사용하는 방법 알아보기](azure-sdk-get-started.yml)
