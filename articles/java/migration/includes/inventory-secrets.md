---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: e37d0337361ce742ce7f7994ab634e63bc4b2ead
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81671649"
---
### <a name="inventory-secrets"></a>인벤토리 비밀

#### <a name="passwords-and-secure-strings"></a>암호 및 보안 문자열

프로덕션 서버의 모든 속성 및 구성 파일에 비밀 문자열과 암호가 있는지 확인합니다. *$CATALINA_BASE/conf*에서 *server.xml* 및 *context.xml*을 확인해야 합니다. 애플리케이션 내에서 암호 또는 자격 증명을 포함하는 구성 파일을 찾을 수도 있습니다. 여기에는 *META-INF/context.xml*이 포함될 수 있으며, 애플리케이션의 경우에는 *application.properties* 또는 *application.yml* 파일이 포함될 수 있습니다.
