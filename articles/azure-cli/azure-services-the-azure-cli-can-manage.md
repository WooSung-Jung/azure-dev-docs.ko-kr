---
title: Azure CLI로 관리할 수 있는 Azure 서비스
description: Azure CLI 참조를 제공하는 서비스 링크, App Configuration, App Service, AD(Active Directory), Backup, Cognitive Search, Cosmos DB, Data Lake Storage, Database, MariaDB, MySQL, PostgreSQL, PostgreSQL, DevOps, DevTest Labs, DNS, Functions, IoT, IoT Central, IoT Edge, IoT Hub, Kubernetes Service(AKS), Lab Services, Machine Learning, Managed Applications, Private Link, Resource Manager, Spring Cloud, SQL Database, Batch, Cognitive Services, Container Instances, Container Registry, Data Lake Analytics, Event Grid, Event Hubs, HDInsight, Key Vault, Load Balancer, Managed Disks, Media Services, Notification Hubs, Service Bus, Service Fabric, 스토리지 계정, Traffic Manager, Virtual Machine Scale Sets, Virtual Network, Compute, 네트워킹, 사물 인터넷, 개발자 도구, 데이터베이스, 분석, 관리 및 거버넌스, 하이브리드, 스토리지, 보안, AI, AI + Machine Learning
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 03/01/2020
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: aaf9969030512668b22c1b5bbfbd2a9a7d7ae47d
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030965"
---
# <a name="azure-services-the-azure-cli-can-manage"></a>Azure CLI로 관리할 수 있는 Azure 서비스

Azure CLI는 여러 Azure 서비스에서 사용할 수 있으며, Azure 리소스를 만들고 관리하는 데 사용되는 유연하면서도 강력한 도구입니다.  이 문서에서는 Azure CLI에서 관리할 수 있는 Azure 서비스 목록을 제공합니다.

Azure CLI 참조는 다음 용도로 사용할 수 있습니다.  

