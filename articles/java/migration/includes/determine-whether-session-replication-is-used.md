---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 26d0422b9ea569b5657a5b7841319e77ce0f1684
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673539"
---
### <a name="determine-whether-session-replication-is-used"></a>세션 복제가 사용되는지 확인

애플리케이션에서 세션 복제를 사용하는 경우 Oracle Coherence*Web의 유무와 관계없이 다음과 같은 세 가지 옵션이 있습니다.

* Coherence*Web은 Azure 가상 머신에서 WebLogic Server와 함께 실행할 수 있지만, 제품을 프로비저닝한 후 이 옵션을 수동으로 구성해야 합니다. 독립 실행형 Coherence를 사용하는 경우 Azure 가상 머신에서 실행할 수도 있지만, 제품을 프로비저닝한 후 이 옵션을 수동으로 구성해야 합니다.
* 세션 관리에 데이터베이스를 사용하도록 애플리케이션을 리팩터링합니다.
* 세션을 Azure Redis Service로 외부화하도록 애플리케이션을 리팩터링합니다. 자세한 내용은 [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview)를 참조하세요.

이 모든 옵션과 관련하여 WebLogic이 HTTP 세션 상태를 어떻게 복제하는지 숙지하는 것이 좋습니다. 자세한 내용은 Oracle 설명서의 [HTTP 세션 상태 복제](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/failover.html#GUID-E13D8142-66BA-46A1-854F-4FC6F82992DD)를 참조하세요.
