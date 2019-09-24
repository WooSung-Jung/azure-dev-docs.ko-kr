---
title: Visual Studio Code를 사용하여 Azure App Service에 Docker 컨테이너 배포
description: 자습서 1단계, 소개 및 필수 구성 요소
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 37df8bf4a84d13d6fc4e25ea6c91ef14b6f6f502
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019491"
---
# <a name="deploy-containers-to-azure-app-service"></a>Azure App Service에 컨테이너 배포

이 자습서에서는 Visual Studio Code를 사용하여 모든 Visual Studio Code 내에서 컨테이너 레지스트리의 컨테이너 이미지를 [Azure App Service](https://azure.microsoft.com/services/app-service/containers/)에 배포하는 과정을 안내합니다.

이 자습서의 단계 중 문제가 발생하는 경우 세부 정보를 듣고 싶습니다. 각 문서의 끝에 있는 **문제가 발생했습니다.** 링크를 사용하여 피드백을 제출합니다.

## <a name="prerequisites"></a>필수 조건

- [Azure 계정](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- 컨테이너 레지스트리에 업로드된 적절한 컨테이너입니다. 예를 들어 Python 웹앱을 사용하여 컨테이너를 만들고 레지스트리에 추가하는 방법에 대한 자세한 내용은 [컨테이너 만들기](https://code.visualstudio.com/python/tutorial-create-containers)에서 찾을 수 있습니다.
- [VS Code용 Azure App Service 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
- [VS Code용 Docker 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).

## <a name="sign-in-to-azure"></a>Azure에 로그인

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [Azure에 로그인되어 있음](tutorial-deploy-containers-02.md)

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=01-verify-prerequisites)
