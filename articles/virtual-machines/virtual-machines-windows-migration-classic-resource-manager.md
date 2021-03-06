---
title: "클래식에서 Azure Resource Manager로 IaaS 리소스의 플랫폼 지원 마이그레이션 | Microsoft Docs"
description: "이 문서에서는 플랫폼 지원 방식으로 클래식에서 Azure Resource Manager로 리소스를 마이그레이션하는 과정을 안내합니다."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 78492a2c-2694-4023-a7b8-c97d3708dcb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: 66b1bcdf0f79ff4743f466c3737696f53ef6a44c
ms.openlocfilehash: 8eb70339785ca15131b5ce8debd6a232a8a693b9


---
# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>클래식에서 Azure Resource Manager로 IaaS 리소스의 플랫폼 지원 마이그레이션
이 문서에서는 Microsoft가 어떻게 클래식에서 Resource Manager 배포 모델로 IaaS(Infrastructure as a Service) 리소스를 마이그레이션할 수 있도록 지원하는지 설명합니다. [Azure Resource Manager 기능 및 이점](../azure-resource-manager/resource-group-overview.md)에 대해 자세히 알아볼 수 있습니다. 가상 네트워크 사이트-사이트 게이트웨이를 사용하여 구독에서 공존하는 두 가지 배포 모델의 리소스를 연결하는 방법에 대해서도 설명합니다.

## <a name="goal-for-migration"></a>마이그레이션 목표
Resource Manager는 템플릿을 사용하여 복잡한 응용 프로그램을 배포할 수 있도록 지원하며 VM 확장을 사용하여 가상 컴퓨터를 구성하고 액세스 관리와 태깅을 통합합니다. Azure Resource Manager는 가용성 집합에 가상 컴퓨터에 대해 확장성 있는 병렬 배포를 포함합니다. 그뿐 아니라 새로운 배포 모델에서는 계산, 네트워크, 저장소의 수명 주기를 독립적으로 관리할 수 있습니다. 마지막으로 가상 네트워크에 가상 컴퓨터를 적용하여 보안 구현을 기본적으로 중요시합니다.

클래식 배포 모델의 거의 모든 기능이 Azure Resource Manager의 계산, 네트워크 및 저장소에 대해 지원됩니다. Azure Resource Manager의 새로운 기능을 제대로 활용하려는 경우 클래식 배포 모델에서 기존 배포를 마이그레이션할 수 있습니다.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>마이그레이션 후 자동화 및 도구 변경
클래식 배포 모델에서 Resource Manager 배포 모델로 리소스를 마이그레이션하는 과정의 일부로, 기존 자동화 또는 도구를 업데이트하여 마이그레이션 후에도 계속 작동되도록 해야 합니다.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>클래식에서 Resource Manager로 IaaS 리소스를 마이그레이션하는 작업의 의미
자세한 내용을 살펴보기 전에 IaaS 리소스에서 데이터 평면과 관리 평면 작업 간 차이를 살펴보겠습니다.

* *관리 평면* 은 리소스 수정을 위해 관리 평면 또는 API로 들어오는 호출을 설명합니다. 예를 들어 VM을 만들고, VM을 다시 시작하고, 새 서브넷을 사용하여 가상 네트워크를 업데이트하는 등의 작업은 실행 중인 리소스를 관리합니다. 인스턴스 연결에는 직접적으로 영향을 주지 않습니다.
* *데이터 평면* (응용 프로그램)은 응용 프로그램 자체의 런타임을 설명하며 Azure API를 통하지 않는 인스턴스와의 상호 작용이 포함됩니다. 웹 사이트에 액세스하거나 실행 중인 SQL Server 인스턴스 또는 mongoDB 서버에서 데이터를 가져오는 경우 데이터 평면 또는 응용 프로그램 상호 작용으로 간주됩니다. 저장소 계정에서 Blob을 복사하고 공용 IP 주소에 액세스하여 가상 컴퓨터에 RDP 또는 SSH 연결을 하는 작업도 데이터 평면에 해당합니다. 이러한 작업은 계산, 네트워킹, 저장소에서 응용 프로그램을 계속 실행하는 상태에서 수행합니다.

