---
ms.openlocfilehash: c9d46571c22c9bee49352dd4c1f492b2c9ff60a8
ms.sourcegitcommit: aa2c66b0fecce51862cc9115f68d39c770f0b2ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77709837"
---
Azure 확장이 설치되면, **Azure** 탐색기로 이동하여 **Azure에 로그인**을 선택하고 표시되는 메시지에 따라 Azure 계정에 로그인합니다. (Azure 확장이 여러 개 설치되어 있으면, 작업 중인 영역에 맞는 확장(예: App Service, Functions)을 선택합니다.)

![VS Code를 통해 Azure에 로그인](../media/deploy-azure/azure-sign-in.png)

로그인한 후에는 Azure 계정의 이메일 주소(또는 "로그인함")가 상태 표시줄에 표시되고 구독이 **Azure** 탐색기에 표시되는지 확인합니다.

![Azure 계정을 보여 주는 VS Code 상태 표시줄](../media/deploy-azure/azure-account-status-bar.png)

![구독을 보여 주는 VS Code Azure 탐색기](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> **"이름이 [구독 ID]인 구독을 찾을 수 없습니다"** 오류가 표시되면 프록시를 사용하고 Azure API에 연결할 수 없기 때문일 수 있습니다. 터미널의 프록시 정보를 사용하여 `HTTP_PROXY` 및 `HTTPS_PROXY` 환경 변수를 구성합니다.
>
> ```bash
> # MacOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```cmd
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
