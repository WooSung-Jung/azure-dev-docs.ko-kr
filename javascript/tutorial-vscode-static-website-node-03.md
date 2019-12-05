---
title: Visual Studio Code에서 정적 Node.js 웹 사이트에 대한 Azure Storage 계정 만들기
description: 자습서 3부, Azure Storage 계정 만들기
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 42badfc649d7cc43eb1a58ab20c8ff639eff5354
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466499"
---
# <a name="create-an-azure-storage-account"></a>Azure Storage 계정 만들기

[이전 단계: 앱 만들기](tutorial-vscode-static-website-node-02.md)

이 단계에서는 기본 제공 웹 서버를 사용하여 간단한 파일 저장소(또는 CDN) 역할을 하는 Azure Storage 계정을 만듭니다. 웹 서버가 기본 제공되기 때문에, 정적 사이트를 빠르게 호스트하려면 Azure Storage를 선택하는 것이 좋습니다.

1. 이전 단계에서 만든 `my-static-app` 폴더에서 Visual Studio Code를 시작하면 해당 폴더가 자동으로 열립니다.

    ```bash
    code .
    ```

1. VS Code에서 Azure 로고를 선택하여 **Azure** 탐색기를 엽니다. **Azure Storage**에서 Azure 구독을 마우스 오른쪽 단추로 클릭하고 **스토리지 계정 만들기**를 선택합니다.

    ![VS Code에서 스토리지 계정 만들기](media/static-website/create-storage-account.png)

1. "Enter the name of the new storage account"(새 스토리지 계정의 이름 입력) 프롬프트에서 스토리지 계정에 대해 전역적으로 고유한 이름을 입력하고 Enter를 누릅니다. 앱 이름으로 유효한 문자는 'a-z'와 '0-9'입니다.

1. "리소스 그룹 선택" 프롬프트에서 **새 리소스 그룹 만들기**를 선택하고 기본 이름을 적용합니다.

1. "위치 선택" 프롬프트에서 근처 [지역](https://azure.microsoft.com/regions/)을 선택합니다.

1. 스토리지 계정이 생성되는 동안 VS Code의 **출력** 패널에 진행률이 표시됩니다.

1. 스토리지 계정이 완성되면 해당 계정을 마우스 오른쪽 단추로 클릭하고 **Configure Static Website**(정적 웹 사이트 구성)를 선택합니다. 정적 웹 사이트 호스팅을 사용하도록 설정하면 Azure Storage에서 인덱스 문서 및 기타 정적 자산이 자동으로 제공됩니다.

    ![Storage 계정 만들기](media/static-website/configure-static-website.png)

1. 프롬프트가 표시되면 인덱스 문서 이름과 404 오류 문서 이름 둘 다에 *index.html*을 입력합니다. 오류 문서에 *index.html*을 사용하는 이유는 React, Angular 및 Vue와 같은 최신 SPA(단일 페이지 앱)가 클라이언트에서 오류를 처리하기 때문입니다. 클래식 정적 웹 사이트의 경우 사용자 지정 404 오류 페이지를 사용합니다.

> [!div class="nextstepaction"]
> [스토리지 컨테이너를 만들었습니다.](tutorial-vscode-static-website-node-04.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