> [!NOTE]
> 일부 마이그레이션 시나리오에서 Azure 플랫폼은 가상 컴퓨터를 중지하고, 할당 취소하고, 다시 시작합니다. 이 경우 데이터 평면 가동이 잠시 중지됩니다.
>
>

## <a name="supported-scopes-of-migration"></a>지원되는 마이그레이션 범위
주로 계산, 네트워크 및 저장소를 대상으로 하는 세 가지 마이그레이션 범위가 있습니다.

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>가상 컴퓨터 마이그레이션(가상 네트워크가 아님)
Resource Manager 배포 모델에서는 기본적으로 응용 프로그램 보안이 적용되어 있습니다. 모든 VM은 Resource Manager 모델의 가상 네트워크에 있어야 합니다. Azure 플랫폼은 마이그레이션의 일부로 VM을 다시 시작합니다(`Stop`, `Deallocate` 및 `Start`). 가상 네트워크에는 다음과 같은 두 가지 옵션이 있습니다.

* 플랫폼에서 새 가상 네트워크를 만들도록 요청하고 가상 컴퓨터를 새 가상 네트워크로 마이그레이션할 수 있습니다.
* 가상 컴퓨터를 Resource Manager의 기존 가상 네트워크로 마이그레이션할 수 있습니다.

> [!NOTE]
> 이 마이그레이션 범위에서는 마이그레이션 중 특정 시간 동안 관리 평면 작업 및 데이터 평면 작업이 허용되지 않을 수 있습니다.
>
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>가상 컴퓨터 마이그레이션(가상 네트워크에서)
대부분의 VM 구성에서는 클래식과 Resource Manager 배포 모델 간에 메타데이터만 마이그레이션됩니다. 기본 VM은 동일한 네트워크의 동일한 하드웨어에서 동일한 저장소로 실행됩니다. 마이그레이션 중 특정 시간 동안 관리 평면 작업이 허용되지 않을 수 있습니다. 그러나 데이터 평면은 계속 작동합니다. 즉, 마이그레이션 중 VM(클래식) 상에서 실행되는 응용 프로그램에 가동 중지 시간이 발생하지 않습니다.

현재 지원되지 않는 구성은 다음과 같습니다. 향후 이에 대한 지원을 추가할 경우 이 구성의 일부 VM에서 가동 중지 시간(VM 작업 중지, 할당 취소, 다시 시작 진행)이 발생할 수 있습니다.

* 단일 클라우드 서비스에 둘 이상의 가용성 집합이 있습니다.
* 단일 클라우드 서비스에서 하나 이상의 가용성 집합과 가용성 집합에 없는 VM이 있습니다.

> [!NOTE]
> 이 마이그레이션 범위에서는 마이그레이션 중 특정 시간 동안 관리 평면이 허용되지 않을 수 있습니다. 앞서 설명한 특정 구성의 경우 데이터 평면 가동 중지 시간이 발생합니다.
>
>

### <a name="storage-accounts-migration"></a>저장소 계정 마이그레이션
원활한 마이그레이션을 위해 클래식 저장소 계정에 Resource Manager VM을 배포할 수 있습니다. 이 기능을 사용하면 계산 및 네트워크 리소스를 저장소 계정과 상관없이 마이그레이션할 수 있으며 이러한 방식이 바람직합니다. 가상 컴퓨터 및 가상 네트워크에 대해 마이그레이션한 후에는 저장소 계정에 대해 마이그레이션을 수행하여 마이그레이션 프로세스를 완료해야 합니다.

> [!NOTE]
> Resource Manager 배포 모델에는 기본 이미지 및 디스크 개념이 적용되지 않습니다. 저장소 계정이 마이그레이션되면 클래식 이미지와 디스크가 Resource Manager 스택에 표시되지 않지만 백업 VHD는 저장소 계정에 남아 있습니다.
>
>

