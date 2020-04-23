---
title: Azure CLI의 출력 형식
description: Azure CLI 명령 출력을 테이블, 목록 또는 json 형식으로 지정하는 방법을 알아봅니다.
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 7f28a5cc7acd7e5d41e1ab91424a9f360a25b702
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031035"
---
# <a name="output-formats-for-azure-cli-commands"></a>Azure CLI 명령의 출력 형식

Azure CLI는 기본 출력 형식으로 JSON을 사용하지만 다른 형식도 제공합니다.  `--output`(`--out` 또는 `-o`) 매개 변수를 사용하여 CLI 출력의 형식을 지정합니다. 인수 값과 출력의 형식은 다음과 같습니다.

--output | Description
---------|-------------------------------
`json`   | JSON 문자열 이 설정은 기본값
`jsonc`  | 색으로 구분된 JSON
`yaml`   | 머신이 읽을 수 있는 JSON 대안인 YAML.
`table`  | 열 제목인 키가 포함된 ASCII 테이블
`tsv`    | 키가 포함되지 않은 탭으로 구분된 값
`none`   | 오류와 경고 이외의 출력은 없음

## <a name="json-output-format"></a>JSON 출력 형식

다음 예제에서는 구독에 있는 가상 머신의 목록을 기본 JSON 형식으로 표시합니다.

```azurecli-interactive
az vm list --output json
```

다음과 같은 출력에는 편의를 위해 생략되고, 교체된 정보를 식별하는 몇몇 필드가 있습니다.

```json
[
  {
    "availabilitySet": null,
    "diagnosticsProfile": null,
    "hardwareProfile": {
      "vmSize": "Standard_DS1"
    },
    "id": "/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010",
    "instanceView": null,
    "licenseType": null,
    "location": "westus",
    "name": "DemoVM010",
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/.../resourceGroups/demorg1/providers/Microsoft.Network/networkInterfaces/DemoVM010VMNic",
          "primary": null,
          "resourceGroup": "demorg1"
        }
      ]
    },
          ...
          ...
          ...
]
```

## <a name="yaml-output-format"></a>YAML 출력 형식

