---
title: Windows용 Azure CLI 설치
description: Windows에 Azure CLI를 설치하는 방법
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 05/01/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c5e9118a04b0dc608309093866307fdc7083f591
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030755"
---
# <a name="install-azure-cli-on-windows"></a>Windows에 Azure CLI 설치

MSI를 통해 Azure CLI를 Windows에 설치하면 Windows 명령 프롬프트(CMD) 또는 PowerShell을 통해 CLI에 대한 액세스 권한을 제공 합니다.
WSL(Linux용 Windows 하위 시스템)을 설치하는 경우 Linux 배포에 패키지를 사용할 수 있습니다. 지원되는 패키지 관리자 목록 또는 WSL에서 수동으로 설치하는 방법은 [주 설치 페이지](install-azure-cli.md)를 참조하세요.

[!INCLUDE [current-version](includes/current-version.md)]

## <a name="install-or-update"></a>설치 또는 업데이트

MSI 배포 파일은 Windows에서 Azure CLI를 설치하거나 업데이트하는 데 사용됩니다. MSI 설치 프로그램을 사용하기 전에 현재 버전을 제거 할 필요는 없습니다.

> [!div class="nextstepaction"]
> [MSI 설치 관리자 다운로드](https://aka.ms/installazurecliwindows)

설치 관리자가 컴퓨터를 변경해도 되는지 물어보면 "예" 상자를 클릭합니다.

PowerShell을 사용하여 Azure CLI를 설치할 수도 있습니다. 관리자 권한으로 PowerShell을 시작하고 다음 명령을 실행합니다.

   ```PowerShell
   Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi
   ```
최신 버전의 Windows용 Azure CLI를 다운로드하여 설치합니다. 버전이 이미 설치되어 있으면 기존 버전이 업데이트됩니다. 설치가 완료되면 PowerShell을 다시 열어서 Azure CLI를 사용합니다.

이제 Windows 명령 프롬프트 또는 PowerShell에서 `az` 명령으로 Azure CLI를 실행할 수 있습니다. PowerShell은 Windows 명령 프롬프트에서 사용할 수 없는 일부 탭 완성 기능을 제공합니다. 로그인하려면, [az login](/cli/azure/reference-index#az-login) 명령을 사용합니다.

[!INCLUDE [interactive-login](includes/interactive-login.md)]

다른 인증 방법에 대한 자세한 내용은 [Azure CLI로 로그인](authenticate-azure-cli.md)을 참조하세요.

## <a name="troubleshooting"></a>문제 해결

Windows에 설치할 때 나타나는 몇 가지 일반적인 문제는 다음과 같습니다. 여기서 다루지 않은 문제가 발생하면 [GitHub에서 문제를 제출](https://github.com/Azure/azure-cli/issues)하세요.

### <a name="proxy-blocks-connection"></a>프록시 연결 차단

프록시에서 연결을 차단하여 MSI 설치 관리자를 다운로드할 수 없는 경우 프록시를 올바르게 구성했는지 확인합니다. Windows 10의 경우 이러한 설정은 `Settings > Network & Internet > Proxy` 창에서 관리됩니다. 필요한 설정 또는 머신에서 구성을 관리할 수 있거나 고급 설정이 필요한 경우에 대해 시스템 관리자에게 문의하세요.

> [!IMPORTANT]
> 이러한 설정은 PowerShell 또는 명령 프롬프트에서 모두 CLI를 사용하여 Azure 서비스에 액세스하는 데에도 필요합니다. 이 작업은 PowerShell에서 다음 명령을 사용하여 수행합니다.
>
> ```powershell
> (New-Object System.Net.WebClient).Proxy.Credentials = `
>   [System.Net.CredentialCache]::DefaultNetworkCredentials
> ```

MSI를 가져오려면 프록시에서 다음 주소에 대한 HTTPS 연결을 허용해야 합니다.

* `https://aka.ms/`
* `https://azurecliprod.blob.core.windows.net/`

## <a name="uninstall"></a>제거

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

Windows의 "앱 및 기능" 목록에서 Azure CLI를 제거 할 수 있습니다. 제거 방법은 다음과 같습니다.

| 플랫폼 | Instructions |
|---|---|
| 윈도우 10 | 시작 > 설정 > 앱 |
| Windows 8<br/>Windows 7 | 시작 > 제어판 > 프로그램 > 프로그램 제거 |

이 화면에서 __Azure CLI__ 를 프로그램 검색 창에 입력하세요. 제거할 프로그램은 __Azure용 Microsoft CLI 2.0__ 으로 나열됩니다. 이 애플리케이션을 선택한 다음 `Uninstall` 단추를 클릭합니다.

## <a name="next-steps"></a>다음 단계

Azure CLI를 설치한 후 해당 기능 및 일반 명령을 잠시 둘러보세요.

> [!div class="nextstepaction"]
> [Azure CLI 시작](get-started-with-azure-cli.md)
