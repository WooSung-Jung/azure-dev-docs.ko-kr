---
title: Azure CLI 확장
description: Azure CLI 확장 사용
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 73488fc464ed052f22f071f6373fb6b8f682b778
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030985"
---
# <a name="use-extensions-with-azure-cli"></a>Azure CLI 확장 사용 

Azure CLI는 확장을 로드하는 기능을 제공합니다. 확장은 Python 휠로, CLI의 일부로 함께 제공되지 않지만 CLI 명령으로 실행됩니다.
확장 기능을 사용하면 자체 CLI 인터페이스를 작성하는 기능과 함께 시험 및 시험판 명령에 액세스할 수 있습니다. 이 문서에서는 확장을 관리하는 방법을 설명하고 해당 사용에 대한 일반적인 질문에 답변합니다.

## <a name="find-extensions"></a>확장 찾기

Microsoft에서 제공 및 유지 관리하는 확장을 참조하려면, [az extension list-available](/cli/azure/extension#az-extension-list-available) 명령을 사용합니다.

```azurecli-interactive
az extension list-available --output table
```

설명서 사이트에 [확장 목록](azure-cli-extensions-list.md)도 있습니다.

## <a name="install-extensions"></a>확장 설치

설치할 확장을 찾은 다음에는 [az extension add](https://docs.microsoft.com/cli/azure/extension#az-extension-add)를 사용하여 가져옵니다. 확장이 `az extension list-available`에 나열된 경우 해당 이름으로 확장을 설치할 수 있습니다.

```azurecli-interactive
az extension add --name <extension-name>
```

확장이 외부 리소스에 있거나 직접 연결되는 링크가 있는 경우 원본 URL 또는 로컬 경로를 제공할 수 있습니다. 이 확장은 _컴파일된 Python 휠 파일이어야 합니다_.

```azurecli-interactive
az extension add --source <URL-or-path>
```

확장이 설치되었으면 `$AZURE_EXTENSION_DIR` 셸 변수 값에서 이 파일을 찾을 수 있습니다. 이 변수가 설정되어 있지 않으면 기본적으로 Linux 및 macOS에서는 `$HOME/.azure/cliextensions`, Windows에서는 `%USERPROFILE%\.azure\cliextensions`입니다.

## <a name="update-extensions"></a>확장 업데이트

확장이 이름별로 설치된 경우 [az 확장 업데이트](https://docs.microsoft.com/cli/azure/extension#az-extension-update)를 사용하여 업데이트합니다.

```azurecli-interactive
az extension update --name <extension-name>
```

그렇지 않으면 [설치 확장](#install-extensions) 지침에 따라 소스에서 확장을 업데이트할 수 있습니다.

CLI가 확장 이름을 해석할 수 없는 경우 이를 제거하고 다시 설치합니다. 확장은 기본 CLI의 일부가 되었을 수 있습니다.
[Azure CLI 설치](install-azure-cli.md)에 설명된 대로 CLI를 업데이트하고 확장 프로그램의 명령이 추가되었는지 확인합니다.

## <a name="uninstall-extensions"></a>확장 제거

확장이 더 이상 필요하지 않으면 [az extension remove](https://docs.microsoft.com/cli/azure/extension#az-extension-remove)를 사용하여 제거합니다.

```azurecli-interactive
az extension remove --name <extension-name>
```

확장을 설치한 위치에서 수동으로 삭제하여 제거할 수도 있습니다. `$AZURE_EXTENSION_DIR` 셸 변수는 모듈 설치 위치를 정의합니다.
이 변수가 설정되어 있지 않으면 기본적으로 Linux 및 macOS에서는 `$HOME/.azure/cliextensions`, Windows에서는 `%USERPROFILE%\.azure\cliextensions`입니다.

```bash
rm -rf $AZURE_EXTENSION_DIR/<extension-name>
```

## <a name="faq"></a>FAQ

다음은 CLI 확장에 대한 몇 가지 다른 일반적인 질문에 대한 답변입니다.

### <a name="what-file-formats-are-allowed-for-installation"></a>설치에 사용할 수 있는 파일 형식은 무엇인가요?

현재 컴파일된 Python 휠 파일만 확장으로 설치할 수 있습니다.

### <a name="can-extensions-replace-existing-commands"></a>확장에서 기존 명령을 바꿀 수 있나요?

예. 확장에서 기존 명령을 바꿀 수 있지만, 바꾼 명령이 실행되기 전에 CLI에서 경고가 발생합니다.

### <a name="how-can-i-tell-if-an-extension-is-in-pre-release"></a>확장이 시험판인지 어떻게 알 수 있나요?

시험판인 경우 확장의 설명서 및 버전 관리가 표시됩니다. Microsoft는 종종 CLI 확장으로 미리보기 명령을 출시하고, 나중에 메인 CLI 제품으로 옮길 수 있는 옵션을 제공합니다. 명령이 확장 외부로 이동되면, 이전 확장을 제거합니다. 

### <a name="can-extensions-depend-upon-each-other"></a>확장은 서로 종속될 수 있나요?

아니요. CLI는 로드 순서를 보장하지 않으므로 종속성을 충족하지 않을 수 있습니다. 확장을 제거하는 것은 다른 것에 영향을 주지 않습니다.

### <a name="are-extensions-updated-along-with-the-cli"></a>확장은 CLI와 함께 업데이트되나요?

아니요. [확장 업데이트](#update-extensions)에 설명된 대로 확장은 별도로 업데이트해야 합니다.

### <a name="how-to-develop-our-own-extension"></a>고유한 확장을 개발하는 방법은 무엇인가요?
자세한 도움말은 [Azure/azure-cli-extensions](https://github.com/Azure/azure-cli/tree/master/doc/extensions) 공식 리포지토리를 참조하세요.