## <a name="unsupported-features-and-configurations"></a>지원되지 않는 기능 및 구성
현재는 일부 기능 및 구성 집합을 지원하지 않습니다. 다음 섹션에서는 이와 관련된 권장 지침에 대해 설명합니다.

### <a name="unsupported-features"></a>지원되지 않는 기능
현재 지원되지 않는 기능은 다음과 같습니다. 이러한 설정을 제거하고 VM을 마이그레이션한 다음 Resource Manager 배포 모델에서 설정을 다시 사용하도록 지정할 수 있습니다(선택 사항).

| 리소스 공급자 | 기능 |
| --- | --- |
| 계산 |연결되지 않은 가상 컴퓨터 디스크 |
| 계산 |가상 컴퓨터 이미지 |
| 네트워크 |끝점 ACL. |
| 네트워크 |가상 네트워크 게이트웨이(Azure ExpressRoute 게이트웨이, Application Gateway) |
| 네트워크 |VNet 피어링을 사용하는 가상 네트워크 (VNet을 ARM으로, 다시 피어로 마이그레이션) [VNet 피어링](../virtual-network/virtual-network-peering-overview.md)에 대해 자세히 알아봅니다. |
| 네트워크 |트래픽 관리자 프로필 |

### <a name="unsupported-configurations"></a>지원되지 않는 구성
현재 지원되지 않는 구성은 다음과 같습니다.

| 부여 | 구성 | 권장 사항 |
| --- | --- | --- |
| 리소스 관리자 |클래식 리소스에 대한 RBAC(역할 기반 액세스 제어) |마이그레이션 후 리소스의 URI가 수정되므로 마이그레이션 후에 수행되어야 하는 RBAC 정책 업데이트를 계획하는 것이 좋습니다. |
| 계산 |VM과 연결된 여러 서브넷 |서브넷만 참조하도록 서브넷 구성을 업데이트합니다. |
| 계산 |가상 네트워크에 속하지만 명시적 서브넷이 할당되지 않은 가상 컴퓨터 |VM을 삭제할 수 있습니다(선택 사항). |
| 계산 |경고, 자동 크기 조정 정책이 있는 가상 컴퓨터 |마이그레이션이 진행되고 이러한 설정은 삭제됩니다. 따라서 마이그레이션 전에 환경을 평가하는 것이 좋습니다. 또는 마이그레이션이 완료된 다음 경고 설정을 다시 구성할 수 있습니다. |
| 계산 |XML VM 확장(BGInfo 1.*, Visual Studio 디버거, 웹 배포 및 원격 디버깅) |이 기능은 지원되지 않습니다. 마이그레이션을 계속하려면 가상 컴퓨터에서 이러한 확장을 제거하는 것이 좋습니다. 그러지 않으면 마이그레이션 프로세스 중에 자동으로 삭제됩니다. |
| 계산 |프리미엄 저장소를 사용한 부팅 진단 |마이그레이션을 계속하기 전에 VM에 대한 부팅 진단 기능을 비활성화합니다. 마이그레이션이 완료된 후에 Resource Manager 스택에서 부팅 진단을 재활성화할 수 있습니다. 또한 스크린샷 및 직렬 로그에 대해 사용되는 blob은 그러한 blob에 대해 요금이 부과되지 않도록 삭제해야 합니다. |
| 계산 |웹/작업자 역할이 포함된 클라우드 서비스 |현재는 지원되지 않습니다. |
| 네트워크 |가상 컴퓨터와 웹/작업자 역할이 포함된 가상 네트워크 |현재는 지원되지 않습니다. |
| Azure 앱 서비스 |앱 서비스 환경이 포함된 가상 네트워크 |현재는 지원되지 않습니다. |
| Azure HDInsight |HDInsight Services가 포함된 가상 네트워크 |현재는 지원되지 않습니다. |
| Microsoft Dynamics Lifecycle Services |Dynamics Lifecycle Services에서 관리하는 가상 컴퓨터가 포함된 가상 네트워크 |현재는 지원되지 않습니다. |
| Azure AD Domain Services |Azure AD Domain Services가 포함된 가상 네트워크 |현재는 지원되지 않습니다. |
| Compute |온-프레미스 DNS 서버에서 전송 연결 중인 VPN Gateway 또는 ExpressRoute 게이트웨이가 있는 VNET을 포함하는 Azure Security Center 확장 |Azure Security Center는 보안을 모니터링하고 경고를 발생시키기 위한 확장을 가상 컴퓨터에 자동으로 설치합니다. 이러한 확장은 일반적으로 구독에서 Azure Security Center가 사용되도록 설정되면 자동으로 설치됩니다. ExpressRoute 게이트웨이 마이그레이션은 현재 지원되지 않고 전송 연결된 VPN Gateway는 온-프레미스 액세스를 상실합니다. ExpressRoute 게이트웨이를 삭제하거나 전송 연결된 VPN Gateway를 마이그레이션하면 마이그레이션을 커밋하도록 진행하는 경우 VM 저장소 계정에 대한 인터넷 액세스를 상실합니다. 게스트 에이전트 상태 blob을 채울 수 없어서 이 문제가 발생하는 경우에는 마이그레이션이 진행되지 않습니다. 마이그레이션을 계속하기 3시간 전에, 구독에서 Azure Security Center 정책을 사용하지 않도록 설정하는 것이 좋습니다. |

