---
title: WildFly 애플리케이션을 Azure Kubernetes Service의 WildFly로 마이그레이션
description: 이 가이드에서는 Azure Kubernetes Service 컨테이너의 WildFly에서 실행되도록 기존 WildFly 애플리케이션을 마이그레이션하려는 경우 알아야 할 사항에 대해 설명합니다.
author: mriem
ms.author: manriem
ms.topic: conceptual
ms.date: 3/16/2020
ms.openlocfilehash: 401da674402173a434a511540c64faccc06bbb3b
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673299"
---
# <a name="migrate-wildfly-applications-to-wildfly-on-azure-kubernetes-service"></a>WildFly 애플리케이션을 Azure Kubernetes Service의 WildFly로 마이그레이션

이 가이드에서는 Azure Kubernetes Service 컨테이너의 WildFly에서 실행되도록 기존 WildFly 애플리케이션을 마이그레이션하려는 경우 알아야 할 사항에 대해 설명합니다.

## <a name="pre-migration"></a>사전 마이그레이션

[!INCLUDE [inventory-server-capacity-aks](includes/inventory-server-capacity-aks.md)]

### <a name="inventory-all-secrets"></a>모든 비밀 인벤토리화

프로덕션 서버의 모든 속성 및 구성 파일에 비밀과 암호가 있는지 확인합니다. WAR에서 *jboss-web.xml* 파일을 확인합니다. 암호 또는 자격 증명이 포함된 구성 파일은 애플리케이션 내에서도 찾을 수 있습니다.

이러한 비밀은 Azure Key Vault에 저장하는 것이 좋습니다. 자세한 내용은 [Azure Key Vault 기본 개념](/azure/key-vault/basic-concepts)을 참조하세요.

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

### <a name="validate-that-the-supported-java-version-works-correctly"></a>지원되는 Java 버전이 제대로 작동하는지 확인

Azure Kubernetes Service에서 WildFly를 사용하려면 특정 버전의 Java가 필요합니다. 따라서 지원되는 버전을 사용하여 애플리케이션을 올바르게 실행할 수 있는지 확인해야 합니다. 현재 서버에서 지원되는 JDK(예: Oracle JDK 또는 IBM OpenJ9)를 사용하는 경우 이 유효성 검사가 특히 중요합니다.

현재 버전을 가져오려면 프로덕션 서버에 로그인하고 다음 명령을 실행합니다.

```bash
java -version
```

WildFly를 실행하는 데 사용할 버전에 대한 지침은 [요구 사항](http://docs.wildfly.org/19/Getting_Started_Guide.html#requirements)을 참조하세요.

### <a name="inventory-jndi-resources"></a>JNDI 리소스 인벤토리화

모든 JNDI 리소스를 인벤토리화합니다. JMS 메시지 브로커와 같은 일부 리소스에는 마이그레이션 또는 재구성이 필요할 수 있습니다.

### <a name="determine-whether-session-replication-is-used"></a>세션 복제가 사용되는지 확인

애플리케이션에서 세션 복제를 사용하는 경우 이 종속성을 제거하도록 애플리케이션을 변경해야 합니다.

#### <a name="inside-your-application"></a>애플리케이션 내부

*WEB-INF/jboss-web.xml* 및/또는 *WEB-INF/web.xml* 파일을 검사합니다.

### <a name="document-datasources"></a>데이터 원본 문서화

애플리케이션에서 데이터베이스를 사용하는 경우 다음 정보를 캡처해야 합니다.

* 데이터 원본 이름은 무엇인가요?
* 연결 풀 구성이란?
* JDBC 드라이버 JAR 파일은 어디에서 찾을 수 있나요?

자세한 내용은 WildFly 설명서의 [데이터 원본 구성](http://docs.wildfly.org/19/Admin_Guide.html#DataSource)을 참조하세요.

### <a name="determine-whether-and-how-the-file-system-is-used"></a>파일 시스템의 사용 여부 및 방법 확인

애플리케이션 서버에서 파일 시스템을 사용하려면 재구성하거나 드물게 아키텍처 변경이 필요합니다. 파일 시스템은 WildFly 모듈 또는 애플리케이션 코드에서 사용할 수 있습니다. 다음 섹션에서 설명하는 시나리오의 일부 또는 전부를 식별할 수 있습니다.

#### <a name="read-only-static-content"></a>읽기 전용 정적 콘텐츠

애플리케이션에서 현재 정적 콘텐츠를 제공하는 경우 이를 대체할 위치가 필요합니다. 정적 콘텐츠를 Azure Blob Storage로 이동하고 전역적으로 빠른 다운로드를 위해 Azure CDN을 추가하는 것이 좋습니다. 자세한 내용은 [Azure Storage에서 정적 웹 사이트 호스팅](/azure/storage/blobs/storage-blob-static-website) 및 [빠른 시작: Azure CDN과 Azure Storage 계정 통합](/azure/cdn/cdn-create-a-storage-account-with-cdn)을 참조하세요.

#### <a name="dynamically-published-static-content"></a>동적으로 게시된 정적 콘텐츠

애플리케이션이 애플리케이션에서 업로드/생성되었지만 생성 후 변경할 수 없는 정적 콘텐츠를 허용하는 경우, 위에서 설명한 대로 Azure Blob Storage 및 Azure CDN를 사용하여 업로드 및 CDN 새로 고침을 처리할 수 있습니다. [Azure Functions를 사용하여 정적 콘텐츠 업로드 및 CDN 사전 로드](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)에서 사용할 샘플 구현을 제공했습니다.

#### <a name="dynamic-or-internal-content"></a>동적 또는 내부 콘텐츠

애플리케이션에서 자주 쓰고 읽는 파일(예: 임시 데이터 파일) 또는 애플리케이션에만 표시되는 정적 파일의 경우 Azure Storage 공유를 영구 볼륨으로 탑재할 수 있습니다. 자세한 내용은 [Azure Kubernetes Service에서 Azure Files를 사용하여 영구 볼륨을 동적으로 만들어 사용](/azure/aks/azure-files-dynamic-pv)을 참조하세요.

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use](includes/determine-whether-jms-queues-or-topics-are-in-use.md)]

