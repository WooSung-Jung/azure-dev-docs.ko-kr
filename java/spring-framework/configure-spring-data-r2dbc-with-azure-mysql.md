---
title: Azure Database for MySQL에서 Spring Data R2DBC를 사용하는 방법
description: Azure Database for MySQL 데이터베이스에서 Spring Data R2DBC를 사용하는 방법을 알아보세요.
documentationcenter: java
ms.date: 03/18/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.author: judubois
ms.openlocfilehash: f5c9f3d0cccad7f6e1a8b5ab199f841925f416cc
ms.sourcegitcommit: a65fa8dbb168bd39e225a293d9ee73d18ece1864
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366754"
---
# <a name="how-to-use-spring-data-r2dbc-with-azure-mysql"></a>Azure MySQL에서 Spring Data R2DBC를 사용하는 방법

이 항목에서는 [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc)를 사용하는 샘플 애플리케이션을 만들어 [https://github.com/mirromutth/r2dbc-mysql](https://github.com/mirromutth/r2dbc-mysql)의 MySQL용 R2DBC 구현을 사용하여 [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) 데이터베이스에서 정보를 저장 및 검색하는 것을 보여줍니다.

[R2DBC](https://r2dbc.io/)는 기존 관계형 데이터베이스에 반응형 API를 제공합니다. Spring WebFlux와 함께 사용하면 비블로킹 API를 사용하고 일반적인 "연결당 1개 스레드" 방법보다 뛰어난 확장성을 제공하는 완전 반응형 Spring Boot 애플리케이션을 만들 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

- Azure 계정. 계정이 없으면 [체험 계정을 얻습니다](https://azure.microsoft.com/free/).
- [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) 또는 [Azure CLI](/cli/azure/install-azure-cli). 자동으로 로그인되고 필요한 모든 도구에 액세스할 수 있는 Azure Cloud Shell을 권장합니다.
- [Java 8](https://www.azul.com/downloads/zulu/)(Azure Cloud Shell에 포함되어 있음)
- [cURL](https://curl.haxx.se) 또는 기능을 테스트하는 유사한 HTTP 유틸리티

## <a name="prepare-the-working-environment"></a>작업 환경 준비

먼저 다음 명령을 사용하여 몇 가지 환경 변수를 설정합니다.

```bash
AZ_RESOURCE_GROUP=r2dbc-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_MYSQL_USERNAME=r2dbc
AZ_MYSQL_PASSWORD=<YOUR_MYSQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

자리 표시자를 이 항목 전체에서 사용되는 다음 값으로 바꿉니다.

- `<YOUR_DATABASE_NAME>`: MySQL Server 인스턴스의 이름. Azure에서 고유해야 합니다.
- `<YOUR_AZURE_REGION>`: 사용할 Azure 지역. 기본적으로 `eastus`를 사용할 수 있지만 거주지와 더 가까운 지역을 구성하는 것이 좋습니다. `az account list-locations`를 입력하면 사용 가능한 지역의 전체 목록을 나열할 수 있습니다.
- `<YOUR_MYSQL_PASSWORD>`: MySQL 데이터베이스 서버의 암호. 이 암호는 8자 이상이어야 하며, 영어 대문자, 영어 소문자, 숫자(0-9) 및 영숫자가 아닌 문자(!, $, #, % 등) 중 세 가지 범주의 문자를 포함해야 합니다.
- `<YOUR_LOCAL_IP_ADDRESS>`: Spring Boot 애플리케이션을 실행할 로컬 컴퓨터의 IP 주소. 이를 확인하는 간편한 방법 중 하나는 브라우저에서 [http://ipv4.icanhazip.com](http://ipv4.icanhazip.com)으로 이동하는 것입니다.

다음으로, 리소스 그룹을 만듭니다.

```azurecli
az group create \
    --name $AZ_RESOURCE_GROUP \
    --location $AZ_LOCATION \
    | jq
```

> [!NOTE]
> [Azure Cloud Shell](https://shell.azure.com/)에 기본적으로 설치되는 `jq` 유틸리티를 사용하여 JSON 데이터를 표시하고 가독성을 높입니다.
> 이 유틸리티가 마음에 들지 않을 경우 여기서 사용하게 될 모든 명령의 `| jq` 부분을 안전하게 제거하면 됩니다.

## <a name="create-an-azure-database-for-mysql"></a>Azure Database for MySQL 만들기

먼저 관리형 MySQL Server 인스턴스를 만듭니다.

> [!NOTE]
> 
> MySQL 데이터베이스 만들기에 관한 자세한 정보는 [Azure portal을 사용하여 Azure Database for MySQL 서버 만들기](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)에서 확인할 수 있습니다.

[Azure Shell](https://shell.azure.com/) 인스턴스에서 다음 스크립트를 실행합니다.

```azurecli
az mysql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_MYSQL_USERNAME \
    --admin-password $AZ_MYSQL_PASSWORD \
    | jq
```

이 명령은 작은 MySQL Server 인스턴스를 만듭니다.

### <a name="configure-a-firewall-rule-for-your-mysql-server-instance"></a>MySQL Server 인스턴스의 방화벽 규칙 구성

Azure Database for MySQL 인스턴스는 기본적으로 보호됩니다. 즉, 들어오는 연결을 허용하지 않는 방화벽이 있습니다. 데이터베이스를 사용하려면 로컬 IP 주소에서 데이터베이스 서버에 액세스할 수 있도록 하는 방화벽 규칙을 추가해야 합니다.

이 문서의 시작 부분에서 로컬 IP 주소를 구성했으므로 다음을 실행하여 서버의 방화벽을 열 수 있습니다.

```azurecli
az mysql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-mysql-database"></a>MySQL 데이터베이스 구성

이전에 만든 MySQL 서버는 비어 있습니다. 이 서버에는 Spring Boot 애플리케이션과 함께 사용할 수 있는 데이터베이스가 없습니다. `r2dbc`라는 새 데이터베이스를 만듭니다.

```azurecli
az mysql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name r2dbc \
    --server-name $AZ_DATABASE_NAME \
    | jq
```

## <a name="create-a-reactive-spring-boot-application"></a>반응형 Spring Boot 애플리케이션 만들기

반응형 Spring Boot 애플리케이션을 만들기 위해 [Spring Initializr](https://start.spring.io/)를 사용합니다. 여기서 만들 애플리케이션은 다음을 사용합니다.

- Spring Boot 2.3.0 M3
- Java 8(단, Java 11과 같은 최신 버전과도 연동됨)
- 종속성: Spring Reactive Web("Spring WebFlux"라고도 함) 및 Spring Data R2DBC

### <a name="generate-the-application-using-spring-initializr"></a>Spring Initializr를 사용하여 애플리케이션 생성

명령줄에서 다음을 입력하여 이 애플리케이션을 생성합니다.

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-r2dbc-workshop -d bootVersion=2.3.0.M3 -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-mysql-driver-implementation"></a>반응형 MySQL 드라이버 구현 추가

생성된 프로젝트의 *pom .xml* 파일을 열어 [https://github.com/mirromutth/r2dbc-mysql](https://github.com/mirromutth/r2dbc-mysql)의 반응형 MySQL 드라이버를 추가합니다.

`spring-boot-starter-webflux` 종속성 다음에 아래와 같은 코드 조각을 추가합니다.

```xml
<dependency>
   <groupId>dev.miku</groupId>
   <artifactId>r2dbc-mysql</artifactId>
   <version>0.8.1.RELEASE</version>
   <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-the-azure-database-for-mysql"></a>Azure Database for MySQL을 사용하도록 Spring Boot 구성

*src/main/resources/application.properties* 파일을 열고 다음을 추가합니다.

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/r2dbc
spring.r2dbc.username=r2dbc@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_MYSQL_USERNAME
```

- 두 개의 `$AZ_DATABASE_NAME` 변수를 이 문서의 시작 부분에서 구성한 값으로 바꿉니다.
- `$AZ_MYSQL_USERNAME` 변수를 이 문서의 시작 부분에서 구성한 값으로 바꿉니다.

이제 제공된 Maven 래퍼를 사용하여 애플리케이션을 시작할 수 있습니다.

```bash
./mvnw spring-boot:run
```

처음으로 실행 중인 이 애플리케이션의 스크린샷은 다음과 같습니다.

![애플리케이션 실행][R2DBC-MYSQL01]

### <a name="create-the-database-schema"></a>데이터베이스 스키마 만들기

주 `DemoApplication` 클래스 내에서 사용할 데이터베이스 스키마를 만들 새 Spring 빈을 구성합니다.

```java
    @Bean
    public ConnectionFactoryInitializer initializer(ConnectionFactory connectionFactory) {
        ConnectionFactoryInitializer initializer = new ConnectionFactoryInitializer();
        initializer.setConnectionFactory(connectionFactory);
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator(new ClassPathResource("schema.sql"));
        initializer.setDatabasePopulator(populator);
        return initializer;
    }
```

이 Spring 빈은 *schema.sql*이라는 파일을 사용하므로 *src/main/resources* 폴더에 해당 파일을 만듭니다.

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

다음 명령을 사용하여 애플리케이션을 중지하고 다시 실행합니다. 이제 애플리케이션이 이전에 생성된 `r2dbc` 데이터베이스를 사용하고 내부에 `todo` 테이블을 만듭니다.

```bash
./mvnw spring-boot:run
```

생성되는 데이터베이스 테이블의 스크린샷은 다음과 같습니다.

   ![데이터베이스 테이블 만들기][R2DBC-MYSQL02]

## <a name="code-the-application"></a>애플리케이션 코딩

다음으로, R2DBC를 사용하여 MySQL Server 인스턴스에서 데이터를 저장하고 검색하는 Java 코드를 추가합니다.

이제 `DemoApplication` 클래스 옆에 새 `Todo` Java 클래스를 만듭니다.

```java
package com.example.demo;

import org.springframework.data.annotation.Id;

public class Todo {

    public Todo() {
    }

    public Todo(String description, String details, boolean done) {
        this.description = description;
        this.details = details;
        this.done = done;
    }

    @Id
    private Long id;

    private String description;

    private String details;

    private boolean done;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }
}
```

이 클래스는 이전에 만든 `todo` 테이블에 매핑된 도메인 모델입니다.

해당 클래스를 관리하려면 리포지토리가 필요합니다. 동일한 패키지에 새 `TodoRepository` 인터페이스를 정의합니다.

```java
package com.example.demo;

import org.springframework.data.repository.reactive.ReactiveCrudRepository;

public interface TodoRepository extends ReactiveCrudRepository<Todo, Long> {
}
```

이 리포지토리는 Spring Data R2DBC에서 관리하는 반응형 리포지토리입니다.

그런 다음, 데이터를 저장하고 검색할 수 있는 컨트롤러를 만들어 애플리케이션을 완성합니다. 동일한 패키지에 `TodoController` 클래스를 구현하고 다음 코드를 추가합니다.

```java
package com.example.demo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/")
public class TodoController {

    private final TodoRepository todoRepository;

    public TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<Todo> createTodo(@RequestBody Todo todo) {
        return todoRepository.save(todo);
    }

    @GetMapping("/")
    public Flux<Todo> getTodos() {
        return todoRepository.findAll();
    }
}
```

마지막으로, 애플리케이션을 중지하고 다시 시작합니다.

```bash
./mvnw spring-boot:run
```

## <a name="test-the-application"></a>애플리케이션 테스트

애플리케이션을 테스트할 때 cURL을 사용할 수 있습니다.

먼저 데이터베이스에 새 "todo" 항목을 만듭니다.

```bash
curl  --header "Content-Type: application/json" \
          --request POST \
          --data '{"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done": "true"}' \
          http://127.0.0.1:8080
```

이 명령은 생성된 항목을 반환합니다.

```json
{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}
```

다음으로, 새 cURL 요청을 사용하여 데이터를 검색합니다.

```bash
curl http://127.0.0.1:8080
```

이 명령은 생성된 항목을 포함한 "todos" 목록을 반환합니다.

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}]
```

이러한 cURL 요청의 스크린샷은 다음과 같습니다.

   ![cURL을 사용하여 테스트][R2DBC-MYSQL03]

축하합니다! R2DBC를 사용하여 Azure Database for MySQL에서 데이터를 저장하고 검색하는 완전한 반응형 Spring Boot 애플리케이션을 만들었습니다.

## <a name="clean-up-resources"></a>리소스 정리

이 빠른 시작에서 사용된 모든 리소스를 정리하려면 리소스 그룹을 삭제합니다.

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>다음 단계

Spring과 Azure에 대한 자세한 사항은 Azure의 Spring 설명서 센터를 참조합니다.

> [!div class="nextstepaction"]
> [Azure의 Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>추가 리소스

Spring Data R2DBC에 대한 자세한 내용은 Spring의 [참조 설명서](https://docs.spring.io/spring-data/r2dbc/docs/1.0.x/reference/html/#reference)를 참조하세요.

Java와 함께 Azure를 사용하는 방법에 관한 자세한 정보는 [Java 개발자를 위한 Azure](/azure/java/)와 [Azure DevOps 및 Java 사용하기](/azure/devops/) 페이지를 참조하세요.

<!-- IMG List -->

[R2DBC-MYSQL01]: media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-01.png
[R2DBC-MYSQL02]: media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-02.png
[R2DBC-MYSQL03]: media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-03.png