## <a name="the-migration-experience"></a>마이그레이션 환경
마이그레이션 환경을 시작하기 전에 다음을 수행하는 것이 좋습니다.

* 마이그레이션하려는 리소스가 지원되지 않는 기능이나 구성을 사용하지 않도록 확인합니다. 일반적으로 플랫폼이 이러한 문제를 감지하고 오류를 생성합니다.
* 가상 네트워크에 없는 VM이 있는 경우에는 준비 작업 중에 중지 및 할당 취소됩니다. 공용 IP 주소를 유지하려는 경우에는 준비 작업을 트리거하기 전에 IP 주소 예약을 참조하세요. 하지만 VM이 가상 네트워크에 있는 경우에는 중지 및 할당 취소되지 않습니다.
* 업무 외 시간 동안 마이그레이션을 계획하여 마이그레이션 중 발생할 수 있는 예기치 않은 오류를 해결할 수 있도록 계획하십시오.
* 준비 단계가 완료된 후 쉽게 검증할 수 있도록 PowerShell, CLI(명령줄 인터페이스) 명령 또는 REST API를 사용하여 VM의 현재 구성을 다운로드합니다.
* 마이그레이션을 시작하기 전에 Resource Manager 배포를 처리할 수 있도록 자동화/운영 스크립트를 업데이트합니다. 경우에 따라 리소스가 준비됨 상태일 때 GET 작업을 실행할 수도 있습니다.
* 클래식 IaaS 리소스에 구성된 RBAC 정책을 평가하고 마이그레이션이 완료되면 계획을 세웁니다.

마이그레이션 워크플로는 다음과 같습니다.