[!INCLUDE [determine-whether-your-application-uses-entity-beans](includes/determine-whether-your-application-uses-entity-beans.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-in-use-aks](includes/determine-whether-the-java-ee-application-client-feature-is-in-use-aks.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-ejb-timers-are-in-use](includes/determine-whether-ejb-timers-are-in-use.md)]

### <a name="determine-whether-jca-connectors-are-in-use"></a>JCA 커넥터가 사용되는지 확인

애플리케이션에서 JCA 커넥터를 사용하는 경우 WildFly에서 JCA 커넥터를 사용할 수 있는지 확인해야 합니다. JCA 구현이 WildFly에 연결되는 경우 해당 종속성을 제거하도록 애플리케이션을 리팩터링해야 합니다. 사용할 수 있는 경우 JAR을 서버 클래스 경로에 추가하고, 필요한 구성 파일을 WildFly 서버 디렉터리의 올바른 위치에 배치해야 합니다.

[!INCLUDE [determine-whether-jaas-is-in-use](includes/determine-whether-jaas-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-uses-a-resource-adapter](includes/determine-whether-your-application-uses-a-resource-adapter.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

### <a name="determine-whether-your-application-is-packaged-as-an-ear"></a>애플리케이션이 EAR로 패키징되는지 확인

애플리케이션이 EAR 파일로 패키지되는 경우 *application.xml* 파일을 검사하고 해당 구성을 캡처합니다.

> [!NOTE]
> AKS 리소스를 더 효율적으로 사용하기 위해 각 웹 애플리케이션의 크기를 독립적으로 조정하려면 EAR을 별도의 웹 애플리케이션으로 분리해야 합니다.

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [perform-in-place-testing](includes/perform-in-place-testing.md)]

## <a name="migration"></a>마이그레이션

[!INCLUDE [provision-azure-container-registry-and-azure-kubernetes-service](includes/provision-azure-container-registry-and-azure-kubernetes-service.md)]

[!INCLUDE [create-a-docker-image-for-wildfly](includes/create-a-docker-image-for-wildfly.md)]

[!INCLUDE [build-and-push-the-docker-image-to-azure-container-registry](includes/build-and-push-the-docker-image-to-azure-container-registry.md)]

[!INCLUDE [provision-a-public-ip-address](includes/provision-a-public-ip-address.md)]

[!INCLUDE [deploy-to-aks](includes/deploy-to-aks.md)]

### <a name="configure-persistent-storage"></a>영구 스토리지 구성

애플리케이션에 비휘발성 스토리지가 필요한 경우 하나 이상의 [영구 볼륨](/azure/aks/azure-disks-dynamic-pv)을 구성합니다.

[!INCLUDE [migrate-scheduled-jobs-aks](includes/migrate-scheduled-jobs-aks.md)]

## <a name="post-migration"></a>마이그레이션 후 작업

이제 애플리케이션을 Azure Kubernetes Service로 마이그레이션했으므로 예상대로 작동하는지 확인해야 합니다. 이 작업이 완료되면 애플리케이션을 클라우드 네이티브로 만들 수 있는 몇 가지 추천 사항이 있습니다.

[!INCLUDE [recommendations-wildfly-on-aks](includes/recommendations-wildfly-on-aks.md)]
