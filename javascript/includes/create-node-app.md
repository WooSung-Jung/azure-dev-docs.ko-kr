---
author: burkeholland
ms.service: app-service
ms.topic: include
ms.date: 03/31/2020
ms.author: buhollan
ms.openlocfilehash: 59eea52aab3f2b1941a3accb5848bc4ad92d2992
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740504"
---
1. 터미널 또는 명령 프롬프트에서 앱 폴더를 만들려는 위치로 이동합니다.

1. 다음 명령을 실행하여 Express Generator를 통해 *myexpressapp*이라는 새 Express 앱을 만듭니다. (`--git --view pug` 매개 변수는 *.gitignore* 파일을 만들고 [pug](https://pugjs.org/api/getting-started.html) 템플릿 엔진(이전의 Jade)을 사용하도록 생성기에 지시합니다.)

    ```bash
    npx express-generator myexpressapp --git --view pug
    ```

1. 앱 폴더로 이동합니다.

    ```bash
    cd myexpressapp
    ```

1. 애플리케이션의 종속성을 설치합니다.

    ```bash
    npm install
    ```

1. 서버를 시작합니다.

    ```bash
    npm start
    ```

1. 브라우저에서 [http://localhost:3000](http://localhost:3000)을 열어서 앱을 테스트합니다. 사이트가 다음과 같이 표시됩니다.

    ![Express 애플리케이션 실행](../media/deploy-azure/express.png)

1. 터미널에서 **Ctrl**+**C**를 눌러 서버를 중지합니다.
