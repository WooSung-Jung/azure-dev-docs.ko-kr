---
title: macOS용 Azure CLI 설치
description: macOS에 Azure CLI를 설치하는 방법
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 11/05/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 862ebf9144d7e81be6dda550eba108198f38d397
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031065"
---
# <a name="install-azure-cli-on-macos"></a>macOS에 Azure CLI 설치

macOS 플랫폼의 경우 [homebrew 패키지 관리자](https://brew.sh)를 사용하여 Azure CLI를 설치할 수 있습니다. Homebrew를 사용하면 손쉽게 CLI 업데이트 설치를 최신으로 유지할 수 있습니다. CLI 패키지는 macOS 버전 10.9 이상에서 테스트 되었습니다.

[!INCLUDE [current-version](includes/current-version.md)]

## <a name="install-with-homebrew"></a>Homebrew로 설치

Homebrew는 CLI 설치를 관리하는 가장 쉬운 방법입니다. 설치, 업데이트 및 제거 하는 편리한 방법을 제공 합니다.
시스템에 homebrew가 없는 경우 [homebrew를 설치](https://docs.brew.sh/Installation.html)한 후 계속 진행하세요.

brew 리포지토리 정보를 업데이트한 후 `install` 명령을 실행하여 CLI를 설치할 수 있습니다.

```bash
brew update && brew install azure-cli
```

> [!IMPORTANT]
>
> Azure CLI는 Homebrew `python3` 패키지에 종속되어 있으므로 이 패키지를 설치합니다.
> Azure CLI는 Homebrew에 게시된 최신 버전의 `python3`과 호환되도록 보장됩니다.

그런 다음 `az` 명령을 사용하여 Azure CLI를 실행할 수 있습니다. 로그인하려면, [az login](/cli/azure/reference-index#az-login) 명령을 사용합니다.

[!INCLUDE [interactive-login](includes/interactive-login.md)]

다른 인증 방법에 대한 자세한 내용은 [Azure CLI로 로그인](authenticate-azure-cli.md)을 참조하세요.

## <a name="troubleshooting"></a>문제 해결

Homebrew 통해 CLI를 설치하는 경우 문제가 발생하는 경우 몇 가지 공용 오류가 있습니다. 여기에서 다루지 않는 문제가 발생하는 경우, [github에 문제를 제출합니다](https://github.com/Azure/azure-cli/issues).

### <a name="completion-is-not-working"></a>완료가 작동하지 않음

Azure CLI의 Homebrew 수식은 Homebrew에서 관리하는 completions 디렉터리에 `az`라는 완료 파일을 설치합니다(기본 위치는 `/usr/local/etc/bash_completion.d/`). 완료를 사용하도록 설정하려면 [여기](https://docs.brew.sh/Shell-Completion)에 나와 있는 Homebrew의 지침을 따르세요.

### <a name="unable-to-find-python-or-installed-packages"></a>Python 또는 설치된 패키지를 찾을 수 없음

homebrew 설치 중 사소한 버전 불일치 또는 다른 문제가 있을 수 있습니다. CLI는 Python 가상 환경을 사용하지 않으므로, 설치된 Python 버전을 찾는 것에 의존합니다. 가능한 수정 방법은 프로그램을 설치하고 Homebrew에서 `python3` 종속성을 다시 연결하는 것입니다.

```bash
brew update && brew install python3 && brew upgrade python3
brew link --overwrite python3
```

### <a name="cli-version-1x-is-installed"></a>CLI 버전 1.x가 설치되었습니다

낮은 버전이 설치된 경우 부실한 homebrew 캐시로 인한 것일 수 있습니다. [업데이트](#update) 지침을 따르세요.

### <a name="proxy-blocks-connection"></a>프록시 연결 차단

프록시를 사용하도록 올바르게 구성하지 않으면 Homebrew에서 리소스를 가져올 수 없습니다. [Homebrew 프록시 구성 지침](https://docs.brew.sh/Manpage#using-homebrew-behind-a-proxy)을 따르세요.

> [!IMPORTANT]
> 프록시를 지원하는 경우 CLI를 사용하여 `HTTP_PROXY` 및 `HTTPS_PROXY`를 Azure 서비스에 연결하도록 설정해야 합니다.
> 기본 인증을 사용하지 않는 경우 `.bashrc` 파일에서 이러한 변수를 내보내는 것이 좋습니다.
> 항상 회사의 보안 정책과 시스템 관리자의 요구 사항을 따르세요.

Homebrew에서 bottle 리소스를 가져오려면 프록시에서 다음 주소에 대한 HTTPS 연결을 허용해야 합니다.

* `https://formulae.brew.sh`
* `https://homebrew.bintray.com`

## <a name="update"></a>업데이트

CLI는 버그 픽스, 향상된 기능, 새로운 기능 및 미리 보기 기능이 정기적으로 업데이트됩니다. 약 2주마다 새 릴리스가 제공됩니다. 로컬 리포지토리 정보를 업데이트한 후 `azure-cli` 패키지를 업그레이드해야 합니다.

```bash
brew update && brew upgrade azure-cli
```

## <a name="uninstall"></a>제거

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

Homebrew를 사용하여 `azure-cli` 패키지를 제거합니다.

```bash
brew uninstall azure-cli
```

## <a name="other-installation-methods"></a>다른 설치 방법

Homebrew를 사용하여 사용자 환경에 Azure CLI를 설치할 수 없는 경우 Linux용 설명서 지침을 사용할 수 있습니다. 이 프로세스는 공식적으로 macOS와 호환되도록 유지되지 않습니다. Homebrew와 같은 패키지 관리자를 사용하는 것이 항상 권장됩니다. 사용할 수 있는 다른 옵션이 없는 경우에만 수동 설치 방법을 사용합니다.

수동 설치 지침은 [Linux에 Azure CLI 수동 설치](install-azure-cli-linux.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

Azure CLI를 설치한 후 해당 기능 및 일반 명령을 잠시 둘러보세요.

> [!div class="nextstepaction"]
> [Azure CLI 시작](get-started-with-azure-cli.md)
