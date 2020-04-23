---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 33146c309d8fdaf21dc218c11054460cfc3dc8f4
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673589"
---
### <a name="domain-configuration"></a>도메인 구성

WebLogic Server의 기본 구성 단위는 도메인입니다. 따라서 *config.xml* 파일에는 마이그레이션을 신중하게 고려해야 하는 여러 구성이 포함되어 있습니다. 이 파일에는 하위 디렉터리에 저장된 추가 XML 파일의 참조가 포함되어 있습니다. Oracle은 일반적으로 **관리 콘솔**을 사용하여 WebLogic Server의 관리 가능한 개체 및 서비스를 구성하고 WebLogic Server가 *config.xml* 파일을 유지할 수 있게 할 것을 권장합니다. 자세한 내용은 [도메인 구성 파일](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/domcf/config_files.html)을 참조하세요.

#### <a name="inside-your-application"></a>애플리케이션 내부

*WEB-INF/weblogic.xml* 파일 및/또는 *WEB-INF/web.xml* 파일을 검사합니다.
