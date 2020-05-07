---
author: yevster
ms.author: yebronsh
ms.date: 1/22/2020
ms.openlocfilehash: 2fc4e3b43a051103e7aea6aa77f66666ca454910
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672139"
---
<!-- Included in "### Switch to a supported platform" sections that have different (required) intro paragraphs. For example:

### Switch to a supported platform

App Service offers specific versions of Java SE. To ensure compatibility, migrate your application to one of the supported versions of in its current environment before you proceed with any of the remaining steps. Be sure to fully test the resulting configuration. Use the latest stable release of your Linux distribution in such tests.

-->

> [!NOTE]
> 현재 서버를 지원되지 않는 JDK(예: Oracle JDK 또는 IBM OpenJ9)에서 실행하는 경우 이 유효성 검사가 특히 중요합니다.

현재 Java 버전을 가져오려면 프로덕션 서버에 로그인하고 다음 명령을 실행합니다.

```bash
java -version
```

Azure App Service에서 사용하는 현재 버전을 가져오기 위해 Java 8 런타임을 사용하려는 경우 [Zulu 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts&architecture=x86-64-bit&package=jdk)을 다운로드하고, Java 11 런타임을 사용하려는 경우 [Zulu 11](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts&architecture=x86-64-bit&package=jdk)을 다운로드합니다.
