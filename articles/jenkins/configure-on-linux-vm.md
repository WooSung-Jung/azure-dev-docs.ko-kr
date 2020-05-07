---
title: 빠른 시작 - Azure에서 Jenkins 서버 만들기
description: Jenkins 솔루션 템플릿에서 Jenkins를 Azure Linux 가상 머신에 설치하고 Java 애플리케이션 샘플을 빌드하는 방법을 알아봅니다.
keywords: Jenkins, Azure, DevOps, 포털, Linux, 가상 머신, 솔루션 템플릿
ms.topic: quickstart
ms.date: 03/03/2020
ms.openlocfilehash: 2ea038dad2294784804f45026ea26198a9b12d79
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82170339"
---
# <a name="quickstart-create-a-jenkins-server-on-an-azure-linux-vm"></a>빠른 시작: Azure Linux VM에 Jenkins 서버 만들기

이 빠른 시작은 Azure에서 사용 가능하도록 구성된 도구와 플러그 인을 사용하여 Ubuntu Linux VM에 [Jenkins](https://jenkins.io)를 설치하는 방법을 보여줍니다. 작업을 완료하면 [GitHub](https://github.com)에서 샘플 Java 앱을 빌드하는 Jenkins 서버가 Azure에서 실행됩니다.

## <a name="prerequisites"></a>사전 요구 사항

* 컴퓨터의 명령줄에서 SSH에 대한 액세스(Bash 셸 또는 [PuTTY](https://www.putty.org/) 등)

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>솔루션 템플릿에서 Jenkins VM 만들기

Jenkins는 단일 Jenkins 설치가 많은 수의 프로젝트를 호스트하거나 빌드 또는 테스트에 필요한 다양한 환경을 제공할 수 있도록 하나 이상의 에이전트에 작업을 위임하는 모델을 지원합니다. 이 섹션의 단계는 Azure에서 Jenkins 서버를 설치 및 구성하는 과정을 안내합니다.

1. 브라우저에서 [Azure Marketplace image for Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview)(Jenkins용 Azure Marketplace 이미지)를 엽니다.

1. **지금 가져오기**를 선택합니다.

    ![[지금 가져오기]를 선택하여 Jenkins Marketplace 이미지에 대한 설치 프로세스를 시작합니다.](./media/install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. 가격 정보와 사용 조건 정보를 검토한 후 **계속**을 선택합니다.

    ![Jenkins Marketplace 이미지 가격 및 사용 조건 정보입니다.](./media/install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. **만들기**를 선택하여 Azure Portal에서 Jenkins 서버를 구성합니다. 

    ![Jenkins Marketplace 이미지를 설치합니다.](./media/install-from-azure-marketplace-image/jenkins-install-create.png)

1. **기본 사항** 탭에서 다음 값을 지정합니다.

   - **이름** - `Jenkins`를 입력합니다.
   - **사용자 이름** - Jenkins가 실행 중인 가상 머신에 로그인할 때 사용할 사용자 이름을 입력합니다. 사용자 이름은 [특정 요구 사항](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm)을 충족해야 합니다.
   - **인증 유형** - **SSH 공개 키**를 선택합니다.
   - **SSH 공개 키** - RSA 공개 키를 복사하여 단일 줄 형식(`ssh-rsa`으로 시작) 또는 여러 줄 PEM 형식으로 붙여넣습니다. Linux 및 macOS에서는 ssh-keygen을, Windows에서는 PuTTYGen을 사용하여 SSH 키를 생성할 수 있습니다. SSH 키 및 Azure에 대한 자세한 내용은 [Azure에서 Windows를 통해 SSH 키를 사용하는 방법](/azure/virtual-machines/linux/ssh-from-windows) 문서를 참조하세요.
   - **구독** - Jenkins를 설치할 Azure 구독을 선택합니다.
   - **리소스 그룹** - **새로 만들기**를 선택하고 Jenkins 설치를 구성하는 리소스 컬렉션의 논리 컨테이너 역할을 하는 리소스 그룹 이름을 입력합니다.
   - **위치** - **미국 동부**를 선택합니다.

     ![[기본] 탭에 Jenkins에 대한 인증 및 리소스 그룹 정보를 입력합니다.](./media/install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. **확인**을 선택하여 **추가 설정** 탭으로 이동합니다. 

1. **추가 설정** 탭에서 다음 값을 지정합니다.

   - **크기** - Jenkins 가상 머신에 적합한 크기 지정 옵션을 선택합니다.
   - **VM 디스크 유형** - HDD(하드 디스크 드라이브) 또는 SSD(반도체 드라이브)를 지정하여 Jenkins 가상 머신에 허용된 스토리지 디스크 유형을 나타냅니다.
   - **가상 네트워크** - (선택 사항) **가상 네트워크**를 선택하여 기본 설정을 수정합니다.
   - **서브넷** - **서브넷**을 선택하고, 정보를 확인한 다음, **확인**을 선택합니다.
   - **공용 IP 주소** - IP 주소 이름은 기본적으로 이전 페이지에서 지정한 Jenkins에 접미사 -IP가 붙습니다. 옵션을 선택하여 이 기본값을 변경할 수 있습니다.
   - **도메인 이름 레이블** - Jenkins 가상 머신에 대한 정규화된 URL 값을 지정합니다.
   - **Jenkins 릴리스 종류** - 옵션(`LTS`, `Weekly build` 또는 `Azure Verified`)에서 원하는 릴리스 종류를 선택합니다. `LTS` 및 `Weekly build` 옵션은 [Jenkins LTS 릴리스 라인](https://jenkins.io/download/lts/) 문서에서 설명하고 있습니다. `Azure Verified` 옵션은 Azure에서 실행되도록 확인된 [Jenkins LTS 버전](https://jenkins.io/download/lts/)을 참조합니다. 
   - **JDK 형식** - 설치될 JDK입니다. 기본값은 OpenJDK의 Zulu 테스트 및 인증된 빌드입니다.

     ![[설정] 탭에서 Jenkins에 대한 가상 머신 설정을 입력합니다.](./media/install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. **확인**을 선택하여 **통합 설정** 탭으로 이동합니다.

1. **통합 설정** 탭에서 다음 값을 지정합니다.

    - **서비스 사용자** - Azure와의 인증을 위한 자격 증명으로 Jenkins에 추가됩니다. `Auto`는 MSI(관리 서비스 ID)에서 서비스 사용자를 만든다는 것을 의미합니다. `Manual`은 서비스 사용자를 직접 만들어야 함을 의미합니다. 
        - **애플리케이션 ID** 및 **비밀** - **서비스 사용자** 옵션에 대해 `Manual` 옵션을 선택하는 경우 서비스 사용자에 대해 `Application ID` 및 `Secret`을 지정해야 합니다. [서비스 사용자를 만들](/cli/azure/create-an-azure-service-principal-azure-cli) 때 기본 역할은 Azure 리소스를 사용하는 데 충분한 **참가자**입니다.
    - **클라우드 에이전트 사용** - `ACI`가 Azure Container Instances를 참조하고 `VM`이 가상 머신을 참조하는 에이전트에 대한 기본 클라우드 템플릿을 지정합니다. 클라우드 에이전트를 사용하지 않도록 설정하려면 `No`를 지정할 수도 있습니다.

1. **확인**을 선택하여 **요약** 탭으로 이동합니다.

1. **요약** 탭이 표시되면 입력된 정보의 유효성을 검사합니다. 탭의 위쪽에서 **유효성 검사 통과** 메시지가 표시되면 **확인**을 선택합니다. 

     ![[요약] 탭이 표시되고 선택한 옵션의 유효성을 검사합니다.](./media/install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. **만들기** 탭이 표시되면 **만들기**를 선택하여 Jenkins 가상 머신을 만듭니다. 서버가 준비되면 Azure Portal에 알림이 표시됩니다.

     ![Jenkins가 준비되었음을 알리는 메시지입니다.](./media/install-from-azure-marketplace-image/jenkins-install-notification.png)

## <a name="connect-to-jenkins"></a>Jenkins에 연결

1. 웹 브라우저에서 가상 머신(예: `http://jenkins2517454.eastus.cloudapp.azure.com/`)으로 이동합니다. Jenkins 콘솔은 보안되지 않은 HTTP를 통해 액세스할 수 없으므로 SSH 터널을 사용하여 컴퓨터에서 안전하게 Jenkins 콘솔에 액세스하는 지침이 이 페이지에서 제공됩니다.

    ![Jenkins 잠금 해제](./media/install-solution-template-steps/jenkins-ssh-instructions.png)

1. 명령줄 페이지에서 `ssh` 명령을 사용하여 터널을 설정하고, 솔루션 템플릿에서 가상 머신을 설정하는 경우 이전에 선택한 가상 머신 관리 사용자의 이름으로 `username`을 바꿉니다.

    ```bash
    ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
    ```
    
1. 터널을 시작한 후에 로컬 컴퓨터에서 `http://localhost:8080/`으로 이동합니다. 

1. Jenkins VM에 SSH를 통해 연결되어 있는 동안 명령줄에서 다음 명령을 실행하여 초기 암호를 가져옵니다.

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```
    
1. 이 초기 암호를 사용하여 Jenkins 대시보드를 처음으로 잠금 해제합니다.

    ![Jenkins 잠금 해제](./media/install-solution-template-steps/jenkins-unlock.png)

1. 다음 페이지에서 **Install suggested plugins**(권장 플러그 인 설치)를 선택하고, Jenkins 대시보드에 액세스하는 데 사용한 Jenkins 관리 사용자를 만듭니다.

    ![Jenkins가 준비되었습니다.](./media/install-solution-template-steps/jenkins-welcome.png)

이제 Jenkins 서버는 코드를 빌드할 준비가 되었습니다.

## <a name="create-your-first-job"></a>첫 번째 작업 만들기

1. Jenkins 콘솔에서 **Create new jobs**(새 작업 만들기)를 선택하고 이름은 **mySampleApp**으로 지정하고 **Freestyle project**(프리스타일 프로젝트)를 선택한 후 **OK**(확인)를 선택합니다.

    ![새 작업 만들기](./media/install-solution-template-steps/jenkins-new-job.png) 

1. **소스 코드 관리** 탭을 선택하고, **Git** 사용을 설정한 다음, **리포지토리 URL** 필드에 다음 URL을 입력합니다. `https://github.com/spring-guides/gs-spring-boot.git`

    ![Git 리포지토리 정의](./media/install-solution-template-steps/jenkins-job-git-configuration.png) 

1. **빌드** 탭을 선택한 후 **빌드 단계 추가**, **Gradle 스크립트 호출**을 선택합니다. **Gradle 래퍼 사용**을 선택한 후 **래퍼 위치**에 `complete`를 입력하고 **작업**에 `build`를 입력합니다.

    ![Gradle 래퍼를 사용하여 빌드](./media/install-solution-template-steps/jenkins-job-gradle-config.png) 

1. **고급**을 선택한 다음, **루트 빌드 스크립트** 필드에서 `complete`를 입력합니다. **저장**을 선택합니다.

    ![Gradle 래퍼 빌드 단계에서 고급 설정을 지정](./media/install-solution-template-steps/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>코드 빌드

1. **지금 빌드**를 선택하여 코드를 컴파일하고 샘플 앱을 패키징합니다. 빌드가 완료되면 프로젝트에 대한 **작업 영역** 링크를 선택합니다.

    ![작업 영역으로 이동하여 빌드에서 JAR 파일 가져오기](./media/install-solution-template-steps/jenkins-access-workspace.png) 

1. `complete/build/libs`로 이동하여 `gs-spring-boot-0.1.0.jar`이 있으면 빌드가 성공한 것입니다. 이제 Azure에서 Jenkins 서버로 고유한 프로젝트를 빌드할 준비가 되었습니다.

## <a name="troubleshooting-the-jenkins-solution-template"></a>Jenkins 솔루션 템플릿 문제 해결

Jenkins 솔루션 템플릿에서 버그가 발생하면 [Jenkins GitHub 리포지토리](https://github.com/azure/jenkins/issues)에서 문제를 제출하세요.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure의 Jenkins](/azure/developer/jenkins)