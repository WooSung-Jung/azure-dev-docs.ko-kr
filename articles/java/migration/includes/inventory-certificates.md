---
author: yevster
ms.author: yebronsh
ms.date: 1/24/2020
ms.openlocfilehash: 3cf4edcf12d7fe72c0fa5a0216d1a8c97a2f2e59
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672149"
---
### <a name="inventory-certificates"></a>인증서 인벤토리화

공용 SSL 엔드포인트 또는 백 엔드 데이터베이스와 기타 시스템 간의 통신에 사용되는 모든 인증서를 문서화합니다. 다음 명령을 실행하여 프로덕션 서버에서 모든 인증서를 볼 수 있습니다.

```bash
keytool -list -v -keystore <path to keystore>
```
