---
title: "Traffic Manager 끝점 모니터링 및 장애 조치(failover) | Microsoft Docs"
description: "이 문서는 Traffic Manager가 끝점 모니터링 및 자동 끝점 장애 조치를 사용하여 Azure 고객이 고가용성 응용 프로그램을 어떻게 배포할 수 있도록 하는지 이해하는 데 도움이 됩니다."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: fff25ac3-d13a-4af9-8916-7c72e3d64bc7
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 69b94c93ad3e9c9745af8485766b4237cac0062c
ms.openlocfilehash: 4df9f744c7dde9224157eca1f869c0c420036d76

---

# <a name="traffic-manager-endpoint-monitoring-and-failover"></a>Traffic Manager 끝점 모니터링 및 장애 조치(failover)

Azure Traffic Manager에는 기본 제공된 끝점 모니터링 및 자동 끝점 장애 조치가 포함됩니다. 이 기능을 통해 Azure 지역 장애를 포함하여 끝점 장애에 대해 복원력 있는 고가용성 응용 프로그램을 제공할 수 있습니다.

## <a name="configure-endpoint-monitoring"></a>끝점 모니터링 구성

끝점 모니터링을 구성하려면 Traffic Manager 프로필에서 다음 설정을 지정해야 합니다.

* **프로토콜**. HTTP 또는 HTTPS를 선택합니다. HTTPS 모니터링은 SSL 인증서가 유효한 지 여부를 확인하지 않으며 인증서가 존재한다는 것만 확인합니다.
* **포트**. 요청에 사용될 포트를 선택합니다.
* **경로**. 모니터링 기능이 액세스하는 웹 페이지 또는 파일의 상대 경로와 이름입니다. 슬래시(/)는 상대 경로로 유효한 항목입니다. 이 값은 파일이 루트 디렉터리(기본값)에 있음을 나타냅니다.

각 끝점의 상태를 검사하기 위해 Traffic Manager는 지정된 프로토콜, 포트 및 상대 경로를 사용하여 끝점에 대해 GET 요청을 합니다.

일반적인 방법은 응용 프로그램 내에서 사용자 지정 페이지를 구현하는 것입니다(예: /health.aspx). 이 경로를 모니터링에 사용하여 성능 카운터 검사 또는 데이터베이스 가용성 확인과 같은 응용 프로그램별 검사를 수행할 수 있습니다. 이러한 사용자 지정 검사에 따르면 페이지는 적절한 HTTP 상태 코드를 반환합니다.

Traffic Manager 프로필의 모든 끝점은 모니터링 설정을 공유합니다. 서로 다른 끝점에 대해 다양한 모니터링 설정을 사용해야 하는 경우 [중첩된 Traffic Manager 프로필](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings)을 만들 수 있습니다.

## <a name="endpoint-and-profile-status"></a>끝점 및 프로필 상태

Traffic Manager 프로필 및 끝점을 사용하거나 사용하지 않도록 설정할 수 있습니다. 그러나 Traffic Manager의 자동 설정 및 프로세스에 따라 끝점 상태가 변경될 수도 있습니다.

### <a name="endpoint-status"></a>끝점 상태

특정 끝점을 사용하거나 사용하지 않도록 설정할 수 있습니다. 정상 상태인 기본 서비스는 영향을 받지 않습니다. 끝점의 상태를 변경하여 Traffic Manager 프로필에서의 끝점 가용성을 제어합니다. 끝점 상태가 Disabled이면 Traffic Manager는 해당 상태를 확인하지 않으며 끝점은 DNS 응답에 포함되지 않습니다.

### <a name="profile-status"></a>프로필 상태

