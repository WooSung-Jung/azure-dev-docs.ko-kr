---
title: 논리 앱 워크플로 만들기
description: 논리 앱 워크플로 만들기
ms.topic: conceptual
ms.date: 6/15/2017
ms.openlocfilehash: e561d57427b4a8e52f1d13bf3a0ff36fc0ed047c
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441658"
---
# <a name="create-a-logic-app-workflow"></a>논리 앱 워크플로 만들기

다음 코드는 논리 앱 워크플로를 만듭니다.

```python
from azure.mgmt.logic.models import Workflow

group_name = 'myresourcegroup'
workflow_name = '12HourHeartBeat'
logic_client.workflows.create_or_update(
    group_name,
    workflow_name,
    Workflow(
        location = 'East US',
        definition={
            "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {},
            "triggers": {},
            "actions": {},
            "outputs": {}
        }
    )
)
```

