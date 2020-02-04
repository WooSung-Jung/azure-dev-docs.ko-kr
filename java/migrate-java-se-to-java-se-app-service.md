---
title: Azure App Service에서 Java SE 애플리케이션을 Java SE로 마이그레이션
description: 이 가이드에서는 Java SE를 사용하여 기존 Spring Boot 또는 다른 표준 웹 애플리케이션을 Azure App Service로 마이그레이션하려는 경우에 알아야 할 사항을 설명합니다.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: 5a9f6838e516b6168be40c83ea1ff4329676e6e3
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830711"
---
# <a name="migrate-executable-jar-web-applications-to-java-se-on-azure-app-service"></a>실행 가능 JAR 웹 애플리케이션을 Azure App Service의 Java SE로 마이그레이션

이 가이드에서는 Java SE를 사용하여 기존 Spring Boot 또는 다른 포함된 서버 웹 애플리케이션을 Azure App Service로 마이그레이션하려는 경우에 알아야 할 사항을 설명합니다.

## <a name="before-you-start"></a>시작하기 전에

마이그레이션 전 요구 사항을 충족할 수 없는 경우 다음 마이그레이션 가이드를 참조하세요.

* 실행 가능 JAR 애플리케이션을 Azure Kubernetes Service의 컨테이너로 마이그레이션(예정)
* 실행 가능 JAR 애플리케이션을 Azure Virtual Machines로 마이그레이션(예정)

## <a name="pre-migration-steps"></a>마이그레이션 전 단계

### <a name="switch-to-a-supported-platform"></a>지원되는 플랫폼으로 전환

App Service는 특정 버전의 Java SE를 제공합니다. 호환성을 보장하기 위해, 애플리케이션을 현재 환경에서 지원되는 버전 중 하나로 마이그레이션한 후 나머지 단계를 진행하세요. 결과 구성을 완전히 테스트해야 합니다. 테스트에서 Linux 배포판의 안정적인 최신 릴리스를 사용하세요.

[!INCLUDE [note-obtain-your-current-java-version](includes/migration/note-obtain-your-current-java-version.md)]

### <a name="inventory-external-resources"></a>인벤토리 외부 리소스

데이터 원본, JMS 메시지 브로커, 다른 서비스의 URL 등과 같은 외부 리소스를 확인합니다. Spring Boot 애플리케이션에서는 일반적으로 *application.properties* 또는 *application.yml* 파일의 *src/main/directory*에서 이러한 리소스의 구성을 찾을 수 있습니다. 또한 프로덕션 배포의 환경 변수에서 관련 구성 설정을 확인합니다.

#### <a name="databases"></a>데이터베이스

SQL 데이터베이스의 연결 문자열을 확인합니다.

Spring Boot 애플리케이션의 경우 연결 문자열은 일반적으로 구성 파일에 나타납니다. 

다음은 *application.properties* 파일의 예제입니다.

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mysql_db
spring.datasource.username=dbuser
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

다음은 *application.yaml* 파일의 예제입니다.

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://mongouser:deepsecret@mongoserver.contoso.com:27017
```

#### <a name="jms-message-brokers"></a>JMS 메시지 브로커

어떤 브로커가 사용되고 있는지 확인합니다. 관련 종속성에 대한 빌드 매니페스트(일반적으로 *pom.xml* 또는 *build.gradle*)를 검사하여 확인할 수 있습니다.

예를 들어 ActiveMQ를 사용하는 Spring Boot 애플리케이션은 일반적으로 다음과 같이 *pom.xml*에 이 종속성을 포함하고 있습니다.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

독점적 브로커를 사용하는 Spring Boot 애플리케이션은 일반적으로 브로커의 JMS 드라이버 라이브러리에 직접 종속성을 포함하고 있습니다. 다음은 *build.gradle* 파일의 예제입니다.

```json
    dependencies {
      ...
      compile("com.ibm.mq:com.ibm.mq.allclient:9.0.4.0")
      ...
    }
