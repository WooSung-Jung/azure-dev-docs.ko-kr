---
title: Azure CLI 제품 간의 차이점
description: Azure CLI 제품 명명, 버전 지정 및 업그레이드 방법을 이해합니다.
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 3cd61d2166d03f7b9d58db1ee1cee77d17b5b336
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030835"
---
# <a name="differences-between-azure-cli-products"></a>Azure CLI 제품 간의 차이점

2018년 6월 말을 기준으로 Azure CLI 제품 이름에서 명시적 버전 번호가 제거되었습니다. 이 변경은 사용자가 "Azure CLI"를 사용하라는 지시를 받았을 때 설명서에 나타났던 혼동을 제거하는 데 도움이 되지만 참조되는 제품의 버전이 명확하지 않습니다. 이전 제품 이름에 익숙한 경우, 변경된 내용은 다음과 같습니다.

* Azure CLI 버전 2.0 이상은 이제 "Azure CLI"라고만 참조됩니다.
* 이전 Azure CLI 버전(1.x 이하)은 이제 "Azure 클래식 CLI"로 참조됩니다.

클래식 Azure CLI로 이름이 변경된 것은 이 도구는 클래식 배포 모델에만 사용될 것을 명시하는 것입니다. 클래식 CLI는 또한 더 이상 업데이트되거나 유지 관리되지 않습니다. 이러한 이유로, 클래식 배포를 이동하여 Azure Resource Manager 모델을 사용하고 사용 가능한 최신 버전의 Azure CLI로 마이그레이션하는 것이 좋습니다.

여전히 클래식 CLI를 사용할 경우, 다음 문서에서 마이그레이션하는 프로세스에 대해 알아볼 수 있습니다.

* [클래식에서 Azure Resource Manager로 마이그레이션](/azure/virtual-machines/linux/migration-classic-resource-manager-overview)
* [Azure CLI 설치](install-azure-cli.md)
* [클래식 Azure CLI에서 Azure CLI로 마이그레이션](https://github.com/Azure/azure-cli/blob/dev/doc/classic_cli_migration.md)
