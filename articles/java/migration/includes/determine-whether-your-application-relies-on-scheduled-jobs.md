---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 4d2df28d5c752d20454c28a6d4a53698aa7ff5b6
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673019"
---
### <a name="determine-whether-your-application-relies-on-scheduled-jobs"></a>애플리케이션에서 예약된 작업을 사용하는지 확인

Quartz Scheduler 작업 또는 Unix cron 작업과 같은 예약된 작업을 Azure Kubernetes Service에서 사용해서는 안 됩니다. Azure Kubernetes Service는 예약된 작업이 포함된 애플리케이션을 내부적으로 배포하는 것을 방지하지 않습니다. 그러나 애플리케이션이 확장되는 경우 동일한 예약된 작업이 예약된 기간마다 두 번 이상 실행될 수 있습니다. 이 경우 의도하지 않은 결과가 발생할 수 있습니다.

AKS 클러스터에서 예약된 작업을 실행하려면 필요에 따라 Kubernetes CronJob을 정의합니다. 자세한 내용은 [CronJob을 사용하여 자동화된 작업 실행](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)을 참조하세요.
