---
title: Visual Studio Code에서 Azure App Service에 Docker 컨테이너 배포
description: 자습서 1부, 소개 및 필수 구성 요소
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 2d6721060281fb73d31576caa47f316f2d078d29
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467157"
---
# <a name="deploy-containers-to-azure-app-service"></a>Azure App Service에 컨테이너 배포

이 자습서에서는 Visual Studio Code를 사용하여 Docker를 사용하여 컨테이너화된 Node.js 애플리케이션을 만들고 레지스트리에 컨테이너 이미지를 푸시한 다음, Azure App Service에 이미지를 배포합니다.

## <a name="prerequisites"></a>필수 조건

- [Azure 구독](#azure-subscription).
- [Visual Studio Code](https://code.visualstudio.com/)
- [Docker 확장](vscode:extension/ms-azuretools.vscode-docker)
- [Azure App Service 확장](vscode:extension/ms-azuretools.vscode-azureappservice)
- [Node.js 및 npm](https://nodejs.org/en/download)(Node.js 패키지 관리자)
- [Docker](https://www.docker.com/community-edition).

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-docker">Docker 확장 설치</a>

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">Azure App Service 확장 설치</a>

### <a name="azure-subscription"></a>Azure 구독

Azure 구독이 없는 경우 지금 200달러의 Azure 크레딧으로 체험 계정에 [지금 가입](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)하여 서비스 조합을 사용해 볼 수 있습니다.

## <a name="sign-in-to-azure"></a>Azure에 로그인

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="verify-docker-install"></a>Docker 설치 확인

터미널 또는 명령 프롬프트에서 다음 명령을 실행하여 Docker가 제대로 설치되었는지 확인합니다.

```bash
docker --version
```

출력은 다음과 같이 나타납니다.

```output
Docker Version 17.12.0-ce, build c97c6d6
```

> [!div class="nextstepaction"]
> [Docker 확장을 설치했습니다.](tutorial-vscode-docker-node-02.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=getting-started)
