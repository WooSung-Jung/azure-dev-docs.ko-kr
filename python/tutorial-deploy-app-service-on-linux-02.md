---
title: '자습서: Visual Studio Code에서 Linux의 Azure App Service에 배포할 앱 준비'
description: 자습서 2단계, 애플리케이션 설정
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: b284dd6b5a5d1a09f1be48fb2ab7e6a8f95a4708
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172365"
---
# <a name="tutorial-prepare-your-app-for-deployment-to-azure-app-service"></a>자습서: Azure App Service에 배포할 수 있도록 앱 준비

[이전 단계: 필수 구성 요소](tutorial-deploy-app-service-on-linux-01.md)

작업하려는 앱이 이미 있는 경우 Flask 또는 Django와 같은 프레임워크를 포함하여 종속성을 설명하는 *requirements.txt* 파일이 있는지 확인합니다.

앱이 아직 없는 경우 아래 옵션 중 하나를 사용합니다. 앱이 로컬에서 실행되는지 확인해야 합니다.

## <a name="minimal-flask-app"></a>최소 Flask 앱

이 섹션에서는 이 연습에서 사용되는 최소 Flask 앱에 대해 설명합니다.

1. 새 폴더를 만들고, VS Code에서 열고, 아래 콘텐츠가 포함된 *hello.py*라는 파일을 추가합니다. 앱 개체는 의도적으로 `myapp`이라고 이름이 지정되어 나중에 설명하는 것처럼 App Service의 시작 명령에 이름이 사용되는 방법을 보여 줍니다.

    ```python
    from flask import Flask
    myapp = Flask(__name__)

    @myapp.route("/")
    def hello():
        return "Hello Flask, on Azure App Service for Linux"
    ```

1. 다음과 같은 콘텐츠가 포함된 *requirements.txt*라는 파일을 만듭니다.

    ```text
    Flask==1.1.1
    ```

1. [Flask 자습서 - Flask에 대한 프로젝트 환경 만들기](https://code.visualstudio.com/docs/python/tutorial-flask#create-a-project-environment-for-flask)의 지침을 따라 앱을 로컬에서 실행할 수 있는 Flask가 설치된 가상 환경을 만듭니다.

1. 이 앱을 실행하려면 다음 명령을 사용합니다(운영 체제에 따라). FLASK_APP 환경 변수는 앱 개체를 찾을 위치를 Flask에 알려 줍니다.

    ```ps
    set FLASK_APP=hello:myapp
    flask run
    ```

    ```bash
    export FLASK_APP=hello:myapp
    flask run
    ```

    그런 다음, URL `http://127.0.0.1:5000/`을 사용하여 브라우저에서 앱을 열 수 있습니다.

## <a name="vs-code-flask-tutorial-sample"></a>VS Code Flask 자습서 샘플

[Flask 자습서](https://code.visualstudio.com/docs/python/tutorial-flask)를 수행하는 결과인 [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial)을 다운로드하거나 복제합니다.

## <a name="vs-code-django-tutorial-sample"></a>VS Code Django 자습서 샘플

[Django 자습서](https://code.visualstudio.com/docs/python/tutorial-django)를 수행하는 결과인 [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial)을 다운로드하거나 복제합니다.

Django 앱이 이 샘플과 같은 로컬 SQLite 데이터베이스를 사용하는 경우 리포지토리에 *db.sqlite3* 파일의 미리 초기화되고 미리 채워진 복사본을 포함해야 합니다. 그 이유는 현재 Linux용 App Service는 배포의 일부로 Django의 `migrate` 명령을 실행할 방법이 없으므로 미리 만든 데이터베이스를 배포해야 합니다. 이 경우에도 데이터베이스는 사실상 읽기 전용입니다. 데이터베이스에 쓰는 경우에도 오류가 발생합니다.

어떤 경우든 가장 좋은 옵션은 앱 코드와 독립적으로 배포 및 초기화되는 별도의 데이터베이스를 사용하는 것입니다.

> [!div class="nextstepaction"]
> [앱이 준비되었습니다.](tutorial-deploy-app-service-on-linux-03.md)

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=02-prepare-app)