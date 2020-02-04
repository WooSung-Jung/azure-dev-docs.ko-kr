---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 2dccfba5cfe61ac862aeabe7b928e121502093b7
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830881"
---
### <a name="determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries"></a>본인이 직접 만든 사용자 지정 공유 Java EE 라이브러리를 사용하고 있는지 확인

공유 Java EE 라이브러리 기능을 사용하는 경우 다음과 같은 두 가지 옵션이 있습니다.

1. 애플리케이션 코드를 리팩터링하여 라이브러리의 모든 종속성을 제거하고, 그 대신 기능을 애플리케이션에 직접 통합합니다.
2. 서버 클래스 경로에 라이브러리를 추가합니다.
