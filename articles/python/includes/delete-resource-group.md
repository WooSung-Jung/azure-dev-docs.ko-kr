---
ms.openlocfilehash: 8f757c030849cb89eea36d74b55867dcc2ff98a6
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441358"
---
[Azure Portal](https://portal.azure.com) 또는 Azure CLI를 사용하여 리소스 그룹을 삭제할 수 있습니다.

- 포털의 왼쪽 탐색 창에서 **리소스 그룹**을 선택하고 이 자습서의 프로세스에서 만든 리소스 그룹을 선택한 다음, **리소스 그룹 삭제** 명령을 사용합니다.

- 다음 Azure CLI 명령(로컬 또는 [Cloud Shell](/azure/cloud-shell/overview) 사용)을 실행하고 `<resource_group>`을 이 자습서에 사용된 그룹의 이름으로 바꿉니다.

    ```azurecli
    az group delete --name <resource_group>
    ```
