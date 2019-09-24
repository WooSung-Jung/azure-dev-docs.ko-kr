---
ms.openlocfilehash: d64a5c908915b5d020f27e1c501aee852f04ae38
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019661"
---
Azure 확장을 설치한 후 **Azure: App Service** 탐색기로 이동하여 Azure 계정에 로그인하고, **Azure에 로그인**을 선택하고, 프롬프트를 따릅니다.

![VS Code를 통해 Azure에 로그인](../media/deploy-azure/azure-sign-in.png)

로그인한 후에는 Azure 계정의 이메일 주소가 상태 표시줄에 표시되고 구독이 **Azure: App Service** 탐색기에 표시되는지 확인합니다.

![Azure 계정을 보여 주는 VS Code 상태 표시줄](../media/deploy-azure/azure-account-status-bar.png)

![구독을 보여 주는 VS Code Azure App Service 탐색기](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> **"이름이 [구독 ID]인 구독을 찾을 수 없습니다"** 오류가 표시되면 프록시를 사용하고 Azure API에 연결할 수 없기 때문일 수 있습니다. 터미널의 프록시 정보를 사용하여 `HTTP_PROXY` 및 `HTTPS_PROXY` 환경 변수를 구성합니다.
>
> ```sh
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
>
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
