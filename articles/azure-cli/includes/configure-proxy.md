---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 05/28/2019
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 676f33377a4e7122941bc789c51465b7f34aa1d3
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030885"
---
프록시로 인해 외부 리소스에 연결할 수 없는 경우 셸에서 `HTTP_PROXY` 및 `HTTPS_PROXY` 변수를 올바르게 설정했는지 확인합니다. 이러한 프록시에 사용할 호스트와 포트를 확인하려면 시스템 관리자에게 문의해야 합니다.

이러한 값은 설치 프로세스에서 사용되는 값을 포함하여 많은 Linux 프로그램에서 사용합니다. 이러한 값을 설정하려면 다음을 수행합니다.

```bash
# No auth
export HTTP_PROXY=http://[proxy]:[port]
export HTTPS_PROXY=https://[proxy]:[port]

# Basic auth
export HTTP_PROXY=http://[username]:[password]@[proxy]:[port]
export HTTPS_PROXY=https://[username]:[password]@[proxy]:[port]
```

> [!IMPORTANT]
> 프록시를 지원하는 경우 CLI를 사용하여 이러한 셸 변수를 Azure 서비스에 연결하도록 설정해야 합니다.
> 기본 인증을 사용하지 않는 경우 `.bashrc` 파일에서 이러한 변수를 내보내는 것이 좋습니다.
> 항상 회사의 보안 정책과 시스템 관리자의 요구 사항을 따르세요.
