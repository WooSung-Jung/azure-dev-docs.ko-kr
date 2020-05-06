---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 9403614c7ae2990a35fcbcce6d2e104f87724da5
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673529"
---
### <a name="determine-the-network-topology"></a>네트워크 토폴로지 결정

현재 Marketplace 제품 세트는 마이그레이션의 시작점입니다. 마이그레이션해야 하는 아키텍처의 특징을 제품에서 지원하지 않는 경우 솔루션 템플릿 중 하나를 사용하여 기본 제품을 설치한 후에라도 기존 배포의 네트워크 토폴로지를 캡처하여 Azure에서 재현해야 합니다.

이 토픽은 매우 광범위한 주제이지만, 다음 참고 자료에서 마이그레이션 작업에 대한 약간의 지침을 얻을 수 있습니다.

* 네트워크 토폴로지를 Azure로 마이그레이션하는 것과 관련된 개략적인 토픽을 설명하는 참고 자료: [빠른 추적 배포 가이드](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/deploying.html#GUID-E0BE4A3E-44CD-4C95-9540-7A850BF02F6A)
* 네트워크 토폴로지에 영향을 미치는 클러스터링과 관련된 중요한 사안을 설명하는 참고 자료: [WebLogic Server 클러스터링](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/clustering.html#GUID-E39A18C2-B990-485F-BFB1-0549250FABFE)
* 데이터 원본은 WebLogic 시스템에 있는 별도의 서버이므로 네트워크 토폴로지 분석의 일부라고 생각해야 합니다. [WebLogic Server 데이터 원본](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jdbc.html#GUID-9FD5F552-B2E4-4FEC-8C10-503A08764B52)
* 메시징 원본도 별도의 서버입니다. [WebLogic Server 메시징](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jms.html#GUID-3B5F647D-E001-413B-AC6A-1E103BDBA93F)
* 부하 분산은 기본 요구 사항입니다. WebLogic Server 쪽의 부하 분산에 대해 다루는 참고 자료: [클러스터의 부하 분산](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/load_balancing.html#GUID-B8F6DE4B-1AAC-428B-878B-BFDCE161C054)