| 인기 문서에 연결되는 링크가 있는 범주 | 서비스 전체 설명서
|-|-|
|[AI + Machine Learning](/cli/azure/popular-articles-using-the-azure-cli?#ai--machine-learning)| [Azure Machine Learning](/azure/machine-learning/)
||[Cognitive Services](/azure/cognitive-services/)
|[분석](/cli/azure/popular-articles-using-the-azure-cli?#analytics)|[Azure Data Lake Storage](/azure/storage/blobs/data-lake-storage-introduction/)
||[Data Lake Analytics](/azure/data-lake-analytics/)
||[Event Hubs](/azure/event-hubs/)
||[HDInsight](/azure/hdinsight/)
|[블록체인](popular-articles-using-the-azure-cli.md)|[Azure Cosmos DB](/azure/cosmos-db/)
|[컴퓨팅](/cli/azure/popular-articles-using-the-azure-cli?#compute)|[App Service](/azure/app-service/)
||[Azure Functions](/azure/azure-functions/)
||[AKS(Azure Kubernetes Service)](/azure/aks/)
||[Azure Spring Cloud](/azure/spring-cloud/)
||[Batch](/azure/batch/)
||[Container Instances](/azure/container-instances/)
||[Service Fabric](/azure/service-fabric/)
||[Virtual Machine Scale Sets](/azure/virtual-machine-scale-sets/)
|[컨테이너](popular-articles-using-the-azure-cli.md)|[Azure Functions](/azure/azure-functions/)
||[AKS(Azure Kubernetes Service)](/azure/aks/)
||[Container Instances](/azure/container-instances/)
||[컨테이너 레지스트리](/azure/container-registry/)
||[Service Fabric](/azure/service-fabric/)
|[데이터베이스](/cli/azure/popular-articles-using-the-azure-cli?#databases)|[Azure Cosmos DB](/azure/cosmos-db/)
||[Azure Database for MariaDB](/azure/mariadb/)
||[Azure Database for MySQL](/azure/mysql/)
||[Azure Database for PostgreSQL](/azure/postgresql/)
||[Azure SQL Database](/azure/sql-database/)
|[개발자 도구](/cli/azure/popular-articles-using-the-azure-cli?#developer-tools)|[App Configuration](/azure/azure-app-configuration/)
||[Azure DevOps](/azure/devops/)
||[Azure DevTest Labs](/azure/lab-services/)
||[Azure Lab Services](/azure/lab-services/classroom-labs/)
|[DevOps](/cli/azure/popular-articles-using-the-azure-cli?#developer-tools)|[Azure DevOps](/azure/devops/)
||[Azure DevTest Labs](/azure/lab-services/)
|[하이브리드.](/cli/azure/popular-articles-using-the-azure-cli?#hybrid)|[Azure AD(Active Directory)](/azure/active-directory/)
||[Azure Database for PostgreSQL](/azure/postgresql/)
||[Azure DevOps](/azure/devops/)
||[Azure IoT Edge](/azure/iot-edge/)
||[Azure SQL Database](/azure/sql-database/)
|[ID](popular-articles-using-the-azure-cli.md)|[Azure AD(Active Directory)](/azure/active-directory/)
|[통합](popular-articles-using-the-azure-cli.md)|[Event Grid](/azure/event-grid/)
||[Service Bus](/azure/service-bus/)
|[사물 인터넷](/cli/azure/popular-articles-using-the-azure-cli?#internet-of-things)|[Azure Cosmos DB](/azure/cosmos-db/)
||[Azure Functions](/azure/azure-functions/)
||[Azure IoT Central](/azure/iot-central/)
||[Azure IoT Edge](/azure/iot-edge/)
||[Azure IoT Hub](/azure/iot-hub/)
||[Azure Machine Learning](/azure/machine-learning/)
||[Event Grid](/azure/event-grid/)
|[관리 및 거버넌스](/cli/azure/popular-articles-using-the-azure-cli?#management-and-governance)|[Azure Backup](/azure/backup/)
||[Azure Managed Applications](/azure/azure-resource-manager/managed-applications/)
||[Azure 리소스 관리자](/azure/azure-resource-manager/)
||[Traffic Manager](/azure/traffic-manager/)
|[미디어](popular-articles-using-the-azure-cli.md)|[Media Services](/azure/media-services/)
|[Mobile](popular-articles-using-the-azure-cli.md)|[App Service](/azure/app-service/)
|[네트워킹](/cli/azure/popular-articles-using-the-azure-cli?#networking)|[Azure DNS](/azure/dns/)
||[Azure Private Link](/azure/private-link/)
||[Load Balancer](/azure/load-balancer/)
||[Traffic Manager](/azure/traffic-manager/)
||[Virtual Network](/azure/virtual-network/)
|[보안](/cli/azure/popular-articles-using-the-azure-cli?#security)|[Azure AD(Active Directory)](/azure/active-directory/)
||[Key Vault](/azure/key-vault/)
|[스토리지](/cli/azure/popular-articles-using-the-azure-cli?#storage)|[Azure Backup](/azure/backup/)
||[Azure Data Lake Storage](/azure/storage/blobs/data-lake-storage-introduction/)
||[Managed Disks](/azure/virtual-machines/windows/managed-disks-overview/)
||[Storage 계정](/azure/storage/common/storage-account-overview/)
|[Web](popular-articles-using-the-azure-cli.md)|[App Service](/azure/app-service/)
||[Azure Cognitive Search](/azure/search/)
||[Azure Spring Cloud](/azure/spring-cloud/)
||[Notification Hubs](/azure/notification-hubs/)

## <a name="see-also"></a>참고 항목

- [Azure CLI 시작](get-started-with-azure-cli.md)
- [Azure CLI의 전체 명령 참조 목록](/cli/azure/reference-index)
- [Azure CLI 사용에 대한 인기 문서](popular-articles-using-the-azure-cli.md)
