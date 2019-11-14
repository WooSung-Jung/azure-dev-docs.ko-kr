---
title: Azure Database for MySQL을 사용하여 App Service에 Spring/Tomcat 앱 배포
description: MySQL을 사용하는 Java App Service에 대한 엔드투엔드 자습서
author: KarlErickson
manager: barbkess
ms.author: karler
ms.date: 11/12/2019
ms.service: app-service
ms.devlang: java
ms.topic: article
ms.openlocfilehash: 2f5fc593733ea1f90d87ef9fbf98d6656410fa66
ms.sourcegitcommit: 25cef39b178a175822bf29f28fb2658bb8df8c59
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74040536"
---
# <a name="deploy-a-spring-app-to-app-service-with-mysql"></a>MySQL을 사용하여 App Service에 Spring 앱 배포

이 자습서에서는 App Service Linux에서 Java 웹앱을 빌드, 구성, 배포, 문제 해결 및 확장하는 프로세스를 단계별로 안내합니다.

이 자습서는 인기 있는 Spring PetClinic 샘플 앱을 기반으로 합니다. 이 항목에서는 앱의 HSQLDB 버전을 로컬에서 테스트한 다음, [Azure App Service](/azure/app-service/containers)에 배포합니다. 그런 다음, [Azure Database for MySQL](/azure/mysql)을 사용하는 버전을 구성하고 배포합니다. 마지막으로, 앱 로그에 액세스하고 앱을 실행하는 작업자 수를 늘려 규모를 확장하는 방법을 알아봅니다.

## <a name="prerequisites"></a>필수 조건

