---
title: Visual Studio Code에서 정적 Node.js 앱 만들기
description: 자습서 2부, 샘플 앱 만들기
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 566d166a69bbbee59726b8e381bee4a24077d8c6
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685977"
---
# <a name="create-the-app"></a>앱 만들기

[이전 단계: 소개 및 필수 구성 요소](tutorial-vscode-static-website-node-01.md)

이 단계에서는 React 유틸리티 CLI인 [create-react-app](https://github.com/facebook/create-react-app)을 사용하여 Azure에 배포할 수 있는 간단한 React 앱을 만듭니다. Angular, Vue, 다른 프레임워크 또는 HTML 파일이 포함된 폴더를 대신 사용할 수도 있습니다. 배포 준비가 완료된 앱이 이미 있으면 [Azure Storage 계정 만들기](tutorial-vscode-static-website-node-03.md)로 건너뛸 수 있습니다.

1. 다음 명령을 실행하여 create-react-app 도구를 사용하여 `my-react-app`이라는 새 React 앱을 스캐폴드합니다.

    ```bash
    npm create react-app my-react-app
    ```

1. 새 폴더로 전환하고 `npm run build`를 실행하여 애플리케이션을 빌드합니다.

    ```bash
    cd my-react-app
    npm run build
    ```

1. 이제 *my-react-app* 폴더에 *build* 폴더가 있어야 합니다. *build* 폴더에는 Azure Storage에 배포하는 HTML, CSS 및 JavaScript 파일이 포함되어 있습니다.

1. 다음 명령을 사용하여 앱을 실행합니다.

    ```bash
    npm start
    ```

1. 브라우저로 [http://localhost:3000](http://localhost:3000)을 열어서 앱이 실행 중인지 확인합니다.

    ![실행 중인 샘플 React 앱](media/static-website/local-app.png)

1. 터미널 또는 명령 프롬프트에서 **Ctrl**+**C**를 눌러서 서버를 중지합니다.

> [!div class="nextstepaction"]
> [앱을 만들었습니다.](tutorial-vscode-static-website-node-03.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
