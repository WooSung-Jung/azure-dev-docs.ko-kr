---
title: Azure App Service에서 Visual Studio Code로 로그 스트리밍
description: 자습서 4부, 로그 보기 또는 추적
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 8484efc27120d374738a933244a3fce71c7c6acb
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686201"
---
# <a name="stream-logs-from-azure-app-service"></a>Azure App Service에서 로그 스트리밍

[이전 단계: 웹 사이트 배포](tutorial-vscode-azure-app-service-node-03.md)

이 단계에서는 `console.log`에 대한 호출을 통해 실행 중인 웹 사이트가 생성한 출력을 보거나 "추적"하는 방법을 알아봅니다. 이러한 출력은 Visual Studio Code의 **출력** 창에 나타납니다.

1. **Azure App Service** 탐색기에서 앱 노드를 마우스 오른쪽 단추로 클릭하고, **Start Streaming Logs**(로그 스트리밍 시작)를 선택합니다.

    ![스트리밍 로그 보기](media/deploy-azure/view-logs.png)

1. 메시지가 표시되면 로깅을 사용하도록 설정하고 애플리케이션을 다시 시작합니다.

    ![로깅을 사용하도록 설정하고 다시 시작하라는 메시지](media/deploy-azure/enable-restart.png)

1. 앱이 다시 시작되면 VS Code 출력 창이 열리고 출력을 표시하는 로그 스트림에 연결됩니다.

    ```bash
    Connecting to log-streaming service...
    2019-09-20 17:33:51.428 INFO  - Container msdocs-vscode-node_2 for site msdocs-vscode-node initialized successfully.
    2019-09-20 17:33:56.500 INFO  - Container logs
    ```

1. 브라우저에서 페이지를 몇 번 새로 고치면 로그 출력이 추가로 표시됩니다.

> [!div class="nextstepaction"]
> [로그가 보입니다.](tutorial-vscode-azure-app-service-node-05.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
