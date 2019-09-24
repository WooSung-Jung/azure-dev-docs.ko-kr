---
title: Azure 리소스 정리
description: 자습서 8단계, 지속적인 변경을 방지하기 위해 Azure 리소스 정리
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 9c0b3b8b4a21975a849531d5c6560a291ed4b7f2
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019891"
---
# <a name="clean-up-resources"></a>리소스 정리

[이전 단계: 스토리지 바인딩 추가](tutorial-vs-code-serverless-python-07.md)

사용자가 만든 함수 앱에는 최소한의 비용을 발생시킬 수 있는 리소스가 포함되어 있습니다([함수 가격 책정](https://azure.microsoft.com/pricing/details/functions/) 참조). 리소스를 정리하려면 **Azure: Functions** 탐색기에서 함수 앱을 마우스 오른쪽으로 클릭하고 **함수 앱 삭제**를 선택합니다.

또한 [Azure Portal](https://portal.azure.com)을 방문하여 왼쪽 탐색 창에서 **리소스 그룹**을 선택하고, 이 자습서의 프로세스에서 만든 리소스 그룹을 선택한 다음, **리소스 그룹 삭제** 명령을 사용합니다.

## <a name="next-steps"></a>다음 단계

Azure Functions에 Python 코드를 배포하는 연습을 완료했습니다. 이제 더 많은 서버리스 함수를 만들 준비가 되었습니다.

앞에서 설명한 것처럼 해당 GitHub 리포지토리 [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions)를 방문하여 함수 확장에 대해 자세히 알아볼 수 있습니다. 문제 및 기여도 환영합니다.

사용할 수 있는 다른 트리거를 살펴보려면 [Azure Functions 개요](/azure/azure-functions/functions-overview.md)를 참조하세요.

AI 및 Machine Learning Service와 함께 데이터 스토리지를 포함하여 Python에서 사용할 수 있는 Azure 서비스에 대해 자세히 알아보려면 [Azure Python 개발자 센터](/azure/python/?view=azure-python)를 참조하세요.

도움이 될 수 있는 Visual Studio Code에 대한 다른 Azure 확장도 있습니다. 확장 탐색기에서 "Azure"를 검색하기만 하면 됩니다.

![Visual Studio Code용 Azure 확장](media/tutorial-vs-code-serverless-python/azure-extensions.png)

몇 가지 인기 있는 확장은 다음과 같습니다.

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). [App Service에 배포 자습서](tutorial-deploy-app-service-on-linux-01.md) 참조
- [Azure CLI 도구](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager 도구](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [완료했습니다.](https://docs.microsoft.com/python/azure/?view=azure-python)

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)
