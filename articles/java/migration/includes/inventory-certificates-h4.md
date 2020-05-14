---
author: yevster
ms.author: yebronsh
ms.date: 1/24/2020
ms.openlocfilehash: 975c6ce1011f76a34f33ec6093f70f281eefa6c3
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990215"
---
#### <a name="inventory-certificates"></a>인증서 인벤토리화

공용 SSL 엔드포인트 또는 백 엔드 데이터베이스와 기타 시스템 간의 통신에 사용되는 모든 인증서를 문서화합니다. 다음 명령을 실행하여 프로덕션 서버에서 모든 인증서를 볼 수 있습니다.

```bash
keytool -list -v -keystore <path to keystore>
```
