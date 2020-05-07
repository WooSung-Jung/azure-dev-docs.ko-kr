---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 3427cc445333c9851a760a607c402e689c7b6ba4
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673159"
---
### <a name="inventory-all-certificates"></a>모든 인증서 인벤토리화

공용 SSL 엔드포인트에 사용되는 모든 인증서를 문서화합니다. 다음 명령을 실행하여 프로덕션 서버에서 모든 인증서를 볼 수 있습니다.

```bash
keytool -list -v -keystore <path to keystore>
```
