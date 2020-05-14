---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 73f3893f22764280ed5d7c030e7c8eab7d2bf6b1
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988775"
---
호스트 OS에 대한 종속성이 있는 코드가 애플리케이션에 포함되어 있는 경우 해당 종속성을 제거하려면 이 코드를 리팩터링해야 합니다. 예를 들어 파일 시스템 경로에서 사용하고 있는 `/` 또는 `\`를 [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) 또는 [`Paths.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-)으로 바꿔야 할 수 있습니다.
