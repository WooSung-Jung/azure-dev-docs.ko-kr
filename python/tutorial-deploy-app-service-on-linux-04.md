---
title: '자습서: Azure App Service on Linux에서 Python 앱에 대한 사용자 지정 시작 파일 구성'
description: 자습서 4단계로, App Service에서 웹앱을 시작하는 방법을 설명합니다.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 7c3c863ed333528c675cda939f52b86f53bc8380
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278955"
---
# <a name="tutorial-configure-a-custom-startup-file-for-python-apps-on-azure-app-service"></a>자습서: Azure App Service에서 Python 앱에 대한 사용자 지정 시작 파일 구성

[이전 단계: App Service 만들기](tutorial-deploy-app-service-on-linux-03.md)

이 문서에서는 Azure App Service에서 Python 앱의 커스텀 시작 파일을 구성하는 방법을 보여줍니다.

앱을 구조화한 방법에 따라 Azure 설명서의 [App Service on Linux에서 Python 앱 구성](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python)에 설명된 대로 앱에 대한 사용자 지정 시작 명령 파일을 만들어야 할 수도 있습니다.

사용자 지정 시작 명령의 특정 사용 사례는 다음과 같습니다.

- 시작 파일 및 앱 개체 이름이 각각 *application.py* 및 `app`이 **아닌** 다른 이름으로 지정된 **Flask** 앱이 있는 경우. 즉, 프로젝트의 루트 폴더에 *application.py*가 있고, *또한* Flask 앱 개체 이름이 `app`인 경우가 아니면, 사용자 지정 시작 명령이 필요합니다.
- 기본값을 초과하는 추가 인수(`--bind=0.0.0.0 --timeout 600`)로 Gunicorn 웹 서버를 시작하려는 경우.

## <a name="create-a-startup-file"></a>시작 파일 만들기

사용자 지정 시작 파일이 필요한 경우 다음 단계를 사용합니다.

1. *startup.txt*(또는 원하는 다른 이름)라는 프로젝트에 시작 명령을 포함하는 파일을 만듭니다. Flask의 경우 다음 섹션의 [Flask 시작 명령](#flask-startup-commands)을 참조하세요. Django 앱은 일반적으로 사용자 지정이 필요하지 않습니다.

1. 앱의 나머지 부분을 사용하여 배포할 수 있도록 파일을 코드 리포지토리에 커밋합니다.

1. **Azure: App Service** 탐색기에서 App Service를 확장하고, **애플리케이션 설정**을 마우스 오른쪽 단추로 클릭한 다음, **포털에서 열기**를 선택합니다.

    ![App Service 탐색기에서 포털의 애플리케이션 설정 열기](media/deploy-azure/open-application-settings-in-portal-for-app-service.png)

1. Azure Portal에서 필요한 경우 로그인한 다음, **구성** 페이지에서 **일반 설정**을 선택하고 **스택 설정** > **시작 명령**에서 시작 파일의 이름(예: *startup.txt*)을 입력한 다음, **저장**을 선택합니다.

    ![Azure Portal에서 시작 명령 파일 이름 설정](media/deploy-azure/enter-startup-file-for-app-service-in-the-azure-portal.png)

    > [!NOTE]
    > 시작 명령 파일을 사용하는 대신 시작 명령을 Azure Portal의 **시작 명령** 필드에 직접 배치할 수도 있습니다. 그러나 일반적으로는 파일을 사용하는 것이 좋습니다. 이는 변경 내용을 감사하고 다른 App Service 인스턴스로 다시 배포할 수 있는 리포지토리에 이러한 비트 구성이 유지되기 때문입니다.

1. 변경 내용을 저장하면 App Service가 다시 시작됩니다. 그러나 앱 코드를 아직 배포하지 않았으므로 이 시점에서 사이트를 방문하면 “애플리케이션 오류”가 표시됩니다. 이 메시지는 Gunicorn 서버가 시작되었지만 앱을 찾지 못했으며, 따라서 HTTP 요청에 응답하지 않음을 나타냅니다. 다음 단계에서 앱 코드를 배포합니다.

## <a name="django-startup-commands"></a>Django 시작 명령

기본적으로 App Service는 *wsgi.py* 파일이 포함하는 폴더를 자동으로 찾고, 다음 명령을 사용하여 Gunicorn을 시작합니다.

```bash
# <module> is the path to the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

`--timeout 1200`을 사용하는 등 Gunicorn 인수를 변경하려면 그러한 수정 항목이 포함된 명령 파일을 만듭니다.

## <a name="flask-startup-commands"></a>Flask 시작 명령

기본적으로 App Service on Linux 컨테이너는 Flask 앱의 시작 파일 이름이 *application.py*이고 앱의 루트 폴더에 있는 것으로 가정합니다. 또한 해당 파일 내에 정의된 Flask 앱 개체의 이름이 `app`으로 지정되어 있다고 가정합니다. 앱이 바로 이 방법으로 구조화되지 않은 경우 사용자 지정 시작 명령은 앱 개체의 위치를 식별해야 합니다.

1. **다른 파일 이름 및/또는 앱 개체 이름**: 예를 들어 앱의 시작 파일이 *hello.py*이고 앱 개체의 이름이 `myapp`인 경우 시작 명령은 다음과 같습니다.

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
    ```

1. **시작 파일이 하위 폴더에 있음**: 예를 들어 시작 파일이 *myapp/website.py*이고 앱 개체가 `app`이면 Gunicorn의 `--chdir` 인수를 사용하여 폴더를 지정한 다음, 시작 파일 및 앱 개체의 이름을 일반적인 방법으로 지정합니다.

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 --chdir myapp website:app
    ```

1. **시작 파일이 모듈 내에 있음**: [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) 코드에서 *webapp.py* 시작 파일은 그 자체가 *\_\_init\_\_.py* 파일이 포함된 모듈인 *hello_app* 폴더 내에 포함되어 있습니다. 앱 개체 이름이 `app`이며 *\_\_init\_\_.py*에 정의되어 있고, *webapp.py*가 상대 가져오기를 사용합니다. 이 배열로 인해 Gunicorn이 `webapp:app`을 가리키면 “비패키지 내에서 상대 가져오기를 시도함”이라는 오류가 발생하며 앱이 시작되지 않습니다.

    이 경우 모듈에서 앱 개체를 가져오는 간단한 shim 파일을 만든 다음, Gunicorn이 shim을 사용하여 앱을 시작하게 합니다. 예를 들어 [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) 코드에는 다음 콘텐츠가 포함된 *startup.py*가 들어있습니다.

    ```python
    # startup.py shim
    from hello_app.webapp import app
    ```

    따라서 시작 명령은 다음과 같습니다.

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 startup:app
    ```

> [!div class="nextstepaction"]
> [시작 파일을 구성했습니다.](tutorial-deploy-app-service-on-linux-05.md)

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=04-startup-command)
