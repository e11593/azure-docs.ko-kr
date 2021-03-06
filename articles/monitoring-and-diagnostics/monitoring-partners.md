---
title: "Azure Monitor 파트너 통합 | Microsoft Docs"
description: "Azure Monitor의 파트너와, 파트너 통합을 위한 설명서에 액세스하는 방법을 살펴봅니다."
author: johnkemnetz
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 01ee13ac-66fc-4edc-8b0c-32f69b986a26
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: johnkem
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: ebbd4166bc3f76c91823ee17b8b9c460feb9e194


---
# <a name="azure-monitor-partner-integrations"></a>Azure Monitor 파트너 통합
| 파트너 |  |  |
| --- | --- | --- |
| [![Partner Logo][alertlogic-logo]<br/>**AlertLogic**][alertlogic-anchor] |[![Partner Logo][appdynamics-logo]<br/>**AppDynamics**][appdynamics-anchor] |[![Partner Logo][atlassian-logo]<br/>**Atlassian**][atlassian-anchor] |
| [![Partner Logo][cloudmonix-logo]<br/>**CloudMonix**][cloudmonix-anchor] |[![Partner Logo][cloudyn-logo]<br/>**Cloudyn**][cloudyn-anchor] |[![Partner Logo][datadog-logo]<br/>**DataDog**][datadog-anchor] |
| [![Partner Logo][dynatrace-logo]<br/>**Dynatrace**][dynatrace-anchor] |[![Partner Logo][newrelic-logo]<br/>**NewRelic**][newrelic-anchor] |[![Partner Logo][opsgenie-logo]<br/>**OpsGenie**][opsgenie-anchor] |
| [![Partner Logo][pagerduty-logo]<br/>**PagerDuty**][pagerduty-anchor] |[![Partner Logo][splunk-logo]<br/>**Splunk**][splunk-anchor] |[![Partner Logo][sumologic-logo]<br/>**Sumo Logic**][sumologic-anchor] |

## <a name="alertlogic-log-manager"></a>AlertLogic Log Manager
Alert Logic Log Manager는 보안 분석 및 보존을 위해 VM, 응용 프로그램 및 Azure 플랫폼 로그를 수집합니다. 여기에는 Azure Monitor API를 통한 Azure 감사 로그가 포함됩니다.  이 정보는 부정 행위를 탐지하고 규정 준수 요구 사항에 부합하는 데 사용됩니다.

[설명서로 이동합니다.][alertlogic-doc]

## <a name="appdynamics"></a>AppDynamics
AppDynamics APM(Application Performance Management)을 사용하면 응용 프로그램 소유자가 성능 병목 문제를 신속히 해결하고 Azure 환경에서 실행되는 응용 프로그램의 성능을 최적화할 수 있습니다. AppDynamics APM은 Azure Marketplace와 자연스럽게 통합되며 Azure Cloud Service(PaaS, 웹 및 작업자 역할 포함), 가상 컴퓨터(IaaS), 원격 서비스 탐지(Microsoft Azure Service Bus), Microsoft Azure Queue Microsoft Azure Remote Services(Azure Blob), Azure Queue (Microsoft Service Bus), 데이터 저장소, Microsoft Azure Blob Storage 모니터링에 사용할 수 있습니다.

[설명서로 이동합니다.][appdynamics-doc]

## <a name="atlassian-jira"></a>Atlassian JIRA
Azure Monitor 경고에 JIRA 티켓을 만들 수 있습니다.

[설명서로 이동합니다.][atlassian-doc]

## <a name="cloudmonix"></a>CloudMonix
CloudMonix는 Microsoft Azure 플랫폼을 위한 모니터링, 자동화 및 자동 복구 서비스를 제공합니다.

[설명서로 이동합니다.][cloudmonix-doc]

## <a name="cloudyn"></a>Cloudyn
Cloudyn은 기업이 클라우드 기능을 완전히 구현할 수 있도록 다중 플랫폼 하이브리드 클라우드 배포를 관리 및 최적화합니다. SaaS 솔루션은 Insights와 함께 사용, 성능 및 비용에 대한 정보와, 스마트한 최적화와 클라우드 관리를 위해 조치 가능한 권장 사항을 제공합니다. Cloudyn을 사용하면 정확한 비용 청구와 계층적 비용 할당 관리를 통해 책임을 밝힐 수 있습니다. Cloudyn은 Azure 배포 최적화를 위한 정보 및 조치 가능한 권장 사항을 제공하기 위해 Azure Monitoring과 통합됩니다.

