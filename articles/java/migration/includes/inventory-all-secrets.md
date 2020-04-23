---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a771d0689e83e0b61bf9b8c31e0cbf36949fdc20
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673399"
---
### <a name="inventory-all-secrets"></a>모든 비밀 인벤토리화

Azure Key Vault 같은 "서비스로 구성" 기술이 등장하기 전에는 "비밀"에 대한 개념이 잘 정의되지 않았습니다. 그 대신 우리가 현재 "비밀"이라고 부르는 기능을 효율적으로 수행하는 별도의 구성 세트가 있었습니다. WebLogic Server 같은 앱 서버를 사용하는 경우 이러한 비밀이 다양한 구성 파일 및 구성 저장소에 있습니다. 프로덕션 서버의 모든 속성 및 구성 파일에 비밀과 암호가 있는지 확인합니다. WAR에 *weblogic.xml* 파일이 있는지 확인합니다. 애플리케이션 내에서도 암호 또는 자격 증명을 포함하는 구성 파일을 찾을 수 있습니다. 자세한 내용은 [Azure Key Vault 기본 개념](/azure/key-vault/basic-concepts)을 참조하세요.
