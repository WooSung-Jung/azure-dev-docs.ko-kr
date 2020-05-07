---
author: yevster
ms.author: yebronsh
ms.date: 1/22/2020
ms.openlocfilehash: 1015c179c0f93485decd77bd89a3ceec8833652e
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672159"
---
### <a name="migrate-scheduled-jobs"></a>예약된 작업 마이그레이션

Azure에서 예약된 작업을 실행하려면 [Azure Functions의 타이머 트리거](/azure/azure-functions/functions-bindings-timer)를 사용하는 방안을 고려해보세요. 작업 코드 자체를 함수로 마이그레이션할 필요가 없습니다. 함수는 단순히 애플리케이션에서 URL을 호출하여 작업을 트리거할 수 있습니다. 이러한 작업 실행을 동적으로 호출하고/하거나 중앙에서 추적해야 하는 경우 [Spring Batch](https://spring.io/projects/spring-batch)를 사용하는 방안을 고려해보세요.

또는 애플리케이션 외부에 코드를 작성하지 않고 URL을 호출하는 되풀이 트리거를 사용하여 논리 앱을 만들 수 있습니다. 자세한 내용은 [개요 - Azure Logic Apps란?](/azure/logic-apps/logic-apps-overview) 및 [Azure Logic Apps의 되풀이 트리거를 사용하여 되풀이 작업 만들기, 예약 및 실행](/azure/connectors/connectors-native-recurrence)을 참조하세요.

> [!NOTE]
> 악의적인 사용을 방지하려면 작업 호출 엔드포인트에 자격 증명이 필요한지 확인해야 합니다. 이 경우 트리거 함수에서 자격 증명을 제공해야 합니다.
