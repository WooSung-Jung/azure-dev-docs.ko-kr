---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: cc634f89170f4db6f961da91e3eb3abe7393539d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673599"
---
### <a name="validate-that-the-supported-java-version-works-correctly"></a>지원되는 Java 버전이 제대로 작동하는지 확인

WebLogic에서 Azure로 마이그레이션하는 모든 경로에는 특정 Java 버전이 필요하며, 필요한 버전은 경로마다 다릅니다. 지원되는 버전을 사용하여 애플리케이션을 올바르게 실행할 수 있는지 확인해야 합니다.

현재 버전을 가져오려면 프로덕션 서버에 로그인하고 다음 명령을 실행합니다.

```bash
java -version
```

> [!NOTE]
> Azure 가상 머신에서 WebLogic으로 마이그레이션할 때 가상 머신에 사전 설치된 Java에 따라 특정 Java 버전에 대한 요구 사항이 결정됩니다.
