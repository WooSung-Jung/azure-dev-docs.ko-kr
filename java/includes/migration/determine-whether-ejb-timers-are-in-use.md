---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: bda7ba230d8287101fc8f3f298b08777bb6e1c61
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897473"
---
### <a name="determine-whether-ejb-timers-are-in-use"></a>EJB 타이머가 사용 중인지 확인

애플리케이션에서 EJB 타이머를 사용하는 경우 각 WildFly 인스턴스에서 독립적으로 EJB 타이머 코드를 트리거할 수 있는지 확인해야 합니다. 이 유효성 검사는 Azure Kubernetes 서비스 배포 시나리오에서 각 EJB 타이머가 자체 WildFly 인스턴스에서 트리거되기 때문에 필요합니다.
