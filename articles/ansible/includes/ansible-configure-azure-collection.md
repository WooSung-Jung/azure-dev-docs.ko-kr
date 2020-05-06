---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 04/09/2020
ms.author: tarcher
ms.openlocfilehash: 4f1fbe75f974aadc9e92e518e0e84d49ee73ed3d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81755226"
---
- **Azure 컬렉션 구성**: 터미널 창에서 다음 명령을 실행하여 Azure 컬렉션을 설치합니다. Azure 컬렉션이 이미 설치되어 있는 경우 `--force` 플래그를 사용하면 최신 버전으로 업데이트됩니다.

    ```bash
    ansible-galaxy collection install azure.azcollection --force
    ```
