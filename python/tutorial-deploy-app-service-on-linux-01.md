---
title: '자습서: Visual Studio Code에서 Linux의 Azure App Service에 Python 앱 배포'
description: 자습서 1단계, 소개, 필수 구성 요소 및 Azure에 로그인
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 24f615f5f456276b1ed78fc431e3cdd929e2d1cd
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172518"
---
# <a name="tutorial-deploy-python-apps-to-azure-app-service-on-linux-from-visual-studio-code"></a>자습서: Visual Studio Code에서 Linux의 Azure App Service에 Python 앱 배포

이 문서에서는 Visual Studio Code를 사용하여 [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) 확장을 통해 Linux에서 Azure App Service에 Python 애플리케이션을 배포하는 과정을 안내합니다.

이 자습서의 단계 중 문제가 발생하는 경우 자세한 내용을 알려주시면 감사하겠습니다. 각 문서의 끝에 있는 **문제가 발생했습니다.** 링크를 사용하여 피드백을 제출합니다.

> [!TIP]
> 현재 Python에 대해 미리 보기로 제공되는 [Linux의 Azure App Service](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro)는 미리 정의된 Docker 컨테이너에서 소스 코드를 실행합니다. 해당 컨테이너는 [Gunicorn](https://gunicorn.org) 웹 서버를 사용하여 Python 3.7 앱을 실행합니다. 이 컨테이너의 특징은 [Linux에서 App Service용 Python 앱 구성](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python)에 설명되어 있습니다. 컨테이너 정의 자체는 [github.com/Azure-App-Service/python](https://github.com/Azure-App-Service/python/tree/master/3.7)에 있습니다.

## <a name="prerequisites"></a>필수 조건

- [Azure 구독](#azure-subscription).
- [Azure App Service 확장이 있는 Visual Studio Code](#visual-studio-code-python-and-the-azure-app-service-extension)
- Python 환경

### <a name="azure-subscription"></a>Azure 구독

Azure 구독이 없는 경우 지금 200달러의 Azure 크레딧으로 체험 계정에 [지금 가입](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)하여 서비스 조합을 사용해 볼 수 있습니다.

### <a name="visual-studio-code-python-and-the-azure-app-service-extension"></a>Visual Studio Code, Python 및 Azure App Service 확장

다음 소프트웨어를 설치합니다.

- [Visual Studio Code](https://code.visualstudio.com/)
- [VS Code Python 자습서 - 필수 구성 요소](https://code.visualstudio.com/docs/python/python-tutorial)에 설명된 대로 Python 및 [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) 확장
- VS Code 내에서 Azure App Service와 상호 작용을 제공하는 [Azure App Service](vscode:extension/ms-azuretools.vscode-azureappservice) 확장 일반 정보를 보려면 [App Service 확장 자습서](https://code.visualstudio.com/tutorials/app-service-extension/getting-started)를 검색하고 [vscode-azureappservice GitHub 리포지토리](https://github.com/Microsoft/vscode-azureappservice)를 방문하세요.

## <a name="sign-in-to-azure"></a>Azure에 로그인

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [Azure에 로그인되어 있음](tutorial-deploy-app-service-on-linux-02.md)

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=01-verify-prerequisites)
