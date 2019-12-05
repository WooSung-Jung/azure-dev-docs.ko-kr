---
title: '자습서: Azure 리소스 정리 - Python에서 Azure Functions'
description: 자습서 8단계, 지속적인 변경을 방지하기 위해 Azure 리소스 정리
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: ddc2ab44d3d8865c89cb6cdf8368461b6e4f666a
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74465939"
---
# <a name="tutorial-clean-up-azure-resources-for-azure-functions"></a>자습서: Azure Functions에 대한 Azure 리소스 정리

[이전 단계: 스토리지 바인딩 추가](tutorial-vs-code-serverless-python-07.md)

이 문서에서는 이 자습서에서 만든 Azure 리소스를 제거하는 방법을 보여줍니다. 사용자가 Visual Studio Code로 만든 Azure 함수 앱에는 최소한의 비용을 발생시킬 수 있는 리소스가 포함되어 있습니다.

리소스를 정리하려면 **Azure: Functions** 탐색기에서 함수 앱을 마우스 오른쪽으로 클릭하고 **함수 앱 삭제**를 선택합니다. 자세한 내용은 [Functions 가격 책정](https://azure.microsoft.com/pricing/details/functions/)을 참조하세요.

또한 [Azure Portal](https://portal.azure.com)을 방문하여 왼쪽 탐색 창에서 **리소스 그룹**을 선택하고, 이 자습서의 프로세스에서 만든 리소스 그룹을 선택한 다음, **리소스 그룹 삭제** 명령을 사용합니다.

## <a name="next-steps"></a>다음 단계

Azure Functions에 Python 코드를 배포하는 연습을 완료했습니다. 이제 더 많은 서버리스 함수를 만들 준비가 되었습니다.

앞에서 설명한 것처럼 해당 GitHub 리포지토리 [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions)를 방문하여 함수 확장에 대해 자세히 알아볼 수 있습니다. 문제 및 기여도 환영합니다.

사용할 수 있는 다른 트리거를 살펴보려면 [Azure Functions 개요](/azure/azure-functions/functions-overview)를 참조하세요.

AI 및 Machine Learning Service와 함께 데이터 스토리지를 포함하여 Python에서 사용할 수 있는 Azure 서비스에 대해 자세히 알아보려면 [Azure Python 개발자 센터](/azure/python/?view=azure-python)를 참조하세요.

도움이 될 수 있는 Visual Studio Code에 대한 다른 Azure 확장도 있습니다. 확장 탐색기에서 "Azure"를 검색하기만 하면 됩니다.

![Visual Studio Code용 Azure 확장](media/tutorial-vs-code-serverless-python/azure-extensions-for-visual-studio-code.png)

몇 가지 인기 있는 확장은 다음과 같습니다.

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). [App Service에 배포 자습서](tutorial-deploy-app-service-on-linux-01.md) 참조
- [Azure CLI 도구](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager 도구](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [완료했습니다.](https://docs.microsoft.com/python/azure/?view=azure-python)

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)
