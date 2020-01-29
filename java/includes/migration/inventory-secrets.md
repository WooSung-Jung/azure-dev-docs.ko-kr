---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 4228d1db44ac16da1c05fe97d20a3491cc9c5f51
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288572"
---
### <a name="inventory-secrets"></a>인벤토리 비밀

#### <a name="passwords-and-secure-strings"></a>암호 및 보안 문자열

프로덕션 서버의 모든 속성 및 구성 파일에 비밀 문자열과 암호가 있는지 확인합니다. $CATALINA_BASE/conf에서 *server.xml* 및 *context.xml*을 확인해야 합니다. 애플리케이션 내에서 암호 또는 자격 증명을 포함하는 구성 파일을 찾을 수도 있습니다. 여기에는 *META-INF/context.xml*이 포함될 수 있으며, 애플리케이션의 경우에는 *application.properties* 또는 *application.yml* 파일이 포함될 수 있습니다.

#### <a name="certificates"></a>인증서

공용 SSL 엔드포인트에 사용되는 모든 인증서를 문서화합니다. 다음 명령을 실행하여 프로덕션 서버에서 모든 인증서를 볼 수 있습니다.

```bash
keytool -list -v -keystore <path to keystore>
```
