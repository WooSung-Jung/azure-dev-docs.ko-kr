---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: d56ba6d8f101b1d90468a436f0a111f78a19fc83
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830851"
---
### <a name="determine-whether-weblogic-has-been-customized"></a>WebLogic이 사용자 지정되었는지 확인

다음 사용자 지정이 수행되었는지 확인하고 수행된 작업을 캡처합니다.

* 시작 스크립트가 변경되었습니까? 시작 스크립트에는 *setDomainEnv*, *commEnv*, *startWebLogic* 및 *stopWebLogic*이 포함되어 있습니다.
* 특정 매개 변수가 JVM에 전달되었나요?
* 서버 클래스 경로에 JAR가 추가되었나요?
