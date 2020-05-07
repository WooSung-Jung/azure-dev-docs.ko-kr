---
title: Azure CLI에 대해 사용 가능한 확장
description: Azure CLI에 대해 공식적으로 지원되는 확장의 전체 목록입니다.
author: haroldrandom
ms.author: jianzen
manager: yonzhan,yungezz
ms.date: 04/16/2020
ms.topic: article
ms.prod: azure
ms.technology: azure-cli
ms.devlang: azure-cli
ms.openlocfilehash: 531ea056b276cc12d5807ed62baa4d3c6044c26e
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030955"
---
# <a name="available-extensions-for-the-azure-cli"></a>Azure CLI에 대해 사용 가능한 확장

이 문서는 Microsoft에서 지원하는 Azure CLI에 사용할 수 있는 전체 확장 목록입니다.

확장 목록도 CLI로부터 가져올 수 있습니다. 가져오려면 [az extension list-available](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)을 실행합니다.

```azurecli-interactive
az extension list-available --output table
```

| 속성 | 버전 | 요약 | 미리 보기 |
|------|---------|---------|---------|
| [aem](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | SAP용 Azure 고급 모니터링 확장 관리 |  |
| [ai-examples](https://github.com/Azure/azure-cli-extensions/tree/master/src/ai-examples) | 0.2.0 | 도움말 콘텐츠에 AI 지원 예를 추가합니다. | yes |
| [aks-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview) | 0.4.43 | 예정된 AKS 기능에 대한 미리 보기 제공 | yes |
| [alertsmanagement](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 명령줄 도구 Alerts 확장 |  |
| [alias](https://github.com/Azure/azure-cli-extensions) | 0.5.2 | 명령 별칭 지원 | yes |
| [application-insights](https://github.com/Azure/azure-cli-extensions/tree/master/src/application-insights) | 0.1.6 | Application Insights 구성 요소 관리와 그러한 구성 요소에서 메트릭, 이벤트, 로그의 쿼리를 지원합니다. | yes |
| [azure-batch-cli-extensions](https://github.com/Azure/azure-batch-cli-extensions) | 5.0.1 | Azure Batch 서비스와 함께 사용하기 위한 추가 명령 |  |
| [azure-cli-iot-ext](https://github.com/azure/azure-iot-cli-extension) | 0.8.9 | Azure CLI용 Azure IoT 확장. |  |
| [azure-cli-ml](https://docs.microsoft.com/azure/machine-learning/service/) | 1.3.0 | Microsoft Azure 명령줄 도구 AzureML 명령 모듈 |  |
| [azure-devops](https://github.com/Microsoft/azure-devops-cli-extension) | 0.18.0 | Azure DevOps를 관리하는 도구입니다. |  |
| [azure-firewall](https://github.com/Azure/azure-cli-extensions/tree/master/src/azure-firewall) | 0.3.1 | Azure Firewall 리소스를 관리합니다. | yes |
| [azure-iot](https://github.com/azure/azure-iot-cli-extension) | 0.9.1 | Azure CLI용 Azure IoT 확장. |  |
| [청사진](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 명령줄 도구 청사진 확장 |  |
| [connectedmachine](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Microsoft Azure 명령줄 도구 Connectedmachine 확장 | yes |
| [connection-monitor-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/connection-monitor-preview) | 0.1.0 | Microsoft Azure 명령줄 연결 모니터 V2 확장 | yes |
| [csvmware](https://github.com/Azure/az-vmware-cli) | 0.3.0 | Azure VMware Solution by CloudSimple을 관리합니다. | yes |
| [databox](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 명령줄 도구 DataBox 확장 |  |
| [databricks](https://github.com/Azure/azure-cli-extensions) | 0.2.0 | Microsoft Azure 명령줄 도구 DatabricksClient 확장 |  |
| [db-up](https://github.com/Azure/azure-cli-extensions/tree/master/src/db-up) | 0.1.13 | Azure Database 워크플로를 간소화하는 추가 명령입니다. | yes |
| [deploy-to-azure](https://github.com/Azure/deploy-to-azure-cli-extension) | 0.2.0 | Github 작업을 사용하여 Azure에 배포합니다. | yes |
| [dev-spaces](https://github.com/Azure/azure-cli-extensions) | 1.0.5 | Dev Spaces는 팀에게 신속하고 반복적인 Kubernetes 개발 환경을 제공합니다. |  |
| [dev-spaces-preview](https://github.com/Azure/azure-cli-extensions) | 0.1.6 | Dev Spaces는 팀에게 신속하고 반복적인 Kubernetes 개발 환경을 제공합니다. | yes |
| [dms-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/dms-preview) | 0.11.0 | Database Migration Service의 새로운 시나리오에 대한 지원. | yes |
| [eventgrid](https://github.com/Azure/azure-cli-extensions) | 0.4.7 | Microsoft Azure 명령줄 도구 EventGrid 명령 모듈 | yes |
| [express-route](https://github.com/Azure/azure-cli-extensions/tree/master/src/express-route) | 0.1.3 | 미리 보기 기능을 사용하여 Expressroute를 관리합니다. | yes |
| [express-route-cross-connection](https://github.com/Azure/azure-cli-extensions/tree/master/src/express-route-cross-connection) | 0.1.1 | ExpressRoute 간 연결을 사용하여 고객의 ExpressRoute 회로를 관리 합니다. |  |
| [front-door](https://github.com/Azure/azure-cli-extensions/tree/master/src/front-door) | 1.0.6 | 네트워킹 Front Doors를 관리합니다. |  |
| [hack](https://github.com/Azure/azure-cli-extensions) | 0.4.2 | Microsoft Azure 명령줄 도구 Hack 확장 | yes |
| [healthcareapis](https://github.com/Azure/azure-cli-extensions) | 0.1.3 | Microsoft Azure 명령줄 도구 HealthCareApis 확장 |  |
| [hpc-cache](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 명령줄 도구 StorageCache 확장 | yes |
| [image-copy-extension](https://github.com/Azure/azure-cli-extensions) | 0.2.3 | 지역 간 관리 vm 이미지 복사 기능에 대한 지원 |  |
| [대화형](https://github.com/Azure/azure-cli) | 0.4.4 | Microsoft Azure 명령줄 대화형 셸 | yes |
| [internet-analyzer](https://github.com/Azure/azure-cli-extensions) | 0.1.0rc5 | Microsoft Azure 명령줄 도구 Internet Analyzer 확장 | yes |
| [ip-group](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Microsoft Azure 명령줄 도구 IpGroup 확장 |  |
| [keyvault-preview](https://github.com/Azure/azure-keyvault-cli-extension) | 0.1.3 | Azure Key Vault 미리보기 명령어 | yes |
| [log-analytics](https://github.com/Azure/azure-cli-extensions/tree/master/src/log-analytics) | 0.1.4 | Azure Log Analytics 쿼리 기능을 지원합니다. | yes |
| [유지 관리](https://github.com/Azure/azure-cli-extensions) | 1.0.1 | Azure 유지 보수 관리를 지원합니다. |  |
| [managementpartner](https://github.com/Azure/azure-cli-extensions) | 0.1.2 | 관리 파트너 미리 보기 지원 |  |
| [메시](https://github.com/Azure/azure-cli-extensions) | 0.10.6 | Microsoft Azure Service Fabric Mesh - 공개 미리 보기에 대한 지원 | yes |
| [mixed-reality](https://github.com/Azure/azure-cli-extensions) | 0.0.1 | Mixed Reality Azure CLI 확장. |  |
| [netappfiles-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/netappfiles-preview) | 0.3.2 | 예정된 ANF(Azure NetApp Files) 기능에 대한 미리보기를 제공합니다. | yes |
| [notification-hub](https://github.com/Azure/azure-cli-extensions) | 0.2.0 | Microsoft Azure 명령줄 도구 알림 허브 확장 |  |
| [peering](https://github.com/Azure/azure-cli-extensions) | 0.1.0rc2 | Microsoft Azure 명령줄 도구 피어링 확장 | yes |
| [powerbidedicated](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Microsoft Azure 명령줄 도구 PowerBIDedicated 확장 | yes |
| [privatedns](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | 프라이빗 DNS 영역을 관리하는 명령입니다. | yes |
| [resource-graph](https://github.com/Azure/azure-cli-extensions/tree/master/src/resource-graph) | 1.1.0 | Resource Graph로 Azure 리소스 쿼리에 대한 지원 | yes |
| [sap-hana](https://github.com/Azure/azure-hanaonazure-cli-extension) | 0.5.9 | SAP HanaOnAzure 인스턴스와 함께 사용하기 위한 추가 명령 |  |
| [spring-cloud](https://github.com/Azure/azure-cli-extensions) | 0.2.2 | Microsoft Azure 명령줄 도구 spring-cloud 확장 | yes |
| [storage-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview) | 0.2.10 | 예정된 스토리지 기능에 대한 미리 보기를 제공합니다. | yes |
| [storagesync](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 명령줄 도구 MicrosoftStorageSync 확장 |  |
| [stream-analytics](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 명령줄 도구 stream-analytics 확장 |  |
| [subscription](https://github.com/Azure/azure-cli-extensions) | 0.1.3 | 구독 관리 미리 보기를 지원합니다. |  |
| [support](https://github.com/azure/azure-cli-extensions/tree/master/src/support) | 1.0.1 | Microsoft Azure 명령줄 도구 지원 확장 |  |
| [synapse](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 명령줄 도구 Synapse 확장 | yes |
| [virtual-network-tap](https://github.com/Azure/azure-cli-extensions/tree/master/src/virtual-network-tap) | 0.1.0 | 가상 네트워크 관리 탭(VTAP) | yes |
| [virtual-wan](https://github.com/Azure/azure-cli-extensions/tree/master/src/virtual-wan) | 0.1.3 | 가상 WAN, 허브, VPN 게이트웨이 및 VPN 사이트를 관리합니다. | yes |
| [vm-repair](https://github.com/Azure/azure-cli-extensions/tree/master/src/vm-repair) | 0.2.6 | VM을 수정하기 위한 자동 복구 명령입니다. |  |
| [vmware](https://github.com/virtustream/azure-vmware-virtustream-cli-extension) | 0.5.5 | Virtustream 명령을 통해 Azure VMware 솔루션을 미리 봅니다. | yes |
| [webapp](https://github.com/Azure/azure-cli-extensions/tree/master/src/webapp) | 0.2.24 | Azure AppService에 대한 추가 명령입니다. | yes |