---
title: Visual Studio Code에서 Azure App Service 만들기
description: 자습서 2부, Node.js 앱 만들기
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 769ca61ef7716eaa08d45e109a9bf50e5801721c
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686188"
---
# <a name="create-your-nodejs-application"></a>Node.js 애플리케이션 만들기

[이전 단계: 소개 및 필수 구성 요소](tutorial-vscode-azure-app-service-node-01.md)

이 단계에서는 Express 애플리케이션 생성기를 사용하여 간단한 Node.js 앱을 만든 다음, Azure에 배포합니다.

[Visual Studio Code Node.js 자습서](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)의 앱을 사용할 수도 있으며, 이 경우 [앱 배포](tutorial-vscode-azure-app-service-node-03.md)로 건너뛸 수 있습니다.

1. 터미널 또는 명령 프롬프트에서 다음 명령을 사용하여 Express Generator를 실행하고 "myExpressApp"이라는 새 Express 앱을 스캐폴드합니다. (`--view pug --git` 매개 변수는 [pug](https://pugjs.org/api/getting-started.html) 템플릿 엔진(이전 이름: Jade)을 사용하고 *.gitignore* 파일을 만들도록 생성기에 지시합니다.)

    ```bash
    npx express-generator myExpressApp --view pug –git
    ```

1. 앱 폴더에서 `npm install`을 실행하여 애플리케이션의 종속성을 설치합니다.

    ```bash
    cd myExpressApp
    npm install
    ```

1. `npm start`를 실행하여 서버를 시작합니다.

    ```bash
    npm start
    ```

1. 브라우저에서 [http://localhost:3000](http://localhost:3000)을 열어서 앱을 테스트합니다. 사이트가 다음과 같이 표시됩니다.

    ![Express 애플리케이션 실행](media/deploy-azure/express.png)

> [!div class="nextstepaction"]
> [Node.js 앱을 만들었습니다.](tutorial-vscode-azure-app-service-node-03.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=create-app)
