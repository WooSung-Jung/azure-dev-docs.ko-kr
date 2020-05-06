---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 171dac6ad7e2761d58f63d173cc78464cedb6dc5
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673339"
---
### <a name="inventory-jndi-resources"></a>JNDI 리소스 인벤토리화

모든 JNDI 리소스를 인벤토리화합니다. 예를 들어 JPA가 `EntityManager` 인스턴스를 특정 데이터베이스에 올바르게 바인딩할 수 있도록 데이터베이스 같은 데이터 원본에 JNDI 이름이 연결되기도 합니다. JNDI 리소스 및 데이터베이스에 대한 자세한 내용은 Oracle 설명서의 [WebLogic Server 데이터 원본](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jdbc.html)을 참조하세요. JMS 메시지 브로커와 같은 기타 JNDI 관련 리소스에는 마이그레이션 또는 재구성이 필요할 수 있습니다. JMS 구성에 대한 자세한 내용은 [JMS 리소스 구성](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/jmsad/overview.html) 이해를 참조하세요.