![마이그레이션 워크플로를 보여주는 스크린 샷](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> 다음 섹션에서 설명하는 모든 작업은 idempotent 상태입니다. 지원되지 않는 기능 또는 구성 오류 이외의 문제가 발생하는 경우 준비, 중단 또는 커밋 작업을 다시 시도하는 것이 좋습니다. Azure 플랫폼은 이 작업을 다시 시도합니다.
>
>

### <a name="validate"></a>유효성 검사
유효성 검사 작업은 마이그레이션 프로세스의 첫 번째 단계입니다. 이 단계의 목표는 마이그레이션 대상 리소스에 대해 백그라운드에서 데이터 분석을 수행하고 리소스 마이그레이션이 가능할 경우 성공/실패를 반환합니다.

마이그레이션을 유효성 검사하려는 가상 네트워크 또는 호스티드 서비스(가상 네트워크가 아닌 경우)를 선택합니다.

* 리소스를 마이그레이션할 수 없을 경우 Azure 플랫폼에 마이그레이션이 지원되지 않는 모든 이유가 나열됩니다.

### <a name="prepare"></a>준비
준비 작업은 마이그레이션 프로세스의 두 번째 단계입니다. 이 단계의 목표는 클래식에서 Resource Manager 리소스로의 IaaS 리소스 전환을 시뮬레이션하고 사용자가 볼 수 있도록 나란히 표시하는 것입니다.

마이그레이션을 준비하려는 가상 네트워크 또는 호스티드 서비스(가상 네트워크가 아닌 경우)를 선택합니다.

* 리소스를 마이그레이션할 수 없을 경우 Azure 플랫폼이 마이그레이션 프로세스를 중지하고 준비 작업이 실패한 이유를 나열합니다.
* 리소스 마이그레이션이 가능할 경우에는 Azure 플랫폼이 가장 먼저 마이그레이션 리소스에 대한 관리 평면 작업을 잠급니다. 예: 마이그레이션 중인 VM에 데이터 디스크를 추가할 수 없습니다.

그런 다음 Azure 플랫폼이 마이그레션하는 리소스에 대해 클래식에서Resource Manager로 메타데이터 마이그레이션을 시작합니다.

준비 작업이 완료되면 리소스를 클래식 및 Resource Manager에서 볼 수 있는 옵션이 표시됩니다. 클래식 배포 모델의 모든 클라우드 서비스에 대해 Azure Platform에서 `cloud-service-name>-migrated`패턴의 리소스 그룹 이름을 만듭니다.

> [!NOTE]
> 기존 가상 네트워크에 없는 가상 컴퓨터는 이 마이그레이션 단계에서 할당 취소된 상태로 중지됩니다.
>
>

### <a name="check-manual-or-scripted"></a>검사(수동 또는 스크립트)
이 검사 단계에서는 앞에서 다운로드한 구성을 사용하여 마이그레이션이 정상적으로 진행되는지 확인할 수 있습니다(선택 사항). 또는 포털에 로그인하고 속성과 리소스에 대해 주요 점검을 수행하여 메타데이터 마이그레이션이 정상적으로 진행되는지 확인합니다.

가상 네트워크를 마이그레이션하는 경우 가상 컴퓨터의 구성이 대부분 다시 시작됩니다. 이러한 VM의 응용 프로그램에 대해 응용 프로그램이 아직 실행 중인지 확인할 수 있습니다.

모니터링/자동화와 작업 스크립트를 테스트하여 VM이 정상적으로 작동하며 업데이트된 스크립트가 올바르게 작동하는지 확인합니다. 리소스가 준비됨 상태인 경우에는 GET 작업만 지원됩니다.

마이그레이션을 언제까지 커밋해야 하는가에 대해 정해진 시간은 없습니다. 이 상태를 원하는 시간 동안 유지할 수 있습니다. 하지만 중단 또는 커밋할 때까지 이러한 리소스에 대한 관리 평면이 잠깁니다.

문제가 발생할 경우 언제든지 마이그레이션을 중단하고 클래식 배포 모델로 돌아갈 수 있습니다. 그럴 경우 클래식 배포 모델에서 해당 VM에서 정상 작업을 재개할 수 있도록 Azure 플랫폼에서 리소스에 대한 관리 평면 작업을 시작합니다.

### <a name="abort"></a>중단
중단 단계에서는 변경 사항을 클래식 배포 모델로 되돌리고 마이그레이션을 중지할 수 있습니다(선택 사항).

> [!NOTE]
> 커밋 작업을 트리거하면 이 작업을 실행할 수 없습니다.     
>
>

### <a name="commit"></a>커밋
유효성 검사를 마친 후 마이그레이션을 커밋할 수 있습니다. 리소스는 클래식에 더 이상 표시되지 않으며 Resource Manager 배포 모델에서만 사용할 수 있습니다. 새 포털에서는 마이그레이션된 리소스만 관리할 수 있습니다.

> [!NOTE]
> 이 작업은 멱등원 작업입니다. 이 작업이 실패하면 작업을 다시 시도하는 것이 좋습니다. 계속 실패할 경우 지원 티켓을 만들거나 [VM 포럼](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)에서 ClassicIaaSMigration 태그를 사용하여 포럼 게시물을 작성할 수 있습니다.
>
>

## <a name="frequently-asked-questions"></a>질문과 대답
**이 마이그레이션 계획이 Azure 가상 컴퓨터에서 실행되는 기존 서비스 또는 응용 프로그램에 영향을 미치나요?**

아니요. VM(클래식)은 일반 공급 제품에서 완전하게 지원되는 서비스입니다. 이러한 리소스를 사용하여 Microsoft Azure에서 작업 공간을 확장할 수 있습니다.

**가까운 미래에 마이그레이션할 계획이 없는 경우 내 VM은 어떻게 됩니까?**

Microsoft는 기존 클래식 API와 리소스 모델을 중단할 계획이 없으며 보다 편리한 마이그레이션을 위해 Resource Manager 배포 모델에서 사용할 수 있는 고급 기능을 고려하고 있습니다. Resource Manager의 IaaS로 구현된 [몇 가지 개선 사항](../azure-resource-manager/resource-manager-deployment-model.md)을 살펴볼 것을 권장합니다.

**이 마이그레이션 계획으로 기존 도구는 어떻게 되나요?**

마이그레이션 계획에서 가장 중요하게 수행해야 할 변경 사항 중 하나는 사용자 도구를 Resource Manager 배포 모델에 맞게 업데이트하는 것입니다.

**관리 평면 가동 중지 시간은 얼마나 되나요?**

마이그레이션하는 리소스의 수에 따라 달라집니다. 소규모 배포(몇 십 대의 VM)의 경우 전체 마이그레이션은 1시간 미만이 소요됩니다. 대규모 배포(수백 대의 VM)의 경우 마이그레이션에 몇 시간이 걸릴 수 있습니다.

**Resource Manager에서 마이그레이션 리소스를 커밋한 다음 롤백할 수 있나요?**

리소스가 준비됨 상태인 경우에만 마이그레이션을 중단할 수 있습니다. 커밋 작업을 사용하여 리소스를 성공적으로 마이그레이션한 후에는 롤백이 지원되지 않습니다.

**커밋 작업이 실패한 경우 마이그레이션을 롤백할 수 있나요?**

커밋 작업이 실패한 경우 마이그레이션을 중단할 수 없습니다. 커밋 작업을 포함한 모든 마이그레이션 작업은 idempotent 상태입니다. 따라서 짧은 기간 이후 작업을 다시 시도해보는 것이 좋습니다. 그래도 오류가 발생할 경우 지원 티켓을 만들거나 [VM 포럼](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)에서 ClassicIaaSMigration 태그로 포럼 게시물을 작성할 수 있습니다.

**Resource Manager에서 IaaS를 활용해야 할 경우 다른 Express 경로 회로를 구매해야 하나요?**

아니요. 최근에 [클래식에서 Resource Manager 배포 모델로의 Express 경로 회로 이동](../expressroute/expressroute-move.md)을 사용하도록 설정했습니다. 이미 Express 경로 회로가 있는 경우 새로 구입하지 않아도 됩니다.

**클래식 IaaS 리소스에 대해 역할 기반 액세스 제어 정책을 구성한 경우 어떻게 되나요?**

마이그레이션 중에는 리소스가 클래식에서 Resource Manager로 전환됩니다. 따라서 마이그레이션 후 필요한 RBAC 정책 업데이트를 계획하는 것이 좋습니다.

**현재 Azure Site Recovery 또는 Azure 백업을 사용하고 있으면 어떻게 되나요?**

백업이 설정된 가상 컴퓨터를 마이그레이션하려면 [백업 자격 증명 모음에 내 클래식 VM을 백업했습니다. 이제 클래식 모드에서 Resource Manager 모드로 내 VM을 마이그레이션하려고 합니다. 복구 서비스 자격 증명 모음에서 어떻게 백업하나요?](../backup/backup-azure-backup-ibiza-faq.md)백업 자격 증명 모음에서 클래식 VM을 백업했습니다. 이제 클래식 모드에서 Resource Manager 모드로 내 VM을 마이그레이션하려고 합니다.  복구 서비스 자격 증명 모음에서 백업하려면 어떻게 해야 하나요?

**내 구독 또는 리소스에서 마이그레이션이 가능한지 확인할 수 있나요?**

예. 플랫폼 지원 마이그레이션 옵션에서 마이그레이션을 준비하는 첫 번째 단계는 리소스를 마이그레이션할 수 있는지 여부를 확인하는 것입니다. 유효성 검사 작업이 실패하는 경우, 마이그레이션을 완료할 수 없는 모든 원인에 대한 모든 메시지가 나타납니다.

**마이그레이션을 위해 IaaS 리소스를 준비하는 동안 할당량 오류가 발생하면 어떻게 되나요?**

마이그레이션을 중단한 다음 지원 요청을 로깅하여 VM을 마이그레이션하고 있는 지역에서 할당량을 늘리는 것이 좋습니다. 할당량 요청이 승인되면 마이그레이션 단계를 다시 실행할 수 있습니다.

**문제를 보고하려면 어떻게 해야 하나요?**

키워드 ClassicIaaSMigration을 사용하여 [VM 포럼](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)에 마이그레이션에 대한 문제와 질문을 게시하세요. 이 포럼에 모든 질문을 게시하는 것이 좋습니다. 지원 계약을 맺은 경우 지원 티켓을 로깅해도 좋습니다.

**마이그레이션 중 플랫폼이 선택한 리소스 이름이 마음에 들지 않으면 어떻게 하나요?**

클래식 배포 모델에서 명시적으로 이름을 입력한 모든 리소스는 마이그레이션 중 해당 이름이 보존됩니다. 새 리소스가 생성되는 경우도 있습니다. 예를 들어 모든 VM에 대해 네트워크 인터페이스가 생성됩니다. 현재는 마이그레이션 중 이와 같이 새로 생성된 리소스 이름을 제어하는 기능을 지원하지 않습니다. [Azure 사용자 의견 포럼](http://feedback.azure.com)에서 이 기능에 대한 투표를 실시하세요.

**메시지를 받았습니다.*"VM은 전반적인 에이전트 상태를 준비되지 않음으로 보고합니다. 따라서 VM은 마이그레이션할 수 없습니다. VM 에이전트가 전반적인 에이전트 상태를 준비된 상태"*로 보고하고 있는지 또는 *"VM에서 보고되지 않은 상태의 확장이 VM에 포함되어 있는지 확인합니다. 따라서 이 VM은 마이그레이션할 수 없습니다."***

VM이 인터넷에 아웃바운드 연결하지 못하는 경우 이 메시지가 수신됩니다. VM 에이전트는 아웃 바운드 연결을 사용하여 Azure 저장소 계정에 연결해 5분 마다 에이전트 상태를 업데이트합니다.

## <a name="next-steps"></a>다음 단계
클래식 IaaS 리소스를 Resource Manager로 마이그레이션하는 작업을 이해했으므로 리소스 마이그레이션을 시작할 수 있습니다.

* [클래식에서 Azure Resource Manager로의 플랫폼 지원 마이그레이션에 대한 기술 정보](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [PowerShell을 사용하여 클래식에서 Azure Resource Manager로 IaaS 리소스 마이그레이션](virtual-machines-windows-ps-migration-classic-resource-manager.md)
* [CLI를 사용하여 클래식에서 Azure Resource Manager로 IaaS 리소스 마이그레이션](virtual-machines-linux-cli-migration-classic-resource-manager.md)
* [커뮤니티 PowerShell 스크립트를 사용하여 클래식 가상 컴퓨터를 Azure Resource Manager로 복제](virtual-machines-windows-migration-scripts.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [가장 일반적인 마이그레이션 오류 검토](virtual-machines-migration-errors.md)



<!--HONumber=Nov16_HO4-->