프로필 상태 설정을 사용하여 특정 프로필을 사용하거나 사용하지 않도록 설정할 수 있습니다. 끝점 상태가 단일 끝점에 영향을 주지만 프로필 상태는 모든 끝점을 포함한 전체 프로필에 영향을 줍니다. 프로필을 사용하지 않도록 설정하면 끝점의 상태가 확인되지 않고 끝점이 DNS 응답에 포함되지 않습니다. [NXDOMAIN](https://tools.ietf.org/html/rfc2308) 응답 코드가 DNS 쿼리에 대해 반환됩니다.

### <a name="endpoint-monitor-status"></a>끝점 모니터 상태

끝점 모니터 상태는 끝점의 현재 상태를 보여 주는 Traffic Manager 생성 값입니다. 이 설정은 수동으로 변경할 수 없습니다. 끝점 모니터 상태는 끝점 모니터링의 결과와 구성된 끝점 상태가 조합된 것입니다. 끝점 모니터 상태의 가능한 값은 다음 표에 나와 있습니다.

| 프로필 상태 | 끝점 상태 | 끝점 모니터 상태 | 참고 사항 |
| --- | --- | --- | --- |
| 사용 안 함 |사용 |비활성 |프로필이 사용되지 않도록 설정되었습니다. 끝점 상태는 Enabled여도 프로필 상태(Disabled)가 우선적으로 적용됩니다. Disabled 프로필의 끝점은 모니터링되지 않습니다. NXDOMAIN 응답 코드가 DNS 쿼리에 대해 반환됩니다. |
| &lt;일부&gt; |사용 안 함 |사용 안 함 |끝점이 사용되지 않도록 설정되었습니다. 사용하지 않도록 설정된 끝점이 모니터링되지 않습니다. 끝점이 DNS 응답에 포함되지 않으므로 트래픽을 수신하지 않습니다. |
| 사용 |사용 |온라인 |끝점은 모니터링되며 정상 상태입니다. DNS 응답에 포함되며 트래픽을 수신할 수 있습니다. |
| 사용 |사용 |성능 저하됨 |끝점 모니터링 상태 검사에 실패했습니다. 끝점이 DNS 응답에 포함되지 않으며 트래픽을 수신하지 않습니다. |
| 사용 |사용 |CheckingEndpoint |끝점이 모니터링되지만 첫 번째 검사 결과가 아직 수신되지 않았습니다. CheckingEndpoint는 일반적으로 프로필에서 끝점을 추가하거나 사용하도록 설정한 직후에 나타나는 일시적 상태입니다. 이 상태의 끝점은 DNS 응답에 포함되며 트래픽을 수신할 수 있습니다. |
| 사용 |사용 |중지됨 |끝점이 가리키는 클라우드 서비스 또는 웹앱이 실행되지 않습니다. 클라우드 서비스 또는 웹앱 설정을 확인합니다. Stopped 상태의 끝점은 모니터링되지 않습니다. DNS 응답에 포함되지 않으며 트래픽을 수신하지 않습니다. |

중첩된 끝점의 끝점 모니터 상태가 계산되는 방식에 대한 자세한 내용은 [중첩 Traffic Manager 프로필](traffic-manager-nested-profiles.md)을 참조하세요.

### <a name="profile-monitor-status"></a>프로필 모니터 상태

프로필 모니터 상태는 구성된 프로필 상태와 모든 끝점에 대한 끝점 모니터 상태 값을 조합한 것입니다. 가능한 값은 다음 표에 설명되어 있습니다.

| 프로필 상태(구성된 대로) | 끝점 모니터 상태 | 프로필 모니터 상태 | 참고 사항 |
| --- | --- | --- | --- |
| 사용 안 함 |&lt;일부&gt; 또는 정의된 끝점이 없는 프로필 |사용 안 함 |프로필이 사용되지 않도록 설정되었습니다. |
| 사용 |하나 이상의 끝점 상태가 Degraded입니다. |성능 저하됨 |끝점에 주의가 필요한지 확인하려면 개별 끝점 상태 값을 검토합니다. |
| 사용 |하나 이상의 끝점 상태가 Online입니다. Degraded 상태의 끝점이 없습니다. |온라인 |서비스가 트래픽을 허용합니다. 별도의 작업이 필요하지 않습니다. |
| 사용 |하나 이상의 끝점 상태가 “CheckingEndpoint”입니다. Online 또는 Degraded 상태인 끝점이 없습니다. |CheckingEndpoints |이러한 전환 상태는 프로필을 만들거나 사용하도록 설정하면 발생합니다. 처음으로 끝점 상태가 검사됩니다. |
| 사용 |프로필의 모든 끝점 상태는 Disabled 또는 Stopped이거나 프로필에 정의된 끝점이 없습니다. |비활성 |끝점이 활성 상태가 아니지만 프로필은 여전히 Enabled입니다. |

## <a name="endpoint-failover-and-recovery"></a>끝점 장애 조치 및 복구

Traffic Manager는 비정상 끝점을 포함하는 모든 끝점의 상태를 정기적으로 검사합니다. Traffic Manager는 끝점이 정상 상태가 되는지 감지한 후 다시 회전에 포함시킵니다.

> [!NOTE]
> Traffic Manager는 반환된 메시지가 200 OK인 경우에만 끝점이 온라인이라고 간주합니다. 다음 이벤트 중 하나가 발생할 경우 끝점이 비정상 상태입니다.
>
> * 200이 아닌 응답을 수신함(다양한 2xx 코드 또는 301/302 리디렉션)
> * 클라이언트 인증 요청
> * 시간 제한(시간 제한 임계값은 10초)
> * 연결할 수 없음
>
> 실패한 검사의 문제 해결에 대한 자세한 내용은 [Azure Traffic Manager의 Degraded 상태 문제 해결](traffic-manager-troubleshooting-degraded.md)을 참조하세요.

다음 타임라인은 모니터링 프로세스에 대한 설명입니다.

![Traffic Manager 끝점 장애 조치 및 장애 복구 순서](./media/traffic-manager-monitoring/timeline.png)

1. **GET**. 각 끝점에서 Traffic Manager 모니터링 시스템이 모니터링 설정에서 지정된 파일과 경로에서 GET 요청을 수행합니다.
2. **200 OK**. 모니터링 시스템에서 10초 내에 HTTP 200 OK 메시지가 반환됩니다. 이 응답을 수신하면 서비스를 사용할 수 있는 것으로 인식합니다.
3. **확인 간격 30초**. 끝점 상태 검사가 30초마다 반복됩니다.
4. **서비스를 사용할 수 없음**. 서비스를 사용할 수 없게 됩니다. Traffic Manager는 다음 상태 검사까지 알지 못합니다.
5. **모니터링 파일 액세스 시도(4회)**. 모니터링 시스템은 GET 요청을 수행하지만 10초의 기간 내에 응답을 수신하지 않습니다(또는 200 이외의 응답을 수신할 수 있음). 30초 간격으로 3회 더 시도합니다. 그 중 한 번의 시도가 성공하면 시도 횟수가 다시 설정됩니다.
6. **상태가 Degraded로 설정**. 네 번째 연속 실패 후 모니터링 시스템은 사용할 수 없는 끝점을 Degraded로 표시합니다.
7. **트래픽이 다른 끝점으로 우회됨**. Traffic Manager DNS 이름 서버가 업데이트되고 Traffic Manager에 의해 DNS 쿼리에 대한 응답으로 끝점이 더 이상 반환되지 않습니다. 새 연결은 사용 가능한 다른 끝점으로 전달됩니다. 그러나 이 끝점을 포함하는 이전 DNS 응답은 재귀 DNS 서버 및 DNS 클라이언트에 의해 계속 캐시될 수 있으므로 클라이언트는 DNS 캐시가 만료될 때까지 끝점을 계속 사용합니다. DNS 캐시가 만료되면 클라이언트는 새 DNS 쿼리를 만들고 다른 끝점으로 전달됩니다. 캐시 기간은 Traffic Manager 프로필의 TTL 설정에 의해 제어됩니다(예: 30초).
8. **계속 상태 검사**. Traffic Manager는 Degraded 상태에 있는 끝점의 상태를 계속해서 검사합니다. Traffic Manager는 끝점이 정상 상태가 되면 이를 감지합니다.
9. **서비스가 다시 온라인 상태가 됩니다**. 서비스를 사용할 수 있습니다. 모니터링 시스템에서 다음 상태 검사를 수행할 때까지 끝점은 Traffic Manager에서 Degraded 상태로 유지됩니다.
10. **서비스에 대한 트래픽 다시 시작**. Traffic Manager는 GET 요청을 전송하고 200 OK 상태 응답을 수신합니다. 서비스는 정상 상태로 돌아왔습니다. Traffic Manager 이름 서버는 업데이트되고 DNS 응답에서 서비스의 DNS 이름을 전달하기 시작합니다. 다른 끝점으로 반환되는 캐시된 DNS 응답이 만료되고 다른 끝점에 대한 기존 연결이 종료됨에 따라 트래픽이 또 다시 끝점으로 반환됩니다.

> [!NOTE]
> Traffic Manager는 DNS 수준에서 작동하므로 모든 끝점에 대한 기존 연결에 영향을 미칠 수 없습니다. 끝점 간에 트래픽을 전달할 때는(프로필 설정을 변경하거나 장애 조치 또는 장애 복구 중) Traffic Manager가 사용 가능한 끝점에 새 연결을 전달합니다. 그러나 다른 끝점은 해당 세션이 종료될 때까지 기존 연결을 통해 트래픽을 계속 수신할 수 있습니다. 트래픽이 기존 연결에서 비워지도록 하기 위해 응용 프로그램은 각 끝점에 사용된 세션 기간을 제한해야 합니다.

## <a name="traffic-routing-methods"></a>트래픽 라우팅 방법

끝점이 Degraded 상태가 되면 더 이상 DNS 쿼리에 대한 응답으로 반환되지 않습니다. 대신 대체 끝점이 선택되어 반환됩니다. 프로필에 구성된 트래픽 라우팅 방법은 대체 끝점을 선택하는 방법을 결정합니다.

* **우선 순위**. 끝점은 우선 순위가 정해진 목록을 구성합니다. 목록에 사용 가능한 첫 번째 끝점은 항상 반환됩니다. 끝점 상태가 Degraded면 사용 가능한 다음 끝점이 반환됩니다.
* **가중치 적용**. 사용 가능한 모든 끝점이 할당된 가중치 및 다른 사용 가능한 끝점의 가중치를 기반으로 임의로 선택됩니다.
* **성능**. 최종 사용자에게 가장 가까운 끝점이 반환됩니다. 해당 끝점을 사용할 수 없는 경우 나머지 사용 가능한 모든 끝점에서 임의로 끝점이 선택됩니다. 임의 끝점을 선택하면 가장 가까운 다음 끝점이 오버로드될 때 발생할 수 있는 연속 오류를 방지할 수 있습니다. [중첩 Traffic Manager 프로필](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region)을 사용하여 성능 트래픽 라우팅에 대한 대체 장애 조치 계획을 구성할 수 있습니다.

자세한 내용은 [Traffic Manager 트래픽 라우팅 방법](traffic-manager-routing-methods.md)을 참조하세요.

> [!NOTE]
> 일반적인 트래픽 라우팅 동작의 한 가지 예외는 모든 적격 끝점이 Degraded 상태에 있을 때 발생합니다. Traffic Manager는 “최선”을 다하고 *모든 Degraded 상태 끝점이 실제로 Online 상태인 것처럼 응답*합니다. 이 동작은 DNS 응답에 어떠한 끝점도 반환하지 않는 것보다 바람직한 방법입니다. Disabled 또는 Stopped 끝점은 모니터링되지 않으므로 트래픽에 적합한 것으로 간주되지 않습니다.
>
> 이 문제는 다음과 같은 서비스의 부적절한 구성 때문에 주로 발생합니다.
>
> * ACL[액세스 제어 목록]이 Traffic Manager 상태 검사 차단
> * Traffic Manager 프로필에서 모니터링 경로가 잘못 구성
>
> 이러한 동작은 Traffic Manager 상태 검사가 제대로 구성되지 않는 경우 발생하며 트래픽 라우팅에서 Traffic Manager가 제대로 *작동하는* 것처럼 보일 수 있습니다. 하지만 이 경우에 전체 응용 프로그램 가용성에 영향을 미치는 끝점 장애 조치는 발생할 수 없습니다. 프로필에 Degraded 상태 대신 Online 상태가 표시되는지 확인하는 것이 중요합니다. Online 상태는 Traffic Manager 상태 검사가 예상대로 작동 중임을 나타냅니다.

실패한 상태 검사의 문제 해결에 대한 자세한 내용은 [Azure Traffic Manager의 Degraded 상태 문제 해결](traffic-manager-troubleshooting-degraded.md)을 참조하세요.

## <a name="faq"></a>FAQ

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Traffic Manager 자체는 Azure 하위 지역 장애에 복원력이 있나요?

Traffic Manager는 Azure에서 가용성 높은 응용 프로그램을 전달하는 데 있어서 핵심적인 구성 요소입니다.
높은 가용성을 제공하려면 Traffic Manager는 매우 높은 수준의 가용성을 유지해야 하고 지역 실패에 대해 복원력도 뛰어나야 합니다.

기본적으로 Traffic Manager 구성 요소는 모든 Azure 지역의 전체 오류에 대해 복원력을 갖습니다. 이러한 복원력은 DNS 이름 서버, API, 저장소 계층 및 끝점 모니터링 서비스 등 모든 Traffic Manager 구성 요소에 해당됩니다.

전체 Azure 하위 지역의 가동 중단과 같은 드문 경우에도, Traffic Manager는 계속 정상적으로 작동해야 합니다. 여러 Azure 하위 지역에 배포된 응용 프로그램은 Traffic Manager에 의존하여 응용 프로그램의 사용 가능한 인스턴스로 트래픽을 전달할 수 있습니다.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>리소스 그룹의 위치 선택은 Traffic Manager에 어떤 영향을 주나요?

Traffic Manager는 단일 전역 서비스입니다. 지역에 영향을 받지 않습니다. 리소스 그룹의 위치 선택은 해당 리소스 그룹에 배포된 Traffic Manager 프로필에 어떤 영향도 주지 않습니다.

Azure Resource Manager에서는 모든 리소스 그룹에서 '위치'를 지정해야 하며 이를 통해 해당 리소스 그룹에 배포된 리소스의 기본 위치가 결정됩니다. Traffic Manager 프로필을 만들면 리소스 그룹에 만들어집니다. 모든 Traffic Manager 프로필은 위치로 **전역**을 사용하고 리소스 그룹 기본값을 재정의합니다.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>각 끝점의 현재 상태를 확인하려면 어떻게 해야 하나요?

각 끝점의 현재 상태 모니터링 상태 및 전체 프로필은 Azure 포털에 표시됩니다. 이 정보는 Traffic Manager [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [PowerShell cmdlets](https://msdn.microsoft.com/library/mt125941.aspx) 및 [크로스 플랫폼 Azure CLI](../xplat-cli-install.md)를 통해서도 사용할 수 있습니다.

Azure는 지난 끝점 상태에 대한 기록 정보를 제공하지 않으며 끝점 상태 변경에 대한 경고를 생성하는 기능도 없습니다.

### <a name="can-i-monitor-https-endpoints"></a>HTTPS 끝점을 모니터링할 수 있나요?

예. Traffic Manager는 HTTPS를 통한 검색을 지원합니다. 모니터링 구성에서 프로토콜로 **HTTPS**를 구성합니다.

Traffic Manager는 다음을 포함하는 인증서 유효성 검사를 제공할 수 없습니다.

* 서버 쪽 인증서의 유효성이 검사되지 않습니다.
* SNI 서버 쪽 인증서가 지원되지 않습니다.
* 클라이언트 인증서는 지원되지 않습니다.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>끝점 상태 검사에 어떤 호스트 헤더가 사용되나요?

Traffic Manager는 HTTP 및 HTTPS 상태 검사에서 호스트 헤더를 사용합니다. Traffic Manager가 사용하는 호스트 헤더는 프로필에 구성된 끝점 대상의 이름입니다. 호스트 헤더에 사용된 값은 target 속성과 별도로 지정할 수 없습니다.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>상태 검사가 시작되는 IP 주소는 무엇인가요?

다음 목록에는 Traffic Manager 상태 검사가 시작될 수 있는 IP 주소가 나와 있습니다. 이 목록의 IP 주소에서 들어오는 연결의 끝점에서는 상태 검사가 허용됩니다.

* 40.68.30.66
* 40.68.31.178
* 137.135.80.149
* 137.135.82.249
* 23.96.236.252
* 65.52.217.19
* 40.87.147.10
* 40.87.151.34
* 13.75.124.254
* 13.75.127.63
* 52.172.155.168
* 52.172.158.37
* 104.215.91.84
* 13.75.153.124
* 13.84.222.37
* 23.101.191.199
* 23.96.213.12
* 137.135.46.163
* 137.135.47.215
* 191.232.208.52
* 191.232.214.62
* 13.75.152.253
* 104.41.187.209
* 104.41.190.203

## <a name="next-steps"></a>다음 단계

 [Traffic Manager 작동 방식](traffic-manager-how-traffic-manager-works.md)

Traffic Manager가 지원하는 [트래픽 라우팅 방법](traffic-manager-routing-methods.md) 에 대해 자세히 알아봅니다.

[Traffic Manager 프로필을 만드는](traffic-manager-manage-profiles.md)

[Degraded 상태 문제를 해결](traffic-manager-troubleshooting-degraded.md) 합니다.



<!--HONumber=Nov16_HO3-->


