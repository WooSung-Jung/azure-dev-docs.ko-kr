---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a521396292214fd4e68b4625a0e5f5bcfca8be92
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672919"
---
### <a name="determine-whether-java-message-service-jms-queues-or-topics-are-in-use"></a>JMS(Java Message Service) 큐 또는 토픽을 사용하는지 확인

애플리케이션에서 JMS 큐 또는 토픽을 사용하는 경우 외부에서 호스팅되는 JMS 서버로 마이그레이션해야 합니다. Azure Service Bus 및 AMQP(고급 메시지 큐 프로토콜)는 JMS를 사용하는 애플리케이션에 매우 유용한 마이그레이션 전략이 될 수 있습니다. 자세한 내용은 [Azure Service Bus 및 AMQP 1.0에서 JMS 사용](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)을 참조하세요.

JMS 영구 저장소가 구성된 경우 해당 구성을 캡처하여 마이그레이션 후에 적용해야 합니다.
