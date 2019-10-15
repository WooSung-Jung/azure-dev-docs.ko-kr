---
title: '자습서: 컨테이너에 대한 Azure App Service에서 Visual Studio Code로 로그 스트리밍'
description: 자습서 4부, Azure App Service의 로그를 보고 동작을 모니터링합니다.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 0f002444d2454b734821e067e65fa513619a68bf
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172218"
---
# <a name="tutorial-stream-logs-from-azure-app-service-for-a-container"></a>자습서: 컨테이너에 대한 Azure App Service에서 로그 스트리밍

[이전 단계: 변경 및 재배포](tutorial-deploy-containers-03.md)

VS Code 내에서 Azure App Service의 실행 중인 사이트에서 로그(또는 "테일")를 볼 수 있습니다. 이 경우에는 `print` 명령문에서 콘솔에 대한 모든 출력을 캡처하고 VS Code **출력** 패널로 라우팅합니다.

1. **Azure: App Service** 탐색기에서 앱을 찾아서 마우스 오른쪽 단추로 클릭하고, **로그 스트리밍 시작**을 선택합니다.

1. 메시지가 표시되면 **예**에 대답하여 로깅을 사용하도록 설정하고 앱을 다시 시작합니다. 앱이 다시 시작되면 VS Code 출력 패널이 열려 로그 스트림에 연결됩니다.

1. 몇 초 후에 로그 스트리밍 서비스에 연결되었다는 메시지가 표시됩니다.

    ```bash
    Connecting to log stream...
    2018-09-27T20:14:26  Welcome, you are now connected to log-streaming service.

    2018-09-27 20:14:59.269 INFO  - Starting container for site

    2018-09-27 20:14:59.270 INFO  - docker run -d -p 24138:8000 --name vsdocs-django-sample-container_0 -e WEBSITES_PORT=8000 -e WEBSITE_SITE_NAME=vsdocs-django-sample-container -e WEBSITE_AUTH_ENABLED=False -e WEBSITE_ROLE_INSTANCE_ID=0 -e WEBSITE_INSTANCE_ID=02c705ae24eaf5f298e553a9c2724b9fe4485707c2d1c36137cd02931091e561 -e HTTP_LOGGING_ENABLED=1 vsdocsregistry.azurecr.io/python-sample-vscode-django-tutorial:latest

    2018-09-27 20:15:06.216 INFO  - Container vsdocs-django-sample-container_0 for site vsdocs-django-sample-container initialized successfully.
    ```

1. 앱 내에서 이동하여 다양한 HTTP 요청에 대한 추가 출력을 볼 수 있습니다.

> [!div class="nextstepaction"]
> [로그가 표시됩니다.](tutorial-deploy-containers-05.md)

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=04-stream-logs)