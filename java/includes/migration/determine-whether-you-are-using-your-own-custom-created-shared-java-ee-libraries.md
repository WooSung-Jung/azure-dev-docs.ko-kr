---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 64a4a0e191c70d930ad8c5f9d19cb81dfb9cb851
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78894167"
---
### <a name="determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries"></a>본인이 직접 만든 사용자 지정 공유 Java EE 라이브러리를 사용하고 있는지 확인

공유 Java EE 라이브러리 기능을 사용하는 경우 다음과 같은 두 가지 옵션이 있습니다.

* 애플리케이션 코드를 리팩터링하여 라이브러리의 모든 종속성을 제거하고, 그 대신 기능을 애플리케이션에 직접 통합합니다.
* 서버 클래스 경로에 라이브러리를 추가합니다.