```

사용되는 브로커를 확인한 후에는 해당하는 설정을 찾습니다. 일반적으로 Spring Boot의 *application.properties* 및 *application.yml* 파일에 있습니다.

다음은 *application.properties* 파일의 예제입니다.

```properties
spring.activemq.brokerurl=broker:(tcp://localhost:61616,network:static:tcp://remotehost:61616)?persistent=false&useJmx=true
spring.activemq.user=admin
spring.activemq.password=tryandguess
```

다음은 *application.yaml* 파일의 예제입니다.

```yaml
ibm:
  mq:
    queueManager: qm1
    channel: dev.ORDERS
    connName: localhost(14)
    user: admin
    password: big$ecr3t
```

#### <a name="all-other-external-resources"></a>기타 모든 외부 리소스

이 가이드에서 가능한 모든 외부 종속성을 문서화하는 것은 불가능합니다. App Service 마이그레이션 후 애플리케이션의 모든 외부 종속성을 충족할 수 있는지 확인하는 것은 팀의 책임입니다.

### <a name="inventory-secrets"></a>인벤토리 비밀

#### <a name="passwords-and-secure-strings"></a>암호 및 보안 문자열

프로덕션 배포의 모든 속성 및 구성 파일과 모든 환경 변수에 비밀 문자열과 암호가 있는지 확인합니다. Spring Boot 애플리케이션에서 이러한 문자열은 *application.properties* 또는 *application.yml*에서 찾을 수 있습니다.

[!INCLUDE [inventory-certificates](includes/migration/inventory-certificates.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

### <a name="special-cases"></a>특수 사례

특정 프로덕션 시나리오에는 추가 변경이 필요하거나 추가 제한 사항이 있을 수 있습니다. 이러한 시나리오는 드물게 발생할 수 있지만, 애플리케이션에 적용할 수 없거나 올바르게 해결되었는지 확인해야 합니다.

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>애플리케이션에서 예약된 작업을 사용하는지 확인

Quartz 스케줄러 작업 또는 cron 작업과 같은 예약된 작업은 App Service에서 사용할 수 없습니다. App Service는 예약된 작업이 포함된 애플리케이션을 내부적으로 배포하는 것을 방지하지 않습니다. 그러나 애플리케이션이 확장되는 경우 동일한 예약된 작업이 예약된 기간마다 두 번 이상 실행될 수 있습니다. 이 경우 의도하지 않은 결과가 발생할 수 있습니다.

예약된 작업을 애플리케이션 프로세스 내부 또는 외부에 인벤토리화합니다.

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>OS 관련 코드가 애플리케이션에 포함되어 있는지 확인

애플리케이션이 실행되는 OS를 수용하는 코드가 애플리케이션에 포함되어 있는 경우 기본 OS를 사용하지 않도록 애플리케이션을 리팩터링해야 합니다. 예를 들어 파일 시스템 경로에서 `/` 또는 `\`를 사용하는 경우 [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) 또는 [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-)으로 바꿔야 할 수 있습니다.

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>프로덕션 서버에서 실행되는 모든 외부 프로세스/디먼 확인

디먼 모니터링과 같이 애플리케이션 서버 외부에서 실행되는 모든 프로세스를 다른 곳으로 마이그레이션하거나 제거해야 합니다.

#### <a name="identify-handling-of-non-http-requests-or-multiple-ports"></a>비 HTTP 요청 또는 여러 포트의 처리를 확인해야 합니다.

App Service는 단일 포트에서 단일 HTTP 엔드포인트만 지원합니다. 애플리케이션이 HTTP가 아닌 다른 프로토콜을 사용하여 여러 포트에서 수신 대기하거나 요청을 수락하는 경우 Azure App Service를 사용하지 마세요.

## <a name="migration"></a>마이그레이션

### <a name="parameterize-the-configuration"></a>구성 매개 변수화

모든 외부 리소스 좌표(예: 데이터베이스 연결 문자열) 및 기타 사용자 지정 가능 설정을 환경 변수에서 읽을 수 있는지 확인합니다. Spring Boot 애플리케이션을 마이그레이션하는 경우 모든 구성 설정이 이미 [외부화 가능](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)합니다.

다음은 *application.properties* 파일의 `SERVICEBUS_CONNECTION_STRING` 환경 변수를 참조하는 예제입니다.

```properties
spring.jms.servicebus.connection-string=${SERVICEBUS_CONNECTION_STRING}
spring.jms.servicebus.topic-client-id=contoso1
spring.jms.servicebus.idle-timeout=10000
```

### <a name="provision-an-app-service-plan"></a>App Service 계획 프로비저닝

[사용 가능한 서비스 계획 목록](https://azure.microsoft.com/pricing/details/app-service/linux/)에서 사양이 현재 프로덕션 하드웨어의 사양과 일치하거나 초과하는 계획을 선택합니다.

> [!NOTE]
> 스테이징/카나리아 배포를 실행하거나 [배포 슬롯](/azure/app-service/deploy-staging-slots)을 사용하려면 추가 용량이 App Service 계획에 포함되어야 합니다. Java 애플리케이션에는 프리미엄 이상의 계획을 사용하는 것이 좋습니다.

[App Service 계획을 만듭니다](/azure/app-service/app-service-plan-manage#create-an-app-service-plan).

### <a name="create-and-deploy-web-apps"></a>웹앱 만들기 및 배포

실행하려는 모든 실행 가능 JAR 파일의 App Service 계획에 대한 웹앱을 만들어야 합니다(런타임 스택으로 "Java SE" 선택).

#### <a name="maven-applications"></a>Maven 애플리케이션

애플리케이션이 Maven POM 파일에서 빌드되는 경우 [Maven용 Webapp 플러그 인을 사용](/azure/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin#configure-maven-plugin-for-azure-app-service)하여 Web App을 만들고 애플리케이션을 배포합니다.

#### <a name="non-maven-applications"></a>비 Maven 애플리케이션

Maven 플러그 인을 사용할 수 없는 경우 다음과 같은 다른 메커니즘을 통해 Web App을 프로비저닝해야 합니다.

* [Azure Portal](https://portal.azure.com/#create/Microsoft.WebSite)
* [Azure CLI](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

Web App이 만들어지면 [사용 가능한 배포 메커니즘](/azure/app-service/deploy-ftp) 중 하나를 사용하여 애플리케이션을 배포합니다. 가능하다면 애플리케이션을 */home/site/wwwroot/app.jar*에 업로드해야 합니다. JAR 이름을 app.jar로 변경하지 않으려는 경우에는 JAR을 실행하는 명령을 사용하여 셸 스크립트를 업로드하면 됩니다. 그런 다음, 포털의 구성 섹션에 있는 [시작 파일](/azure/app-service/containers/app-service-linux-faq#built-in-images) 텍스트 상자에 이 스크립트의 전체 경로를 붙여넣습니다. 시작 스크립트는 배치된 디렉터리에서 실행되지 않습니다. 따라서 항상 절대 경로를 사용하여 시작 스크립트의 파일을 참조해야 합니다(예: `java -jar /home/myapp/myapp.jar`).

### <a name="migrate-jvm-runtime-options"></a>JVM 런타임 옵션 마이그레이션

애플리케이션에 특정 런타임 옵션이 필요한 경우 [가장 적합한 메커니즘을 사용하여 해당 옵션을 지정](/azure/app-service/containers/configure-language-java#set-java-runtime-options)합니다.

[!INCLUDE [configure-custom-domain-and-ssl](includes/migration/configure-custom-domain-and-ssl.md)]

[!INCLUDE [import-backend-certificates](includes/migration/import-backend-certificates.md)]

### <a name="migrate-external-resource-coordinates-and-other-settings"></a>외부 리소스 좌표 및 기타 설정 마이그레이션

[다음 단계에 따라 연결 문자열 및 기타 설정 마이그레이션](/azure/app-service/containers/configure-language-java#spring-boot-1)을 따릅니다.

> [!NOTE]
> [구성 매개 변수화](#parameterize-the-configuration) 섹션의 변수를 사용하여 매개 변수화된 Spring Boot 애플리케이션 설정의 경우에는 애플리케이션 구성에서 이러한 환경 변수를 정의해야 합니다. 환경 변수를 사용하여 명시적으로 매개 변수화되지 않은 Spring Boot 애플리케이션 설정은 애플리케이션 구성을 통해 재정의할 수 있습니다. 다음은 그 예입니다.

  ```properties
  spring.jms.servicebus.connection-string=${CUSTOMCONNSTR_SERVICE_BUS}
  spring.jms.servicebus.topic-client-id=contoso1
  spring.jms.servicebus.idle-timeout=1800000
  ```

![App Service 애플리케이션 구성](media/migrate-java-se-to-java-se-app-service/app-service-parameterized-spring-boot-app-settings.png)

[!INCLUDE [migrate-scheduled-jobs](includes/migration/migrate-scheduled-jobs.md)]

### <a name="restart-and-smoke-test"></a>다시 시작 및 스모크 테스트

마지막으로, Web App을 다시 시작하여 모든 구성 변경 내용을 적용해야 합니다. 다시 시작이 완료되면 애플리케이션이 올바르게 실행되고 있는지 확인합니다.

## <a name="post-migration-steps"></a>마이그레이션 후 단계

이제 애플리케이션이 Azure App Service로 마이그레이션되었으므로 예상대로 작동하는지 확인해야 합니다. 이 작업이 완료되면 애플리케이션을 클라우드 네이티브로 만들 수 있는 몇 가지 추천 사항이 있습니다.

### <a name="recommendations"></a>권장 사항

* */home* 디렉터리를 파일 스토리지에 사용하도록 선택한 경우 [이를 Azure Storage로 바꾸는 것](/azure/app-service/containers/how-to-serve-content-from-azure-storage)이 좋습니다.

* 연결 문자열, SSL 키 및 기타 비밀 정보가 포함된 */home* 디렉터리에 구성이 있는 경우 가능하다면 [Azure Key Vault](/azure/app-service/app-service-key-vault-references) 및/또는 [애플리케이션 설정을 통한 매개 변수 주입](/azure/app-service/configure-common#configure-app-settings)을 사용하는 방안을 고려해 보세요.

* 가동 중지 시간이 없는 안정적인 배포를 위해 [배포 슬롯을 사용](/azure/app-service/deploy-staging-slots)하는 것이 좋습니다.

* DevOps 전략을 설계하고 구현합니다. 개발 속도를 높이는 동시에 안정성을 유지하기 위해 [Azure Pipelines를 사용하여 배포를 자동화하고 테스트](/azure/devops/pipelines/ecosystems/java-webapp)하는 것이 좋습니다. 배포 슬롯을 사용하는 경우 [슬롯 및 후속 슬롯 전환에 대한 배포를 자동화](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot)할 수 있습니다.

* 비즈니스 연속성 및 재해 복구 전략을 설계하고 구현합니다. 중요 업무용 애플리케이션의 경우 [다중 지역 배포 아키텍처](/azure/architecture/reference-architectures/app-service-web-app/multi-region)를 고려하세요.
