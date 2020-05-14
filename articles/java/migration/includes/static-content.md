---
author: yevster
ms.author: yebronsh
ms.date: 4/10/2020
ms.openlocfilehash: c6f4f3b58ff7d5a8733a0f2df2ab0bf3c6eb6fe4
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988760"
---
#### <a name="read-only-static-content"></a>읽기 전용 정적 콘텐츠

애플리케이션에서 현재 정적 콘텐츠를 제공하는 경우 이를 대체할 위치가 필요합니다. 정적 콘텐츠를 Azure Blob Storage로 이동하고 전역적으로 빠른 다운로드를 위해 Azure CDN을 추가하는 것이 좋습니다. 자세한 내용은 [Azure Storage에서 정적 웹 사이트 호스팅](/azure/storage/blobs/storage-blob-static-website) 및 [빠른 시작: Azure CDN과 Azure Storage 계정 통합](/azure/cdn/cdn-create-a-storage-account-with-cdn)을 참조하세요.

#### <a name="dynamically-published-static-content"></a>동적으로 게시된 정적 콘텐츠

애플리케이션이 애플리케이션에서 업로드/생성되었지만 생성 후 변경할 수 없는 정적 콘텐츠를 허용하는 경우, 위에서 설명한 대로 Azure Blob Storage 및 Azure CDN를 사용하여 업로드 및 CDN 새로 고침을 처리할 수 있습니다. [Azure Functions를 사용하여 정적 콘텐츠 업로드 및 CDN 사전 로드](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)에서 사용할 샘플 구현을 제공했습니다.
