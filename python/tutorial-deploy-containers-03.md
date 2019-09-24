---
title: Visual Studio Code에서 변경 후 컨테이너를 Azure App Service에 다시 배포
description: 자습서 3단계, 컨테이너 이미지를 다시 빌드하고 다시 배포하는 간단한 단계입니다.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 30b0d0863900c36232b69c23db0eae1c70a34396
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019501"
---
# <a name="make-changes-and-redeploy"></a>변경 및 재배포

[이전 단계: Azure에 이미지 배포](tutorial-deploy-containers-02.md)

불가피하게 앱을 변경했으므로 컨테이너를 여러 번 다시 빌드하고 다시 배포하게 됩니다. 다행히 프로세스는 간단합니다.

1. 앱을 로컬에서 변경하고 테스트합니다. (이 단계와 다음 단계는 자습서, [VS Code에서 Python 컨테이너 만들기](https://code.visualstudio.com/docs/python/tutorial-create-container)에 설명되어 있습니다.)

1. Docker 이미지를 다시 빌드합니다. 앱 코드만 변경하는 경우 빌드는 몇 초 밖에 걸리지 않습니다.

1. 이미지를 레지스트리에 밀어넣습니다. 앱 코드를 다시 변경하는 경우에는 해당 작은 레이어만 밀어넣어야 하며 프로세스는 일반적으로 몇 초 내에 완료됩니다.

1. **Azure: App Service** 탐색기에서 적절한 App Service를 마우스 오른쪽 단추로 클릭하고 **다시 시작**을 선택합니다. 앱 서비스를 다시 시작하면 레지스트리에서 최신 컨테이너 이미지를 자동으로 가져옵니다.

1. 약 15-20초 후 App Service URL을 다시 방문하여 업데이트를 확인합니다.

> [!div class="nextstepaction"]
> [변경하고 다시 배포했습니다.](tutorial-deploy-containers-04.md)

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=03-make-changes-redeploy)