`yaml` 형식은 출력을 일반 텍스트 데이터 serialization 형식인 [YAML](http://yaml.org/)로 인쇄합니다. YAML은 JSON보다 읽기 쉬우며, 해당 형식으로 쉽게 매핑됩니다. 일부 애플리케이션 및 CLI 명령은 JSON 대신 YAML을 구성 입력으로 사용합니다.

```azurecli-interactive
az vm list --out yaml
```

다음과 같은 출력에는 편의를 위해 생략되고, 교체된 정보를 식별하는 몇몇 필드가 있습니다.

```yaml
- availabilitySet: null
  diagnosticsProfile: null
  hardwareProfile:
    vmSize: Standard_DS1_v2
  id: /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010
  identity: null
  instanceView: null
  licenseType: null
  location: westus
  name: ExampleVM1
  networkProfile:
    networkInterfaces:
    - id: /subscriptions/.../resourceGroups/DemoRG1/providers/Microsoft.Network/networkInterfaces/DemoVM010Nic
      primary: null
      resourceGroup: DemoRG1
  ...
...
```

## <a name="table-output-format"></a>테이블 출력 형식

`table` 형식은 ASCII 테이블로 출력을 인쇄하여 읽고 검사하기 쉽게 합니다. 중첩된 개체는 테이블 출력에 포함되지 않지만 쿼리의 일부로 필터링될 수 있습니다. 일부 필드는 테이블에 포함되지 않습니다. 따라서 빠르고 검색 가능한 데이터의 개요를 만들려는 경우 이 형식을 사용하는 것이 좋습니다.

```azurecli-interactive
az vm list --out table
```

```output
Name         ResourceGroup    Location
-----------  ---------------  ----------
DemoVM010    DEMORG1          westus
demovm212    DEMORG1          westus
demovm213    DEMORG1          westus
KBDemo001VM  RGDEMO001        westus
KBDemo020    RGDEMO001        westus
```

`--query` 매개 변수를 사용하여 목록 출력에 표시할 속성과 열을 사용자 지정할 수 있습니다. 다음 예제에서는 `list` 명령에서 VM 이름 및 리소스 그룹 이름만 선택하는 방법을 보여 줍니다.

```azurecli-interactive
az vm list --query "[].{resource:resourceGroup, name:name}" -o table
```

```output
Resource    Name
----------  -----------
DEMORG1     DemoVM010
DEMORG1     demovm212
DEMORG1     demovm213
RGDEMO001   KBDemo001VM
RGDEMO001   KBDemo020
```

> [!NOTE]
> 일부 키는 기본적으로 테이블 보기에 인쇄되지 않습니다. `id`, `type` 및 `etag`가 포함됩니다. 출력에 표시해야 하는 경우 JMESPath 리키잉 기능을 사용하여 키 이름을 변경하고 필터링을 방지할 수 있습니다.
>
> ```azurecli-interactive
> az vm list --query "[].{objectID:id}" -o table
> ```

데이터를 필터링하는 쿼리를 사용하는 방법에 대한 자세한 내용은 [Azure CLI와 함께 JMESPath 쿼리 사용](/cli/azure/query-azure-cli)을 참조하세요.

## <a name="tsv-output-format"></a>TSV 출력 형식

`tsv` 출력 형식은 추가 서식 지정, 키 또는 다른 기호 없이 탭 및 줄 바꿈으로 구분된 값을 반환합니다. 이 형식은 출력을 어떤 형태의 텍스트를 처리해야 하는 다른 명령 및 도구에 간편하게 사용할 수 있습니다. `table` 형식과 마찬가지로 `tsv`는 중첩된 개체를 인쇄하지 않습니다.

앞의 예제에 `tsv` 옵션을 사용하면 탭으로 구분된 결과가 출력됩니다.

```azurecli-interactive
az vm list --out tsv
```

```output
None    None        /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010    None    None    westus    DemoVM010            None    Succeeded    DEMORG1    None            Microsoft.Compute/virtualMachines    cbd56d9b-9340-44bc-a722-25f15b578444
None    None        /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm212    None    None    westus    demovm212            None    Succeeded    DEMORG1    None            Microsoft.Compute/virtualMachines    4bdac85d-c2f7-410f-9907-ca7921d930b4
None    None        /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm213    None    None    westus    demovm213            None    Succeeded    DEMORG1    None            Microsoft.Compute/virtualMachines    2131c664-221a-4b7f-9653-f6d542fbfa34
None    None        /subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo001VM    None    None    westus    KBDemo001VM            None    Succeeded    RGDEMO001    None            Microsoft.Compute/virtualMachines    14e74761-c17e-4530-a7be-9e4ff06ea74b
None    None        /subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo020   None    None    westus    KBDemo020            None    Succeeded    RGDEMO001    None            Microsoft.Compute/virtualMachines    36baa9-9b80-48a8-b4a9-854c7a858ece
```

TSV 출력 형식의 제한 중 하나는 출력 순서에 대한 보장이 없다는 것입니다. CLI는 순서를 유지하기 위해 응답 JSON의 키를 알파벳 순서로 정렬한 다음, TSV 출력을 위해 키 값을 인쇄합니다. 하지만 Azure 서비스 응답 형식이 변경될 수 있으므로 순서가 항상 동일하다는 보장은 없습니다.

일관적인 순서를 적용하려면 `--query` 매개 변수와 [다중 선택 목록](query-azure-cli.md#get-multiple-values) 형식을 사용해야 합니다. CLI 명령에서 단일 JSON 사전을 반환하는 경우 일반 형식 `[key1, key2, ..., keyN]`을 사용하여 키 순서를 적용합니다.  배열을 반환하는 CLI 명령의 경우 일반 형식 `[].[key1, key2, ..., keyN]`을 사용하여 열 값의 순서를 지정합니다.

예를 들어 위에 표시되는 정보를 ID, 위치, 리소스 그룹 및 VM 이름순으로 지정하려면 다음을 수행합니다.

```azurecli-interactive
az vm list --out tsv --query '[].[id, location, resourceGroup, name]'
```

```output
/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010    westus    DEMORG1    DemoVM010
/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm212    westus    DEMORG1    demovm212
/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm213    westus    DEMORG1    demovm213
/subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo001VM     westus  RGDEMO001       KBDemo001VM
/subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo020       westus  RGDEMO001       KBDemo020
```

다음 예제는 `tsv` 출력이 bash에서 다른 명령에 파이프되는 방법을 보여줍니다. 쿼리는 출력을 필터링하고 순서를 적용하는 데 사용되고, `grep`은 이름에 "RGD"라는 텍스트가 포함된 항목을 선택하고, `cut` 명령은 네 번째 필드를 선택하여 출력에 VM 이름을 표시합니다.

```azurecli-interactive
az vm list --out tsv --query '[].[id, location, resourceGroup, name]' | grep RGD | cut -f4
```

```output
KBDemo001VM
KBDemo020
```

## <a name="set-the-default-output-format"></a>기본 출력 형식을 설정합니다.

대화형 `az configure` 명령을 사용하여 환경을 설정하고 출력 형식에 대한 기본 설정을 지정합니다. 기본 출력 형식은 `json`입니다.

```azurecli-interactive
az configure
```

```output
Welcome to the Azure CLI! This command will guide you through logging in and setting some default values.

Your settings can be found at /home/defaultuser/.azure/config
Your current configuration is as follows:

  ...

Do you wish to change your settings? (y/N): y

What default output format would you like?
 [1] json - JSON formatted output that most closely matches API responses.
 [2] jsonc - Colored JSON formatted output that most closely matches API responses.
 [3] table - Human-readable output format.
 [4] tsv - Tab- and Newline-delimited. Great for GREP, AWK, etc.
 [5] yaml - YAML formatted output. An alternative to JSON. Great for configuration files.
 [6] none - No output, except for errors and warnings.
Please enter a choice [1]:
```

환경을 구성하는 방법에 대해 자세히 알아보려면 [Azure CLI 구성](/cli/azure/azure-cli-configuration)을 참조하세요.
