---
title: Visual Studio Code에서 Azure App Service에 Node.js 앱 배포
description: 자습서 1부, 소개 및 필수 구성 요소
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 9c9c7085788f39e82eaec78b11d9c679b20c24eb
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686217"
---
# <a name="deploy-to-azure-app-service-using-visual-studio-code"></a>Visual Studio Code를 사용하여 Azure App Services에 배포

이 자습서에서는 [App Service 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)을 사용하여 Azure App Service on Linux에 Node.js 애플리케이션을 배포합니다.

## <a name="prerequisites"></a>필수 조건

- [Azure 구독](#azure-subscription).
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure App Service 확장](vscode:extension/ms-azuretools.vscode-azureappservice)
- [Node.js 및 npm](https://nodejs.org/en/download)(Node.js 패키지 관리자)

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">Azure App Service 확장 설치</a>

### <a name="azure-subscription"></a>Azure 구독

Azure 구독이 없는 경우 지금 200달러의 Azure 크레딧으로 체험 계정에 [지금 가입](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)하여 서비스 조합을 사용해 볼 수 있습니다.

## <a name="sign-in-to-azure"></a>Azure에 로그인

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [Azure 확장을 설치했습니다.](tutorial-vscode-azure-app-service-node-02.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=getting-started)
