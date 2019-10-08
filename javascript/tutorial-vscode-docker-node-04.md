---
title: Visual Studio Code에서 Node.js 앱의 컨테이너 이미지 배포
description: 자습서 4부, Azure App Service에 이미지 배포
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 60eb5be0b3d4049c7955195f3bb6bc85dd2b2498
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686017"
---
# <a name="deploy-the-image-to-azure-app-service"></a>Azure App Service에 이미지 배포

[이전 단계: 앱 이미지 만들기](tutorial-vscode-docker-node-03.md)

이 단계에서는 레지스트리로 푸시한 이미지를 Visual Studio Code에서 직접 [Azure App Service](https://azure.microsoft.com/en-us/services/app-service/)에 배포합니다.

1. **DOCKER** 탐색기의 **레지스트리**에서 이미지의 노드를 펼치고 `:latest`를 마우스 오른쪽 단추로 클릭한 후 **Deploy Image to Azure App Service**(Azure App Service에 이미지 배포)를 선택합니다.

    ![탐색기에서 배포](media/deploy-containers/deploy-image-command.png)

1. 메시지가 표시되면 App Service에 대한 값을 입력합니다.

    - 이름은 Azure에서 고유해야 합니다.
    - 기존 리소스 그룹을 선택하거나 새로 만듭니다. (**리소스 그룹**은 Azure에 있는 애플리케이션 리소스의 명명된 컬렉션입니다.)
    - 기존 App Service 계획을 선택하거나 새로 만듭니다. (**App Service 계획**은 웹 사이트를 호스트하는 물리적 리소스를 정의합니다. 이 자습서에서는 기본 또는 무료 요금제 계층을 사용할 수 있습니다.)

1. 배포가 완료되면 Visual Studio Code에 웹 사이트 URL과 함께 알림이 표시됩니다.

    ![배포 완료 메시지](media/deploy-containers/deploy-successful.png)

1. Visual Studio Code의 **출력** 패널, **Docker** 섹션에서 결과를 확인할 수도 있습니다.

    ![배포 완료 출력](media/deploy-containers/deploy-output.png)

1. 배포된 웹 사이트를 찾아보려면 **출력** 패널에서 URL을 **Ctrl**+**클릭**합니다. 새 App Service는 Visual Studio Code의 **AZURE** 탐색기에서 **APP SERVICE** 섹션 아래에도 표시되며, 여기에서 웹 사이트를 마우스 오른쪽 단추로 클릭하고 **웹 사이트 찾아보기**를 선택할 수 있습니다.

> [!div class="nextstepaction"]
> [내 사이트가 Azure에 있습니다.](tutorial-vscode-docker-node-05.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=deploy-app)
