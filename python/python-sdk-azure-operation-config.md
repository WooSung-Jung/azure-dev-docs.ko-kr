---
title: 작업 구성에 대한 매개 변수 - Python용 Azure SDK
description: Python용 Azure SDK에서 발생된 C
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/07/2018
ms.topic: conceptual
ms.devlang: python
ms.custom: seo-python-october2019
ms.openlocfilehash: 0730cec8470a3b55421c6c0cafa08f88819cb1d8
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72279080"
---
# <a name="parameters-for-operation-configuration"></a>작업 구성에 대한 매개 변수

Python용 Azure SDK의 작업에서 메서드에 대한 추가 매개 변수를 제공할 수 있습니다.

추가 매개 변수는 `kwargs`에 제공합니다. 이 기능을 *operation_config*라고 합니다.

작업 구성을 위한 옵션은 다음과 같습니다.

|매개 변수 이름|Type|역할|
|----------------------|------|---------------|
| verify |`bool`|SSL 인증서를 확인할지 지정합니다. 기본값은 True입니다.|
|  cert |`str`| 클라이언트 측 확인을 위한 로컬 인증서의 경로입니다.|
|  시간 제한 |`int`| 서버 연결을 설정하기 위한 시간 제한(초).|
|  allow_redirects |`bool` | 리디렉션을 허용할지 지정합니다.|
|  max_redirects  |`int`| 최대 허용 리디렉션 수입니다.|
|  proxies  |`dict` |프록시 서버 설정입니다.|
|  use_env_proxies |`bool` |로컬 환경 변수에서 프록시 설정을 읽을지 지정합니다.|
|  retries  |`int` | 총 재시도 횟수입니다.|
|  enable_http_logger | `bool`| 디버그 모드에서 HTTP 로그를 사용합니다(기본적으로 False).|
