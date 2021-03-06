---
title: '7단계: Visual Studio Code에서 Linux의 Azure App Service에 배포 후 리소스 정리'
description: 자습서 7 단계, Azure 리소스 정리
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 001e5d58311267221c5fafb3de89aff3859f4197
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441408"
---
# <a name="7-clean-up-resources-after-deploying-to-azure-app-service-on-linux-from-visual-studio-code"></a>7: Visual Studio Code에서 Linux의 Azure App Service에 배포 후 리소스 정리

[이전 단계: 로그 스트리밍](tutorial-deploy-app-service-on-linux-06.md)

사용자가 만든 Azure App Service에는 비용을 발생시킬 수 있는 백업 App Service 계획이 포함됩니다. 이러한 비용을 방지하려면 모든 리소스가 포함된 리소스 그룹을 삭제합니다.

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>다음 단계

Linux의 App Service에 Python 코드를 배포하는 연습을 완료했습니다.

앞에서 설명한 것처럼 해당 GitHub 리포지토리 [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice)를 방문하여 App Service 확장에 대해 자세히 알아볼 수 있습니다. 문제 및 기여도 환영합니다.

AI 및 Machine Learning Service와 함께 데이터 스토리지를 포함하여 Python에서 사용할 수 있는 Azure 서비스에 대해 자세히 알아보려면 [Azure Python 개발자 센터](https://docs.microsoft.com/python/azure/?view=azure-python)를 참조하세요.

도움이 될 수 있는 VS Code에 대한 다른 Azure 확장도 있습니다. 확장 탐색기에서 "Azure"를 검색하기만 하면 됩니다.

![Visual Studio Code용 Azure 확장](media/deploy-containers/azure-extensions-for-visual-studio-code.png)

몇 가지 인기 있는 확장은 다음과 같습니다.

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure CLI 도구](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [ARM(Azure Resource Manager) 도구](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [완료했습니다!](https://docs.microsoft.com/python/azure/?view=azure-python) 

[문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=07-clean-up-resources)
