---
title: '자습서: VS Code를 사용하여 Python에서 서버리스 Azure Functions 만들기 및 배포'
description: 자습서 1단계로, 소개 및 필수 구성 요소입니다.
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: a380a447150f29653a1f94a3fe1f6464dd495a81
ms.sourcegitcommit: fc3408b6e153c847dd90026161c4c498aa06e2fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/19/2019
ms.locfileid: "75191000"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>자습서: Visual Studio Code를 사용하여 Python에서 서버리스 Azure Functions 만들기 및 배포

이 문서에서는 Visual Studio Code 및 Azure Functions 확장을 사용하여 Python으로 서버리스 HTTP 엔드포인트를 만들거나, 스토리지에 연결(또는 “바인딩”)도 추가합니다.

Azure Functions는 가상 머신을 프로비저닝하거나 웹앱을 게시할 필요가 없는 서버리스 환경에서 코드를 실행합니다. Visual Studio Code에 대한 Azure Functions 확장은 많은 구성 문제를 자동으로 처리하여 Functions를 사용하는 프로세스를 크게 간소화합니다.

이 자습서의 단계 중 문제가 발생하는 경우 자세한 내용을 알려주시면 감사하겠습니다. 각 문서의 끝에 있는 **문제가 발생했습니다.** 링크를 사용하여 피드백을 제출해주세요.

## <a name="prerequisites"></a>사전 요구 사항

- [Azure 구독](#azure-subscription).
- [Azure Functions Core Tools](#azure-functions-core-tools).
- [Azure Functions 확장을 사용하는 Visual Studio Code](#visual-studio-code-python-and-the-azure-functions-extension).

### <a name="azure-subscription"></a>Azure 구독

Azure 구독이 없는 경우 지금 200달러의 Azure 크레딧으로 30일 체험 계정에 [지금 가입](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)하여 서비스 조합을 사용해 볼 수 있습니다.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

[Azure Functions Core Tools 사용](/azure/azure-functions/functions-run-local#v2)에 나와 있는 해당 운영 체제 지침에 따라 Azure Functions Core Tools를 설치합니다. 이 자습서를 완료하는 데 필요하지 않은 Chocolately 패키지 관리자에 대한 문서의 설명은 무시합니다.

Node.js를 설치할 때는 기본 옵션을 사용하고 필요한 도구를 자동으로 설치하는 옵션을 선택하지 *않습니다*.  또한 `-g` 옵션을 `npm install` 명령과 함께 사용하여 Core Tools를 후속 명령에서 사용할 수 있도록 해야 합니다.

    > [!TIP]
    > The Core Tools are written in .NET Core, and the Core Tools package is best installed using the Node.js package manager, npm, which is why you need to install .NET Core and Node.js at present, even for working with Azure Functions in Python. You can, however bypass the .NET Core requirement using "extension bundles" as described in the aforementioned documentation. Whatever the case, you need install these components only once, after which Visual Studio Code automatically prompts you to install any updates.

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code, Python 및 Azure Functions 확장

다음 소프트웨어를 설치합니다.

- Azure Functions에 필요한 Python 3.7 또는 Python 3.6. [Python 3.7.5](https://www.python.org/downloads/release/python-375/) 및 [Python 3.6.8](https://www.python.org/downloads/release/python-368/)은 호환되는 최신 버전입니다. 이러한 페이지에서 아래로 스크롤하여 설치 관리자를 찾습니다. 설치할 때 **PATH에 Python 3.x 추가**를 선택하고 **지금 설치** 옵션을 선택하여 기본 옵션을 사용합니다. Windows에서는 프로세스가 끝날 때 **경로 길이 제한 사용 안 함**을 선택합니다.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Visual Studio Code Python 자습서 - 필수 구성 요소](https://code.visualstudio.com/docs/python/python-tutorial)에 설명된 [Python 확장](https://marketplace.visualstudio.com/items?itemName=ms-python.python).
- [Azure Functions 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions). 일반 정보는 [vscode-azurefunctions GitHub 리포지토리](https://github.com/Microsoft/vscode-azurefunctions)를 참조하세요.

    > [!NOTE]
    > Azure Functions 확장은 [Azure Tools 확장 팩](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)에 포함되어 있습니다.

### <a name="sign-in-to-azure"></a>Azure에 로그인

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>필수 조건 확인

모든 Azure Functions 도구가 설치되어 있는지 확인하려면 Visual Studio Code 명령 팔레트(**F1**)를 열고 **터미널: 새 통합 터미널 만들기** 명령을 선택하고 터미널이 열리면 `func` 명령을 실행합니다.

![Azure Functions 핵심 도구 필수 구성 요소 확인](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

Azure Functions 로고로 시작하는 출력(출력을 위쪽으로 스크롤 해야 함)은 Azure Functions Core Tools가 존재함을 나타냅니다.

`func` 명령이 인식되지 않으면 `npm install -g azure-functions-core-tools`를 다시 실행하여 설치에 성공했는지 확인합니다. 또한 install 명령에 `-g` 스위치를 사용해야 합니다. 그렇지 않으면 npm이 현재 폴더에만 패키지를 설치합니다.

`func` 명령은 Node.js 글로벌 폴더에 설치된 *func.cmd* 파일을 통해 작동합니다. 이 폴더의 위치를 확인하려면 `npm -l`를 실행하고 출력의 끝에 있는 위치를 살펴봅니다.

> [!div class="nextstepaction"]
> [Azure에 로그인되어 있음](tutorial-vs-code-serverless-python-02.md)

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
