---
title: Azure SQL Database를 통해 Spring Data JDBC를 사용하는 방법
description: Azure SQL Database를 통해 Spring Data JDBC를 사용하는 방법을 알아보세요.
services: sql-database
documentationcenter: java
ms.date: 12/19/2018
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: ccbc2c78877e5c687cd463e49b84475495368fa3
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81668799"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-sql-database"></a>Azure SQL Database를 통해 Spring Data JDBC를 사용하는 방법

## <a name="overview"></a>개요

이 문서는 [Spring Data]를 사용하는 샘플 애플리케이션을 만들어 [JDBC(Java Database Connectivity)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/)를 사용하여 [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)에서 정보를 저장 및 검색하는 것을 보여줍니다.

## <a name="prerequisites"></a>사전 요구 사항

이 문서의 단계를 완료하려면 다음 필수 구성 요소가 필요합니다.

* Azure 구독. Azure 구독이 아직 없는 경우 [MSDN 구독자 혜택]을 활성화하거나 [체험판 Azure 계정]에 등록할 수 있습니다.
* 지원되는 JDK(Java Development Kit) Azure에서 개발하는 경우 사용할 수 있는 JDK에 대한 자세한 내용은 <https://aka.ms/azure-jdks>를 참조하세요.
* [Apache Maven](http://maven.apache.org/), 버전 3.0 이상
* [Curl](https://curl.haxx.se/) 또는 기능을 테스트하는 유사한 HTTP 유틸리티.
* [Git](https://git-scm.com/downloads) 클라이언트

## <a name="create-an-azure-sql-database"></a>Azure SQL Database 만들기

### <a name="create-a-sql-database-server-using-the-azure-portal"></a>Azure Portal을 사용하여 SQL 데이터베이스 서버 만들기

> [!NOTE]
> 
> Azure SQL 데이터베이스 만들기에 관한 자세한 정보는 [Azure portal에서 Azure SQL 데이터베이스 만들기](/azure/sql-database/sql-database-get-started-portal)에서 확인할 수 있습니다.

1. <https://portal.azure.com/>에서 Azure Portal을 찾아 로그인합니다.

1. **+리소스 만들기**를 클릭한 다음 **데이터베이스**를 클릭하고 **SQL Database**를 클릭합니다.

   ![SQL 데이터베이스 만들기][SQL01]

1. 다음 정보를 지정합니다.

   * **데이터베이스 이름**: SQL 데이터베이스의 고유명을 선택합니다. 이 고유명은 나중에 지정할 SQL 서버에서 만들어집니다.
   * **구독**: 사용할 Azure 구독을 지정합니다.
   * **리소스 그룹**: 새 리소스 그룹을 만들지 기존 리소스 그룹을 선택할지를 지정합니다.
   * **원본 선택**: 이 자습서에서는 `Blank database`를 선택하여 새 데이터베이스를 만듭니다.

   ![SQL 데이터베이스 속성 지정하기][SQL02]
   
1. **서버**를 클릭한 다음, **새로 만들기**를 클릭하여 다음 정보를 지정합니다.

   - **서버 이름**: SQL 서버의 고유명을 선택합니다. 이 고유명은 *wingtiptoyssql.database.windows.net* 같은 정규화된 도메인 이름을 만드는 데 사용됩니다.
   - **서버 관리자 로그인**: 데이터베이스 관리자 이름을 지정합니다.
   - **암호** 및 **암호 확인**: 데이터베이스 관리자용 암호를 지정합니다.
   - **위치**: 데이터베이스에 가장 가까운 Azure 지역을 지정합니다.


1. 위의 정보를 모두 입력한 후 **확인**을 클릭합니다.

1. **검토 + 만들기**를 클릭합니다.

1. 설정을 검토하고 **만들기**를 클릭합니다.

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a>Azure Portal을 사용하여 SQL 서버용 방화벽 규칙 구성하기

1. <https://portal.azure.com/>에서 Azure Portal을 찾아 로그인합니다.

1. **모든 리소스**를 클릭한 다음, 방금 만든 SQL 서버를 클릭합니다.

1. 왼쪽 탐색 창에서 **개요** 섹션을 클릭하고 **서버 방화벽 설정**을 클릭합니다.

   ![방화벽 설정 표시하기][SQL06]

1. **방화벽 및 가상 네트워크** 섹션에서 규칙의 고유명을 지정하여 새 규칙을 만든 다음, 데이터베이스 액세스 권한이 필요한 IP 주소 범위를 입력하고 **저장**을 클릭합니다. (이 연습에서 IP 주소는 클라이언트인 dev 머신의 IP 주소입니다.  **시작 IP 주소** 및 **끝 IP 주소** 모두에 사용할 수 있습니다.)

   ![방화벽 설정 구성하기][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a>Azure Portal을 사용하여 SQL Server의 연결 문자열 검색하기

1. <https://portal.azure.com/>에서 Azure Portal을 찾아 로그인합니다.

1. **모든 리소스**를 클릭한 다음, 방금 만든 SQL 데이터베이스를 클릭합니다.

1. **연결 문자열**을 클릭한 다음 **JDBC**를 클릭하여 JDBC 텍스트 필드 값을 복사합니다.

   ![JDBC 연결 문자열 검색하기][SQL09]

### <a name="create-test-table-in-database"></a>데이터베이스에서 테스트 테이블 만들기
이 데이터베이스에 대해 클라이언트 애플리케이션을 실행하려면 다음 SQL 명령을 사용하여 새 테이블을 만듭니다.

``` SQL
IF NOT EXISTS (SELECT 1 FROM sysobjects WHERE NAME='pet' and XTYPE='U')
  CREATE TABLE pet (
    id      INT           IDENTITY  PRIMARY KEY,
    name    VARCHAR(255),
    species VARCHAR(255)
  );

```

## <a name="configure-the-sample-application"></a>샘플 애플리케이션 구성

1. 명령 셸을 열고 다음 예와 같이 git 명령을 사용하여 샘플 프로젝트를 복제합니다.

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. 다음 종속성을 포함하도록 POM 파일을 수정합니다.

```
 <dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.4.1.jre11</version>
 </dependency>
```
1. 샘플 프로젝트의 *리소스* 디렉터리에서 *application.properties* 파일을 찾거나, 파일이 아직 없는 경우 해당 파일을 만듭니다.

1. 텍스트 편집기에서 *application.properties* 파일을 열어 파일에 다음 줄을 추가하거나 구성하고 샘플 값을 앞서 다룬 적절한 값으로 바꿉니다.

   ```yaml
   spring.datasource.driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   위치:

   | 매개 변수 | Description |
   |---|---|
   | `spring.datasource.url` | 이 문서에서 앞서 다룬 SQL JDBC 문자열의 편집 버전을 지정합니다. |
   | `spring.datasource.username` | 이 문서에서 앞서 다룬 SQL 관리자 이름을 축약된 서버 이름을 추가하여 지정합니다. |
   | `spring.datasource.password` | 이 문서에서 앞서 다룬 SQL 관리자 암호를 지정합니다. |

1. *application.properties* 파일을 저장하고 닫습니다.

## <a name="package-and-test-the-sample-application"></a>샘플 애플리케이션 패키지 및 테스트하기 

1. 다음 예와 같이 Maven을 사용하여 샘플 애플리케이션을 컴파일합니다.

   ```shell
   mvn clean package -P sql
   ```

1. 다음 예와 같이 샘플 애플리케이션을 시작합니다.

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 다음 예와 같이 명령 프롬프트에서 `curl`을 사용하여 새 레코드를 만듭니다.

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   또는:

``` shell
   curl -s -d "{\"name\":\"cat\",\"species\":\"feline\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   애플리케이션이 다음과 같이 값을 반환해야 합니다.

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. 다음 예와 같이 명령 프롬프트에서 `curl`을 사용하여 모든 기존 레코드 검색합니다.

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   애플리케이션이 다음과 같이 값을 반환해야 합니다.

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>요약

이 자습서에서는, Spring Data를 사용하는 Java 샘플 애플리케이션을 만들어 JDBC를 사용하여 Azure SQL 데이터베이스에서 정보를 저장 및 검색했습니다.

## <a name="next-steps"></a>다음 단계

Spring과 Azure에 대한 자세한 사항은 Azure의 Spring 설명서 센터를 참조합니다.

> [!div class="nextstepaction"]
> [Azure의 Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>추가 리소스

Java와 함께 Azure를 사용하는 방법에 관한 자세한 정보는 [Java 개발자를 위한 Azure]와 [Azure DevOps 및 Java 사용하기]를 참조하세요.

<!-- URL List -->

[Java 개발자를 위한 Azure]: /azure/developer/java/
[체험판 Azure 계정]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps 및 Java 사용하기]: /azure/devops/
[MSDN 구독자 혜택]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SQL01]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-09.png
