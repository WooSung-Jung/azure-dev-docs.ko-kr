---
title: Visual Studio Code에서 Node.js 앱의 컨테이너 이미지 만들기
description: 자습서 3부, Node.js 애플리케이션 이미지 만들기
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 1b79f84bd69853578796b4485ca669be98f41006
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686098"
---
# <a name="create-your-nodejs-application-image"></a>Node.js 애플리케이션 이미지 만들기

[이전 단계: 컨테이너 레지스트리 사용](tutorial-vscode-docker-node-02.md)

이 단계에서는 Visual Studio Code의 Docker 확장을 사용하여 필요한 파일을 추가하여 앱의 이미지를 만들고 이미지를 빌드한 다음, 레지스트리로 푸시합니다.

이 연습용 앱이 아직 없으면 [Visual Studio Code Node.js 자습서](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)의 앱을 사용하십시오.

## <a name="add-docker-files"></a>Docker 파일 추가

1. Visual Studio Code에서 **명령 팔레트**(**F1**)를 열고 `add docker files to workspace`를 입력한 다음, **Docker: Add Docker files to workspace**(작업 영역에 Docker 파일 추가) 명령을 선택합니다.

1. 메시지가 표시되면 **Node.js**를 애플리케이션 유형으로 선택한 후 애플리케이션이 수신 대기하는 게시물을 선택합니다.

1. 이 명령은 `.dockerignore` 및 Docker Compose에 대한 구성 파일과 함께 `Dockerfile`을 작성합니다.

    > [!TIP]
    > VS Code는 Docker 파일을 제대로 지원합니다. 스마트 제안, 완료 및 오류 감지와 같은 다양한 언어 기능에 대해 알아보려면 VS Code 설명서에서 [Docker 작업](https://code.visualstudio.com/docs/azure/docker)을 참조하세요.

## <a name="build-a-docker-image"></a>Docker 이미지 빌드

`Dockerfile`은 소스 파일의 위치와 컨테이너 내에서 앱을 시작하는 명령을 포함하여 앱의 환경을 설명합니다.

> [!TIP]
> 컨테이너와 이미지: 컨테이너는 이미지의 인스턴스입니다.

1. **명령 팔레트**(**F1**)를 열고 **Docker 이미지: 이미지 빌드**를 실행하여 이미지를 빌드합니다.

1. 메시지가 표시되면 방금 만든 `Dockerfile`을 선택한 다음, 이미지에 이름을 지정합니다. 이름에는 대상 레지스트리 또는 Docker Hub 계정이 포함되어야 합니다.

    `[registry or username]/[image name]:[tag]`

    이 자습서에서는 Azure Container Registry를 사용하며, 이미지 이름은 다음과 같습니다.

    `msdocsvscodereg.azurecr.io/myexpressapp:latest`

    Docker Hub를 사용하는 경우 Docker Hub 사용자 이름을 사용합니다. 예:

    `fiveisprime/myexpressapp:latest`

1. 완료되면 Visual Studio Code의 **터미널** 패널이 열리고 `docker build` 명령이 실행됩니다. 출력에는 앱 환경을 구성하는 각 단계나 레이어도 표시됩니다.

1. 빌드된 이미지는 **DOCKER** 탐색기의 **이미지** 아래에 나타납니다.

    ![Visual Studio Code의 Docker 이미지 목록](media/deploy-containers/image-list.png)

## <a name="push-the-image-to-a-registry"></a>이미지를 레지스트리로 푸시

1. Visual Studio Code에서 **명령 팔레트**(**F1**)를 열고 **Docker 이미지: 푸시**를 실행하여 방금 빌드한 이미지를 선택합니다. **터미널** 패널에는 이 작업에 사용된 `docker push` 명령이 표시됩니다.

1. 완료되면 Docker 확장 탐색기에서 **이미지** 노드를 확장하여 이미지를 살펴봅니다.

    ![Azure Container Registry에 표시되는 푸시된 이미지](media/deploy-containers/image-in-acr.png)

> [!div class="nextstepaction"]
> [내 애플리케이션 이미지를 만들었습니다.](tutorial-vscode-docker-node-04.md) [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=containerize-app)
