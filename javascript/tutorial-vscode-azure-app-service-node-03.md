---
title: Visual Studio Code에서 Azure App Service에 Node.js 앱 배포
description: '자습서 3부: 웹 사이트 배포'
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 937eb54e9885e3b5b9fa7be54f551945a54572cd
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467206"
---
# <a name="deploy-the-website"></a>웹 사이트 배포

[이전 단계: 앱 만들기](tutorial-vscode-azure-app-service-node-02.md)

이 단계에서는 Visual Studio Code와 Azure App Service 확장을 사용하여 Node.js 웹 사이트를 배포합니다. 이 자습서에서는 앱이 압축되어 Azure App Service on Linux에 배포되는 가장 기본적인 배포 모델을 사용합니다.

1. 앱을 배포하기 전에 `PORT` 환경 변수(`process.env.PORT`)에서 제공하는 포트를 수신 대기하고 있는지 확인합니다.

1. 앱 폴더(예: 이전 단계의 `myExpressApp`)에서 다음 명령을 사용하여 해당 폴더에서 VS Code를 시작합니다.

    ```bash
    code .
    ```

1. VS Code에서 Azure 아이콘을 선택하여 **Azure App Service** 탐색기를 연 다음, 파란색 위쪽 화살표 아이콘을 선택하여 Azure에 앱을 배포합니다.

    ![웹앱에 배포](media/deploy-azure/deploy.png)

    > [!TIP]
    > 또는 **명령 팔레트**(**F1**)를 열고 '웹앱에 배포'를 입력하고 **Azure App Service: 웹앱에 배포** 명령을 실행하여 명령 팔레트(Ctrl+Shift+P)에서 배포할 수도 있습니다.

1. 메시지가 표시되면 다음 정보를 입력합니다.

    a. 앱의 현재 폴더를 선택합니다(예: 이 자습서에서 사용된 `myExpressApp`).

    a. **새 웹앱 만들기**를 선택합니다.

    a. 앱에 전역적으로 고유한 이름을 입력하고, **Enter** 키를 누릅니다. 앱 이름에 유효한 문자는 'a-z', '0-9' 및 '-'입니다.

    a. 가까운 Azure 지역([지역](https://azure.microsoft.com/regions/)의 설명 참조) 또는 액세스해야 하는 다른 서비스 근처의 위치를 선택합니다.

    a. **Node.js 버전**을 선택합니다. LTS를 권장 합니다.

1. 확장에서 앱이 생성되는 동안 진행률이 VS Code의 **출력** 창에 나타납니다(**Azure App Service** 출력을 선택해야 할 수도 있음).

    ![Azure App Service의 Visual Studio Code 출력 창](media/deploy-azure/output-window.png)

1. 대상 서버에서 `npm install`을 실행하도록 구성을 업데이트하라는 메시지가 표시되면 **예**를 선택합니다. 그러면 앱이 배포됩니다.

    ![구성된 배포](media/deploy-azure/server-build.png)

1. 배포가 시작되면 이후 모든 배포에서 자동으로 동일한 App Service 웹앱을 대상으로 지정하도록 작업 영역을 업데이트하라는 메시지가 표시됩니다. 변경 내용이 올바른 앱에 배포되도록 하려면 **예**를 선택합니다.

    ![구성된 배포](media/deploy-azure/save-configuration.png)

1. 배포가 완료되면 프롬프트에서 **웹 사이트 찾아보기**를 선택하여 새로 배포된 웹 사이트를 확인합니다.

## <a name="troubleshooting"></a>문제 해결

"이 디렉터리 또는 페이지를 볼 수 있는 권한을 가지고 있지 않습니다." 오류는 앱이 제대로 시작되지 않았음을 나타냅니다. [다음 단계](tutorial-vscode-azure-app-service-node-04.md)에 설명된 대로 로그를 보면 문제점을 진단하고 수정하는 데 유용할 수 있습니다. 해결할 수 없는 경우 아래에 있는 **문제가 발생했습니다.** 단추를 클릭하여 문의해 주세요. 성심껏 도와드리겠습니다!

## <a name="updating-the-website"></a>웹 사이트 업데이트

동일한 프로세스를 사용하여 새 앱을 만들지 않고 기존 앱을 선택하여 변경 내용을 해당 앱에 배포할 수 있습니다.

----

> [!div class="nextstepaction"]
> [내 사이트가 Azure에 있습니다.](tutorial-vscode-azure-app-service-node-04.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=deploy-app)