[설명서로 이동합니다.][cloudyn-doc]

## <a name="datadog"></a>DataDog
Datadog는 클라우드급 응용 프로그램을 위한 세계 선두의 모니터링 서비스로, 서버, 데이터베이스, 도구 및 서비스의 데이터를 하나로 모아 전체 스택에 대한 통합된 뷰를 제시합니다. 이 기능은 SaaS 기반 데이터 분석 플랫폼에서 제공되므로 개발 및 운영 팀이 협업하여 중단 시간을 방지하고, 성능 문제를 해결하며, 개발 및 개발 주기가 제 때에 끝날 수 있게 할 수 있습니다. Datadog와 Azure를 통합하면 전체 인프라에서 메트릭을 수집 및 파악하고, VM 메트릭과 응용 프로그램 수준 메트릭을 상관하며, 속성 및 사용자 지정 태그의 조합을 통해 메트릭을 상세 분석할 수 있습니다.

[설명서로 이동합니다.][datadog-doc]

## <a name="dynatrace"></a>Dynatrace
Dynatrace OneAgent는 해당하는 Azure 확장 메커니즘을 통해 Azure VM 및 App Services와 통합됩니다.
이러한 방식으로 호스트, 네트워크 및 서비스에 대한 성능 메트릭을 수집할 수 있습니다.
메트릭을 단순히 표시하는 것 외에도 전체 환경을 시각화하여 클라이언트 측에서 데이터베이스 계층으로 가는 트랜잭션을 나타낼 수 있습니다.
문제의 AI 기반 상관과, 메서드 수준 코드 및 데이터베이스 정보를 포함하여 완전 통합된 근본 원인 분석을 통해 문제 해결과 성능 최적화가 매우 간편해집니다.

[설명서로 이동합니다.][dynatrace-doc]

## <a name="newrelic"></a>NewRelic
[자세히 알아봅니다][newrelic-doc].

## <a name="opsgenie"></a>OpsGenie
OpsGenie는 Azure가 생성한 경고의 디스패처 역할을 합니다. OpsGenie는 호출 예약 및 에스컬레이션에 따라 이메일, 문자 메시지(SMS), 전화, 푸시 알림 등을 사용하여 알림을 받을 적합한 사람을 판단합니다. 간단히 말해, Azure는 탐지된 문제에 대한 경고를 생성하고 OpsGenie가 그에 대해 작업할 적합한 사람을 확인합니다.

[설명서로 이동합니다.][opsgenie-doc]

## <a name="pagerduty"></a>PagerDuty
선두적인 사건 관리 솔루션인 PagerDuty는 Azure Alerts의 메트릭에 대해 최고 수준의 지원을 제공합니다. 현재 PagerDuty는 Azure Applications Azure Monitor 경고, 자동 크기 조정 알림 및 감사 로그 이벤트에 대한 알림과, Azure 서비스에 대한 플랫폼 수준 메트릭에 대한 알림을 지원합니다. 이렇게 향상된 기능을 통해 사용자는 더 많은 핵심 Azure 플랫폼 정보를 파악하는 동시에 PagerDuty의 사건 관리 기능을 완벽하게 활용하여 실시간으로 대처할 수 있습니다. 확장된 Azure 통합은 Webhook를 통해 구현되어 더 빠르고 간편한 설정과 사용자 지정이 가능합니다.

[설명서로 이동합니다.][pagerduty-doc]

