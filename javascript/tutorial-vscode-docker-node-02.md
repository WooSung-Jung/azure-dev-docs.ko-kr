---
title: Visual Studio Code의 컨테이너 레지스트리 사용
description: 자습서 2부, 컨테이너 레지스트리 사용
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: e6dde135a2e6482284488fb83d9f811b02249c4d
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740561"
---
# <a name="use-a-container-registry"></a>컨테이너 레지스트리 사용

[이전 단계: 소개 및 필수 구성 요소](tutorial-vscode-docker-node-01.md)

이 단계에서는 앱 이미지에 적합한 컨테이너 레지스트리를 설정합니다. Azure App Service와 같은 컨테이너 지원 호스팅 서비스는 레지스트리에서 이미지를 가져옵니다.

이 자습서에서는 이미지용 프라이빗 보안 호스팅 레지스트리인 ACR([Azure Container Registry](https://azure.microsoft.com/services/container-registry/))을 사용합니다. 단, 여기에 표시된 도구와 프로세스는 [Docker Hub](https://hub.docker.com/)와 같은 다른 레지스트리에서도 작동합니다.

## <a name="create-an-azure-container-registry"></a>Azure Container Registry 만들기

1. Visual Studio Code에서 <kbd>F1</kbd> 키를 눌러 **명령 팔레트**를 엽니다.

1. 검색 상자에서 "registry"를 입력하고, **Azure Container Registry: 레지스트리 만들기**를 선택합니다.

   ![VS Code의 Docker 탐색기](media/deploy-containers/docker-create-registry.jpg)

1. 프롬프트에서 다음 값을 제공합니다.

    - **레지스트리 이름**은 Azure 전체에서 고유해야 하며, 5-50자의 영숫자를 포함해야 합니다.
    - **SKU**에 대해 **기본**을 선택합니다.
    - **리소스 그룹**은 구독 내에서만 고유하면 됩니다.
    - 위치의 **위치**에서 가까운 지역을 선택합니다.

    Visual Studio Code를 통해 Azure에서 레지스트리를 만드는 프로세스가 시작됩니다. 완료되면 아래와 같은 알림이 표시되어 레지스트리가 성공적으로 만들어졌는지 확인할 수 있습니다.

   ![레지스트리가 만들어졌다고 알려주는 Visual Studio Code의 확인](media/deploy-containers/registry-created.jpg)

1. **Docker** 탐색기를 열고, 방금 설정한 레지스트리 엔드포인트가 **레지스트리** 아래에 표시되는지 확인합니다.

   ![Docker 탐색기에 레지스트리가 나타나는지 확인](media/deploy-containers/docker-explorer-registry.jpg)

## <a name="sign-in-to-azure-container-registry"></a>Azure Container Registry에 로그인

Docker 확장에서 Azure 레지스트리를 볼 수는 있지만, ACR(Azure Container Registry)에 로그인할 때까지 이미지를 푸시할 수 없습니다.

1. <kbd>Ctrl + `</kbd>를 눌러 VS Code에서 **통합 터미널**을 엽니다.

1. 다음 Azure CLI 명령을 실행하여 ACR에 로그인합니다. "<your-registry-name>"을 방금 만든 레지스트리 이름으로 바꿉니다.

    ```bash
    az acr login --name <your-registry-name>
    ```

> [!div class="nextstepaction"]
> [레지스트리를 만들었습니다.](tutorial-vscode-docker-node-03.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