* [Azure CLI](https://docs.microsoft.com/cli/azure/overview)
* [Java 8](http://java.oracle.com/)
* [Maven 3](http://maven.apache.org/)
* [Git](https://github.com/)
* [Tomcat](https://tomcat.apache.org/download-80.cgi)
* [MySQL CLI](http://dev.mysql.com/downloads/mysql/)

## <a name="get-the-sample"></a>샘플 받기

샘플 앱을 시작하려면 다음 명령을 사용하여 원본 리포지토리를 복제하고 준비합니다.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux.git
cd e2e-java-experience-in-app-service-linux
yes | cp -rf .prep/* .
```

## <a name="build-and-run-the-hsqldb-sample-locally"></a>로컬에서 HSQLDB 샘플 빌드 및 실행

먼저 HSQLDB를 데이터베이스로 사용하여 샘플을 로컬에서 테스트합니다.

샘플의 HSQLDB 버전으로 이동한 다음, 빌드합니다.

```bash
cd initial-hsqldb/spring-framework-petclinic
mvn package
```

그런 다음, TOMCAT_HOME 환경 변수를 Tomcat 설치 위치로 설정합니다.

```bash
export TOMCAT_HOME=<Tomcat install directory>
```

그런 다음, *pom.xml* 파일을 업데이트하여 Tomcat WAR 파일 배포용 Maven을 구성합니다. 다음 XML을 기존 `<plugins>` 요소의 자식으로 추가합니다. 필요한 경우 `1.7.7`을 [Cargo Maven 2 플러그 인](https://mvnrepository.com/artifact/org.codehaus.cargo/cargo-maven2-plugin)의 최신 버전으로 변경합니다.

```xml
<plugin>
    <groupId>org.codehaus.cargo</groupId>
    <artifactId>cargo-maven2-plugin</artifactId>
    <version>1.7.7</version>
    <configuration>
        <container>
            <containerId>tomcat8x</containerId>
            <type>installed</type>
            <home>${TOMCAT_HOME}</home>
        </container>
        <configuration>
            <type>existing</type>
            <home>${TOMCAT_HOME}</home>
        </configuration>
        <deployables>
            <deployable>
                <groupId>${project.groupId}</groupId>
                <artifactId>${project.artifactId}</artifactId>
                <type>war</type>
                <properties>
                    <context>/</context>
                </properties>
            </deployable>
        </deployables>
    </configuration>
</plugin>
```

이 구성이 설정되면 로컬에서 Tomcat에 앱을 배포할 수 있습니다.

```bash
mvn cargo:deploy
```

그런 다음, Tomcat을 시작합니다.

```bash
${TOMCAT_HOME}/bin/catalina.sh run
```

이제 브라우저에서 [http://localhost:8080](http://localhost:8080)으로 이동하여 실행 중인 앱을 보고 어떻게 작동하는지 직접 확인할 수 있습니다. 완료되면 Bash 프롬프트에서 Ctrl+C를 선택하여 Tomcat을 중지합니다.

## <a name="deploy-to-azure-app-service"></a>Azure App Service에 배포

로컬에서 실행되는 모습을 살펴보았으므로 이제 앱을 Azure에 배포합니다.

먼저 다음 환경 변수를 설정합니다.

```bash
export RESOURCEGROUP_NAME=<resource group>
export WEBAPP_NAME=<web app>
export WEBAPP_PLAN_NAME=${WEBAPP_NAME}-appservice-plan
export REGION=<region>
```

Maven은 이러한 값을 사용하여 사용자가 제공하는 이름으로 Azure 리소스를 만듭니다. 환경 변수를 사용하여 프로젝트 파일에서 계정 비밀을 안전하게 유지할 수 있습니다.

그런 다음, *pom.xml* 파일을 업데이트하여 Azure 배포용 Maven을 구성합니다. 이전에 추가한 `<plugin>` 요소 뒤에 다음 XML을 추가합니다. 필요한 경우 `1.7.0`을 [Azure App Service용 Maven 플러그 인](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)의 최신 버전으로 변경합니다.

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.8.0</version>
    <configuration>
        <schemaVersion>v2</schemaVersion>
        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>
        <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>            
            <webContainer>TOMCAT 8.5</webContainer>
        </runtime>
    </configuration>
</plugin>
```

그런 다음, Azure에 로그인합니다.

```azurecli
az login
```

그런 다음, Azure App Service에 앱을 배포합니다.

```bash
mvn azure-webapp:deploy
```

이제 `https://<app-name>.azurewebsites.net`으로 이동하여 실행 중인 앱을 확인합니다. 여기서 `<app-name>`은 적절한 이름으로 바꿉니다.

## <a name="set-up-azure-database-for-mysql"></a>Azure Database for MySQL 설정

그런 다음, HSQLDB 대신 MySQL을 사용하도록 전환합니다. Azure에서 MySQL 서버 인스턴스를 만들고 데이터베이스를 추가한 다음, 새 데이터베이스 연결 정보를 사용하여 앱 구성을 업데이트합니다.

먼저 이후 단계에서 사용할 다음 환경 변수를 설정합니다.

```bash
export MYSQL_SERVER_NAME=<server>
export MYSQL_SERVER_FULL_NAME=${MYSQL_SERVER_NAME}.mysql.database.azure.com
export MYSQL_SERVER_ADMIN_LOGIN_NAME=<admin>
export MYSQL_SERVER_ADMIN_PASSWORD=<password>
export MYSQL_DATABASE_NAME=<database>
export DOLLAR=\$
```

그런 다음, 데이터베이스 서버를 만들고 초기화합니다. 초기 구성에는 [az mysql up](/cli/azure/ext/db-up/mysql?view=azure-cli-latest#ext-db-up-az-mysql-up)을 사용합니다. 그런 다음, [az mysql server configuration set](/cli/azure/mysql/server/configuration?view=azure-cli-latest#az-mysql-server-configuration-set)을 사용하여 연결 제한 시간을 늘리고 서버 표준 시간대를 설정합니다.

```azurecli
az mysql up \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server-name ${MYSQL_SERVER_NAME} \
    --database-name ${MYSQL_DATABASE_NAME} \
    --admin-user ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
    --admin-password ${MYSQL_SERVER_ADMIN_PASSWORD}

az mysql server configuration set --name wait_timeout \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value 2147483

az mysql server configuration set --name time_zone \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value -8:00
```

그런 다음, MySQL CLI를 사용하여 데이터베이스를 만듭니다.

```bash
mysql -u ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
 -h ${MYSQL_SERVER_FULL_NAME} -P 3306 -p
```

MySQL CLI 프롬프트에서 다음 명령을 실행합니다. 여기서 `<database name>`은 앞서 `MYSQL_DATABASE_NAME` 환경 변수에 지정한 것과 동일한 값으로 바꿉니다.

```console
CREATE DATABASE <database name>;
```

이제 MySQL을 사용할 준비가 되었습니다.

## <a name="configure-the-app-for-mysql"></a>MySQL용 앱 구성

다음으로, 앱의 MySQL 버전에 연결 정보를 추가한 후 App Service에 배포합니다.

먼저 Bash 프롬프트에서 올바른 폴더로 이동합니다.

```bash
cd ../../initial-mysql/spring-framework-petclinic
```

그런 다음, *pom.xml* 파일을 업데이트하여 MySQL을 활성 구성으로 만듭니다. 다음과 같이 HSQLDB 프로필에서 `<activation>` 요소를 제거하고 MySQL 프로필에 배치합니다. 코드 조각의 나머지 부분에는 기존 구성이 표시됩니다. Maven에서 이전에 설정된 환경 변수를 사용하여 MySQL 액세스를 구성하는 방법을 확인합니다.

```xml
<profile>
    <id>MySQL</id>
    <activation>
        <activeByDefault>true</activeByDefault>
    </activation>
    <properties>
        <db.script>mysql</db.script>
        <jpa.database>MYSQL</jpa.database>
        <jdbc.driverClassName>com.mysql.jdbc.Driver</jdbc.driverClassName>
        <jdbc.url>jdbc:mysql://${DOLLAR}{MYSQL_SERVER_FULL_NAME}:3306/${DOLLAR}{MYSQL_DATABASE_NAME}?useUnicode=true</jdbc.url>
        <jdbc.username>${DOLLAR}{MYSQL_SERVER_ADMIN_LOGIN_NAME}</jdbc.username>
        <jdbc.password>${DOLLAR}{MYSQL_SERVER_ADMIN_PASSWORD}</jdbc.password>
    </properties>
    ...
</profile>
```

그런 다음, *pom.xml* 파일을 업데이트하여 Azure 배포 및 MySQL용 Maven을 구성합니다. 이전에 추가한 `<plugin>` 요소 뒤에 다음 XML을 추가합니다. 필요한 경우 `1.7.0`을 [Azure App Service용 Maven 플러그 인](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)의 최신 버전으로 변경합니다.

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.7.0</version>
    <configuration>

        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>

        <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>

        <appSettings>
            <property>
                <name>MYSQL_SERVER_FULL_NAME</name>
                <value>${MYSQL_SERVER_FULL_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_LOGIN_NAME</name>
                <value>${MYSQL_SERVER_ADMIN_LOGIN_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_PASSWORD</name>
                <value>${MYSQL_SERVER_ADMIN_PASSWORD}</value>
            </property>
            <property>
                <name>MYSQL_DATABASE_NAME</name>
                <value>${MYSQL_DATABASE_NAME}</value>
            </property>
        </appSettings>

    </configuration>
</plugin>
```

그런 다음, 앱을 빌드한 후 Tomcat을 사용하여 앱을 배포하고 실행하는 방법으로 로컬에서 테스트합니다.

```bash
mvn package
mvn cargo:deploy
${TOMCAT_HOME}/bin/catalina.sh run
```

이제 [http://localhost:8080](http://localhost:8080)에서 로컬로 앱을 볼 수 있습니다. 앱은 이전과 동일한 모습으로 동일하게 작동하지만 HSQLDB 대신 Azure Database for MySQL을 사용합니다. 완료되면 Bash 프롬프트에서 Ctrl+C를 선택하여 Tomcat을 중지합니다.

마지막으로, App Service에 앱을 배포합니다.

```bash
mvn azure-webapp:deploy
```

이제 `https://<app-name>.azurewebsites.net`으로 이동하여 App Service 및 Azure Database for MySQL을 사용하여 실행 중인 앱을 확인할 수 있습니다.

## <a name="access-the-app-logs"></a>앱 로그 액세스

문제를 해결해야 하는 경우 앱 로그를 확인할 수 있습니다. 로컬 머신에서 원격 로그 스트림을 열려면 다음 명령을 사용합니다.

```azurecli
az webapp log tail --name ${WEBAPP_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

로그 보기를 마쳤으면 Ctrl+C를 선택하여 스트림을 중지합니다.

로그 스트림은 `https://<app-name>.scm.azurewebsites.net/api/logstream`에서도 볼 수 있습니다.

## <a name="scale-out"></a>확장

앱에 대규모 트래픽을 지원하려면 다음 명령을 사용하여 여러 인스턴스로 규모를 확장할 수 있습니다.

```azurecli
az appservice plan update --number-of-workers 2 \
    --name ${WEBAPP_PLAN_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

축하합니다! Spring Framework, JSP, Spring Data, Hibernate, JDBC, App Service Linux 및 Azure Database for MySQL을 사용하여 Java 웹앱을 빌드하고 확장했습니다.

## <a name="clean-up-resources"></a>리소스 정리

이전 섹션에서는 리소스 그룹에 Azure 리소스를 만들었습니다. 나중에 이러한 리소스를 사용하지 않을 것으로 생각되는 경우 다음 명령을 실행하여 리소스 그룹을 삭제합니다.

```azurecli
az group delete --name ${RESOURCEGROUP_NAME}
```

## <a name="next-steps"></a>다음 단계

다음으로, App Service에서 Java에 사용할 수 있는 기타 구성 및 CI/CD 옵션을 확인합니다.

> [!div class="nextstepaction"]
> [Azure App Service용 Linux Java 앱 구성](/azure/app-service/containers/configure-language-java)
> [!div class="nextstepaction"]
> [Azure Pipelines를 사용하여 Java 웹앱에 빌드 및 배포](/azure/devops/pipelines/ecosystems/java-webapp?view=azure-devops&tabs=java-tomcat)
> [!div class="nextstepaction"]
> [Jenkins 플러그 인을 사용하여 Azure App Service에 배포](/azure/jenkins/deploy-jenkins-app-service-plugin)
