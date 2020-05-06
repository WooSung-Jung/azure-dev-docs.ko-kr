---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 029277ac75c455041d26f160b1f0c16a5675368d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673609"
---
### <a name="validate-whether-and-how-the-file-system-is-used"></a>파일 시스템의 사용 여부 및 사용 방법 확인

VM 파일 시스템은 지속성, 시작 및 종료의 측면에서 온-프레미스 파일 시스템과 동일한 방식으로 작동합니다. 그렇다 하더라도, 파일 시스템 요구 사항을 파악하고 VM의 스토리지 크기와 성능이 적절한지 확인해야 합니다.

#### <a name="read-only-static-content"></a>읽기 전용 정적 콘텐츠

애플리케이션에서 현재 정적 콘텐츠를 제공하는 경우 이를 대체할 위치가 필요합니다. 정적 콘텐츠를 Azure Blob Storage로 이동하고 전역적으로 빠른 다운로드를 위해 Azure CDN을 추가하는 것이 좋습니다. 자세한 내용은 [Azure Storage에서 정적 웹 사이트 호스팅](/azure/storage/blobs/storage-blob-static-website) 및 [빠른 시작: Azure CDN과 Azure Storage 계정 통합](/azure/cdn/cdn-create-a-storage-account-with-cdn)을 참조하세요.

#### <a name="dynamically-published-static-content"></a>동적으로 게시된 정적 콘텐츠

애플리케이션이 애플리케이션에서 업로드/생성되었지만 생성 후 변경할 수 없는 정적 콘텐츠를 허용하는 경우, 위에서 설명한 대로 Azure Blob Storage 및 Azure CDN를 사용하여 업로드 및 CDN 새로 고침을 처리할 수 있습니다. [Azure Functions를 사용하여 정적 콘텐츠 업로드 및 CDN 사전 로드](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)에서 사용할 샘플 구현을 제공했습니다.
