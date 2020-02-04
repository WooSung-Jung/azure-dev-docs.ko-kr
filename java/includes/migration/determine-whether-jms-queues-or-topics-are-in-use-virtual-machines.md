---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: e36fae7adfda8c05e222151872b137fa600cdd97
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830771"
---
### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>JMS 큐 또는 토픽을 사용 중인지 확인

애플리케이션에서 JMS 큐 또는 토픽을 사용하는 경우 외부에 호스트되는 JMS 서버로 마이그레이션해야 합니다. Azure Service Bus 및 고급 메시지 큐 프로토콜은 JMS를 사용하는 애플리케이션을 위한 훌륭한 마이그레이션 전략이 될 수 있습니다. 자세한 내용은 [Azure Service Bus 및 AMQP 1.0에서 JMS(Java Message Service) 사용](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)을 참조하세요.

JMS 영구 저장소가 구성된 경우 해당 구성을 캡처하여 마이그레이션 후에 적용해야 합니다.

Oracle Message Broker를 사용하는 경우 이 소프트웨어를 Azure 가상 머신으로 마이그레이션하여 그대로 사용할 수 있습니다.