## <a name="splunk-add-on-for-microsoft-cloud-services"></a>Microsoft 클라우드 서비스를 위한 Splunk 추가 기능
Microsoft 클라우드 서비스를 위한 Splunk 추가 기능은 [여기 Splunkbase](https://splunkbase.splunk.com/app/3110/)에서 사용할 수 있습니다.

[설명서로 이동합니다.][splunk-doc]

## <a name="sumo-logic"></a>Sumo Logic
Sumo Logic은 안전한 클라우드 기반, 컴퓨터 데이터 분석 서비스이며, 전체 응용 프로그램 수명 주기와 스택을 통해 구조화, 반구조화 및 구조화되지 않은 데이터에서 실시간 연속 인텔리전스를 전달합니다. 전 세계 1,000명 이상의 고객이 최신 응용 프로그램 및 클라우드 인프라를 빌드, 실행 및 보안하기 위한 예측 분석에 Sumo Logic을 사용합니다. Sumo Logic을 사용하여 고객은 다중 테넌트, 서비스 모델 장점을 이용하여 지속적인 혁신으로 성장을 가속화하여 경쟁 우위, 비즈니스 가치와 성장을 증가시킵니다.

[자세히 알아봅니다][sumologic-doc].

## <a name="next-steps"></a>다음 단계
* [활동 로그(이전의 감사 로그)에 대해 자세히 알아보기](../resource-group-audit.md)
* [Azure 활동 로그를 이벤트 허브로 스트림](monitoring-stream-activity-logs-event-hubs.md)

<!--Connectors Documentation-->
[alertlogic-anchor]: #alertlogic-log-manager "AlertLogic"
[appdynamics-anchor]: #appdynamics "AppDynamics"
[atlassian-anchor]: #atlassian-jira "Atlassian"
[cloudmonix-anchor]: #cloudmonix "CloudMonix"
[cloudyn-anchor]: #cloudyn "Cloudyn"
[datadog-anchor]: #datadog "DataDog"
[dynatrace-anchor]: #dynatrace "Dynatrace"
[newrelic-anchor]: #newrelic "NewRelic"
[opsgenie-anchor]: #opsgenie "OpsGenie"
[pagerduty-anchor]: #pagerduty "PagerDuty"
[splunk-anchor]: #splunk-add-on-for-microsoft-cloud-services "Splunk"
[sumologic-anchor]: #sumo-logic "Sumo Logic"

<!--Icon references-->
[alertlogic-logo]: ./media/partner-logos/alertlogic.png
[appdynamics-logo]: ./media/partner-logos/appdynamics.png
[atlassian-logo]: ./media/partner-logos/atlassian.png
[cloudmonix-logo]: ./media/partner-logos/cloudmonix.png
[cloudyn-logo]: ./media/partner-logos/cloudyn.png
[datadog-logo]: ./media/partner-logos/datadog.png
[dynatrace-logo]: ./media/partner-logos/dynatrace.png
[newrelic-logo]: ./media/partner-logos/newrelic.png
[opsgenie-logo]: ./media/partner-logos/opsgenie.png
[pagerduty-logo]: ./media/partner-logos/pagerduty.png
[splunk-logo]: ./media/partner-logos/splunk.png
[sumologic-logo]: ./media/partner-logos/sumologic.png

<!--Partner Documentation-->
[alertlogic-doc]: https://docs.alertlogic.com/userGuides/log-manager-collection-sources.htm "AlertLogic 설명서."
[appdynamics-doc]: https://docs.appdynamics.com/display/PRO42/Register+for+AppDynamics+for+Windows+Azure "AppDynamics 설명서."
[atlassian-doc]: https://azure.microsoft.com/blog/automated-notifications-from-azure-monitor-for-atlassian-jira/
[cloudmonix-doc]: http://cloudmonix.com/features/azure-management/ "CloudMonix 소개."
[cloudyn-doc]: https://www.cloudyn.com/azure-monitoring "Cloudyn 소개."
[datadog-doc]: http://docs.datadoghq.com/integrations/azure/ "DataDog 설명서."
[dynatrace-doc]: https://blog.ruxit.com/ruxit-monitoring-azure-web-apps/ "Dynatrace 설명서."
[newrelic-doc]: https://newrelic.com/azure "NewRelic 설명서."
[opsgenie-doc]: https://www.opsgenie.com/docs/integrations/azure-integration "OpsGenie 설명서."
[pagerduty-doc]: https://www.pagerduty.com/docs/guides/azure-integration-guide/ "PagerDuty 설명서"
[splunk-doc]: http://docs.splunk.com/Documentation/AddOns/released/MSCloudServices/About "Splunk 설명서."
[sumologic-doc]: https://www.sumologic.com/azure "SumoLogic 설명서"



<!--HONumber=Dec16_HO2-->


