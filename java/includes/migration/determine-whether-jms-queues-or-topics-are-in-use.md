---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: b2458a80eccfcae24a3364208334cce3aedea861
ms.sourcegitcommit: 21ddeb9bd9abd419d143dc2ca8a7c821a1758cf9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128903"
---
### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>JMS 큐 또는 토픽을 사용 중인지 확인

애플리케이션에서 JMS 큐 또는 토픽을 사용하는 경우 외부에 호스트되는 JMS 서버로 마이그레이션해야 합니다(예: Azure Service Bus로 마이그레이션하려면 [Service Bus를 메시지 브로커로 사용](/azure/service-bus-messaging/message-transfers-locks-settlement) 참조).

JMS 영구 저장소가 구성된 경우 해당 구성을 캡처하여 마이그레이션 후에 적용해야 합니다.
