---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 11/26/2018
ms.topic: include
ms.openlocfilehash: ee0b2b0c8c557aa54255f28429bb3a174c0e21a9
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030895"
---
### <a name="cli-fails-to-install-or-run-on-windows-subsystem-for-linux"></a>CLI가 Linux 용 Windows 하위 시스템에서 설치 또는 실행 실패

[Linux용 Windows 하위 시스템(WSL)](/windows/wsl/about)이 Windows 플랫폼 상단의 시스템 호출 변환 레이어이기 때문에 Azure CLI를 설치하거나 실행할 때 오류가 발생할 수 있습니다. CLI는 WSL에서 버그가 있을 수 있는 몇 가지 기능을 사용합니다. CLI를 설치 하는 방법에 관계 없이 오류가 발생하는 경우, 이는 CLI 설치 프로세스는 상관 없이 WSL 문제일 가능성이 높습니다.

WSL 설치 문제를 해결하려면 다음을 수행합니다.

* 가능한 경우 Linux 컴퓨터 또는 VM에 동일한 설치 프로세스를 실행하여 성공하는지 알아봅니다. 성공하는 경우 문제는 거의 확실히 WSL과 관련이 있습니다. Azure에서 Linux VM을 시작하려면 [Azure Portal에서 Linux VM 만들기](/azure/virtual-machines/linux/quick-create-portal) 설명서를 참조합니다.
* 최신 버전의 WSL를 실행하고 있는지 확인합니다. 최신 버전을 가져오려면 [Windows 10 설치를 업데이트합니다](https://support.microsoft.com/help/4027667/windows-10-update).
* WSL의 [미해결 문제](https://github.com/Microsoft/WSL/issues)가 있는지 확인하여 문제를 해결할 수도 있습니다.
  종종 문제를 해결하는 방법에 대한 제안이나 문제가 해결될 릴리스에 대한 정보가 제공됩니다.
* 문제에 대한 기존 문제가 없는 경우 [WSL로 새로운 문제를 제기](https://github.com/Microsoft/WSL/issues/new)하고 최대한 많은 정보를 포함시켜야 합니다.

WSL에서 설치 또는 실행 문제가 계속되면 [Windows용 CLI 설치](../install-azure-cli-windows.md)를 고려해 보세요.
