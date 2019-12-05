---
title: Visual Studio Code의 컨테이너 레지스트리 사용
description: 자습서 2부, 컨테이너 레지스트리 사용
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 7d51e8011824ec19e9530f9bc94bcb2ce07f2851
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466635"
---
# <a name="use-a-container-registry"></a>컨테이너 레지스트리 사용

[이전 단계: 소개 및 필수 구성 요소](tutorial-vscode-docker-node-01.md)

이 단계에서는 앱 이미지에 적합한 컨테이너 레지스트리를 설정합니다. Azure App Service와 같은 컨테이너 지원 호스팅 서비스는 레지스트리에서 이미지를 가져옵니다.

이 자습서에서는 이미지용 프라이빗 보안 호스팅 레지스트리인 ACR([Azure Container Registry](https://azure.microsoft.com/services/container-registry/))을 사용합니다. 단, 여기에 표시된 도구와 프로세스는 [Docker Hub](https://hub.docker.com/)와 같은 다른 레지스트리에서도 작동합니다.

## <a name="create-an-azure-container-registry"></a>Azure Container Registry 만들기

1. [Azure Portal](https://portal.azure.com)에 로그인한 다음, **리소스 만들기** > **컨테이너** > **Container Registry**를 선택합니다.

    ![Azure Portal에서 컨테이너 레지스트리 만들기](media/deploy-containers/portal-01.png)

1. **컨테이너 레지스트리 만들기** 양식이 나타나면 적절한 값을 입력합니다.

    - **레지스트리 이름**은 Azure 전체에서 고유해야 하며, 5-50자의 영숫자를 포함해야 합니다.
    - **구독**에서 해당하는 구독을 선택합니다.
    - **리소스 그룹**은 구독 내에서만 고유하면 됩니다.
    - 위치의 **위치**에서 가까운 지역을 선택합니다.
    - **관리 사용자**를 **사용**으로 설정합니다.
    - **SKU**에 대해 **기본**을 선택합니다.

    ![컨테이너 레지스트리 양식의 값](media/deploy-containers/portal-02.png)

1. **만들기**를 선택하여 레지스트리를 만듭니다.

1. 레지스트리가 만들어지면 포털에서 알림을 열고 레지스트리의 **리소스로 이동**을 선택합니다.

    ![새로 만든 레지스트리 리소스 열기](media/deploy-containers/portal-03.png)

1. 레지스트리 페이지에서 **액세스 키**를 선택하고 관리자 자격 증명을 적어둡니다.

    ![Azure Portal의 레지스트리에 대한 레지스트리 자격 증명](media/deploy-containers/portal-04.png)

1. 명령 프롬프트 또는 터미널에서 아래 명령을 사용하고 `<registry_name>`을 레지스트리 이름으로 바꾸고 `<username>`과 `<password>`를 Azure Portal에 표시된 관리 사용자의 값으로 바꿔서 Docker에 로그인합니다.

    ```bash
    docker login <registry_name>.azurecr.io -u <username> -p <password>
    ```

1. Visual Studio Code에서 **Docker** 탐색기를 열고 방금 설정한 레지스트리 엔드포인트가 **레지스트리** 아래 표시되는지 확인합니다.

    ![Docker 탐색기에 레지스트리가 나타나는지 확인](media/deploy-containers/registries.png)

> [!div class="nextstepaction"]
> [레지스트리를 만들었습니다.](tutorial-vscode-docker-node-03.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
