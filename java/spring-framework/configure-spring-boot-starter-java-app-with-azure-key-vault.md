---
title: Azure Key Vault에 Spring Boot Starter를 사용하는 방법
description: Azure Key Vault 스타터에 Spring Boot Initializer 앱을 구성하는 방법을 알아봅니다.
services: key-vault
documentationcenter: java
ms.date: 10/29/2019
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 13ca873a838d3097484343a28d1f12300550ded8
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812134"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a>Azure Key Vault에 Spring Boot Starter를 사용하는 방법

이 문서에서는 **[Spring Initializr]** 를 사용하여 앱을 만드는 방법을 보여 줍니다. 여기서는 Spring Boot Starter를 Azure Key Vault에 사용하여 키 자격 증명 모음에 비밀 형태로 저장된 연결 문자열을 검색합니다.

## <a name="prerequisites"></a>필수 조건

이 문서의 단계를 완료하려면 다음 필수 구성 요소가 필요합니다.

* Azure 구독. Azure 구독이 아직 없는 경우 [MSDN 구독자 혜택]을 활성화하거나 [체험판 Azure 계정]에 등록할 수 있습니다.
* 지원되는 JDK(Java Development Kit) Azure에서 개발하는 경우 사용할 수 있는 JDK에 대한 자세한 내용은 <https://aka.ms/azure-jdks>를 참조하세요.
* [Apache Maven](http://maven.apache.org/), 버전 3.0 이상

## <a name="create-an-app-using-spring-initializr"></a>Spring Initialzr를 사용하여 앱 만들기

다음 절차에서는 Spring Initializr를 사용하여 애플리케이션을 만듭니다.

1. [https://www.microsoft.com]\(<https://start.spring.io/>) 로 이동합니다.

1. **Java**를 사용하여 **Maven** 프로젝트를 생성할지 지정합니다.  

1. 애플리케이션에 대한 **그룹** 및 **아티팩트** 이름을 입력합니다.

1. **종속성** 섹션에서 **Azure Key Vault**를 입력합니다.

1. 페이지의 아래쪽으로 스크롤하고 **생성**을 클릭합니다.

   ![Spring Boot 프로젝트 생성][secrets-01]

1. 메시지가 표시되면 로컬 컴퓨터의 경로에 프로젝트를 다운로드합니다.

## <a name="sign-into-azure"></a>Azure에 로그인

다음 절차에서는 Azure CLI에서 사용자를 인증합니다.

1. 명령 프롬프트를 엽니다.

1. Azure CLI를 사용하여 Azure 계정에 로그인합니다.

   ```azurecli
   az login
   ```

지시에 따라 로그인 프로세스를 완료합니다.

1. 구독 나열:

   ```azurecli
   az account list
   ```

   Azure가 구독 목록을 반환하며 사용하려는 구독의 GUID를 복사해야 합니다. 예를 들어 다음과 같습니다.

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. 다음 예제처럼 Azure에 사용하려는 계정의 GUID를 지정합니다.

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-a-new-azure-key-vault"></a>새 Azure Key Vault 만들기

다음 절차에서는 키 자격 증명 모음을 만들고 초기화합니다.

1. 다음 예제처럼 키 자격 증명 모음에사용할 Azure 리소스에 대한 리소스 그룹을 만듭니다.

   ```azurecli
   az group create --name vged-rg2 --location westus
   ```

   위치:

   | 매개 변수 | 설명 |
   |---|---|
   | `name` | 리소스 그룹의 고유한 이름을 지정합니다. |
   | `location` | 리소스 그룹을 호스팅할 [Azure 지역](https://azure.microsoft.com/regions/)을 지정합니다. |

   Azure CLI가 다음 에제처럼 리소스 그룹 만들기 결과를 표시합니다.  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/vged-rg2",
     "location": "westus",
     "managedBy": null,
     "name": "vged-rg2",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. 애플리케이션 등록에서 Azure 서비스 주체를 만듭니다. 예를 들어 다음과 같습니다.
   ```shell
   az ad sp create-for-rbac --name "vgeduser"
   ```
   위치:

   | 매개 변수 | 설명 |
   |---|---|
   | `name` | Azure 서비스 주체에 대한 이름을 지정합니다. |

   Azure CLI가 *appId* 및 *암호*가 포함된 JSON 상태 메시지를 반환합니다. 이 항목은 나중에 클라이언트 ID와 클라이언트 암호로 사용됩니다. 예를 들어 다음과 같습니다.

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "vgeduser",
     "name": "http://vgeduser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

3. 리소스 그룹에서 새 키 자격 증명 모음을 만듭니다. 예를 들어 다음과 같습니다.

   ```azurecli
   az keyvault create --name vgedkeyvault --resource-group vged-rg2 --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```

   위치:

   | 매개 변수 | 설명 |
   |---|---|
   | `name` | 키 자격 증명 모음의 고유한 이름을 지정합니다. |
   | `location` | 리소스 그룹을 호스팅할 [Azure 지역](https://azure.microsoft.com/regions/)을 지정합니다. |
   | `enabled-for-deployment` | [키 자격 증명 모음 배포 옵션](/cli/azure/keyvault)을 지정합니다. |
   | `enabled-for-disk-encryption` | [키 자격 증명 모음 암호화 옵션](/cli/azure/keyvault)을 지정합니다. |
   | `enabled-for-template-deployment` | [키 자격 증명 모음 암호화 옵션](/cli/azure/keyvault)을 지정합니다. |
   | `sku` | [키 자격 증명 모음 SKU 옵션](/cli/azure/keyvault)을 지정합니다. |
   | `query` | 이 자습서를 완료하는 데 필요한 키 자격 증명 모은 URI인 값을 응답에서 검색합니다. |

   Azure CLI가 나중에 사용하게 되는 키 자격 증명 모음의 URI를 표시합니다. 예를 들어 다음과 같습니다.  

   ```azurecli
   "https://vgedkeyvault.vault.azure.net"

   ```

4. 앞서 만든 Azure 서비스 주체의 액세스 정책을 설정합니다. 예를 들어 다음과 같습니다.

   ```azurecli
   az keyvault set-policy --name vgedkeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```

   위치:

   | 매개 변수 | 설명 |
   |---|---|
   | `name` | 앞의 키 자격 증명 모음 이름을 지정합니다. |
   | `secret-permission` | 키 자격 증명 모음에 대해 [보안 정책](/cli/azure/keyvault)을 지정합니다. |
   | `spn` | 앞의 애플리케이션 등록에서 GUID를 지정합니다. |

   Azure CLI가 다음 예제처럼 보안 정책 만들기 결과를 표시합니다.  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "location": "westus",
     "name": "vgedkeyvault",
     "properties": {
       ...
       ... (A long list of values will be displayed here.)
       ...
     },
     "resourceGroup": "vged-rg2",
     "tags": {},
     "type": "Microsoft.KeyVault/vaults"
   }
   ```

5. 새 키 자격 증명 모음에 비밀을 저장합니다. 예를 들어 다음과 같습니다.

   ```azurecli
   az keyvault secret set --vault-name "vgedkeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```

   위치:

   | 매개 변수 | 설명 |
   |---|---|
   | `vault-name` | 앞의 키 자격 증명 모음 이름을 지정합니다. |
   | `name` | 비밀 이름을 지정합니다. |
   | `value` | 비밀 값을 지정합니다. |

   Azure CLI가 다음 예제처럼 비밀 만들기 결과를 표시합니다.  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://vgedkeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-app"></a>앱 구성 및 컴파일

애플리케이션을 구성하고 컴파일하려면 다음 절차를 수행합니다.

1. 앞서 디렉터리에 다운로드한 Spring Boot 프로젝트 아카이브 파일에서 파일을 추출합니다.

2. 프로젝트에서 *src/main/resources* 폴더로 이동하고 텍스트 편집기에서 *application.properties* 파일을 엽니다.

3. 이 자습서의 앞부분에서 완료한 단계의 값을 사용하여 키 자격 증명 모음의 값을 추가합니다. 예를 들어 다음과 같습니다.

   ```yaml
   azure.keyvault.uri=https://vgedkeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```

   위치:

   |          매개 변수          |                                 설명                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           키 자격 증명 모음을 만든 URI를 지정합니다.           |
   | `azure.keyvault.client-id`  |  서비스 주체를 만들 때 *appId* GUID를 지정합니다.   |
   | `azure.keyvault.client-key` | 서비스 주체를 만들 때 *암호* GUID를 지정합니다. |


4. 프로젝트의 기본 소스 코드 파일로 이동합니다. 예를 들어 */src/main/java/com/vged/secrets*입니다.

5. 텍스트 편집기에서 애플리케이션의 메인 Java 파일(예: *SecretsApplication.java*)을 열어 다음 줄을 추가합니다.

   ```java
   package com.vged.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }

      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   이 코드 예제는 키 자격 증명 모음에서 연결 문자열을 검색하고 명령줄에 표시합니다.

6. Java 파일을 저장하고 닫습니다.

## <a name="build-and-test-your-app"></a>앱 빌드 및 테스트

애플리케이션을 테스트하려면 다음 절차를 수행합니다.

1. Spring Boot 앱에 대한 *pom.xml* 파일이 있는 디렉터리로 이동합니다.

1. Maven을 사용하여 Spring Boot 애플리케이션을 빌드합니다. 예를 들어 다음과 같습니다.

   ```bash
   mvn clean package
   ```

   Maven이 빌드의 결과를 표시합니다.

   ![Spring Boot 애플리케이션 빌드 상태][build-application-01]

1. Maven에서 Spring Boot 애플리케이션을 실행합니다. 애플리케이션이 키 자격 증명 모음의 연결 문자열을 표시합니다. 예:

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot 런타임 메시지][build-application-02]

## <a name="summary"></a>요약

이 자습서에서는 **[Spring Initializr]** 를 사용하여 새 Java 웹 애플리케이션을 만들었으며, 민감한 정보를 저장하기 위해 Azure Key Vault를 만들었고 Azure Key Vault를 통해 정보를 검색하도록 애플리케이션을 구성했습니다.

## <a name="next-steps"></a>다음 단계

Spring과 Azure에 대한 자세한 사항은 Azure의 Spring 설명서 센터를 참조합니다.

> [!div class="nextstepaction"]
> [Azure의 Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>추가 리소스

Azure Key Vaults를 사용하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.

* [Key Vault 설명서].

* [Azure Key Vault 시작]

Azure에서 Spring Boot 애플리케이션을 사용 하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.

* [Azure App Service에 Spring Boot 애플리케이션 배포](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Azure Container Service의 Kubernetes 클러스터에 Spring Boot 애플리케이션 실행](deploy-spring-boot-java-app-on-kubernetes.md)

Java와 함께 Azure를 사용하는 방법에 관한 자세한 정보는 [Java 개발자를 위한 Azure]와 [Azure DevOps 및 Java 사용하기]를 참조하세요.

<!-- URL List -->

[Key Vault 설명서]: /azure/key-vault/
[Azure Key Vault 시작]: /azure/key-vault/key-vault-get-started
[Java 개발자를 위한 Azure]: /azure/java/
[체험판 Azure 계정]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps 및 Java 사용하기]: /azure/devops/
[MSDN 구독자 혜택]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
