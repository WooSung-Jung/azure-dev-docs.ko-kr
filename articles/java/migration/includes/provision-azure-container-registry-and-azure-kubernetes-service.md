---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: e72cefaab3ccdbbaae01992cc11285944fb09a36
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673209"
---
### <a name="provision-azure-container-registry-and-azure-kubernetes-service"></a>Azure Container Registry 및 Azure Kubernetes Service 프로비저닝

다음 명령을 사용하여 서비스 주체가 레지스트리에서 읽기 권한자 역할을 갖는 컨테이너 레지스트리와 Azure Kubernetes 클러스터를 만듭니다. 클러스터의 네트워킹 요구 사항에 [적합한 네트워크 모델을 선택](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model)해야 합니다.

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```
