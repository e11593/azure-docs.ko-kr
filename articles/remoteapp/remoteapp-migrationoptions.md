---
title: "Azure RemoteApp에서 마이그레이션하기 위한 옵션 | Microsoft Docs"
description: "Azure RemoteApp에서 마이그레이션하기 위한 옵션에 대해 알아봅니다."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c4e0e5bc-5c13-4487-b1b6-ebf2a5edc1f0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 0af5a4e2139a202c7f62f48c7a7e8552457ae76d
ms.openlocfilehash: d12ccdc13d6964a6de8068a63f945c7eac40b682


---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Azure RemoteApp에서 마이그레이션하기 위한 옵션
> [!IMPORTANT]
> Azure RemoteApp은 중단될 예정입니다. 자세한 내용은 [알림](https://go.microsoft.com/fwlink/?linkid=821148) 을 읽어보세요.
> 
> 

[사용 중지 알림](https://go.microsoft.com/fwlink/?linkid=821148) 또는 평가를 완료했기 때문에 Azure RemoteApp 사용을 중단했으면 Azure RemoteApp을 다른 앱 서비스로 마이그레이션해야 합니다. 마이그레이션에는 두 가지 방식, 즉 자체 관리형 솔루션(종종 IaaS(Infrastructure as a Service)라고 함) 배포 또는 완전 관리형 제품(종종 PaaS(Platform as a Service)/SaaS(Software as a Service)라고 함)이 있습니다. 

자체 관리형 IaaS는 VM(가상 컴퓨터) 또는 실제 시스템에 직접 배포되어 사용자가 관리, 운영 및 소유하는 자체(DIY) 배포입니다. 다른 한편으로 완전 관리형 PaaS/SaaS 제품은 Azure RemoteApp과 매우 비슷합니다. 즉 파트너는 운영 및 서비스를 처리하는 원격 솔루션 위에 서비스 계층을 제공하는 반면 고객은 이미지 및 앱 관리를 일부 수행합니다.

다른 호스팅 옵션의 예를 포함하여 자세한 정보를 참조하세요.    

## <a name="self-managed-iaas-solutions"></a>자체 관리형 솔루션(IaaS)
### <a name="rds-on-iaas"></a>**IaaS의 RDS**
RemoteApp이나 온-프레미스 또는 호스팅된 환경에 속한 데스크톱(예: Azure VM)을 사용하면 네이티브 세션 기반 원격 데스크톱 서비스(Windows Server) 배포본을 배포할 수 있습니다. RDS 배포에 이미 익숙하고 기존 기술 전문 지식을 갖춘 고객에게는 IaaS에 대한 RDS 배포가 가장 적합합니다. 

> [!NOTE]
> 이 배포 옵션을 사용하려면 RDS 클라이언트 액세스 라이선스로서 SA(Software Assurance)가 있는 볼륨 라이선스가 필요합니다.
> 
> 

Azure VM에 RDS를 배포하는 것은 템플릿을 배포/패치하는 것보다 쉽습니다([개요](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) 참조 및 [가져오기](https://aka.ms/rdautomation)). 더 많은 사용자 지정 및 구성 옵션이 있지만 [크기 자동 조정 스크립트](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76)를 사용하면 Azure RemoteApp 내에서 Azure 클래식 배포 모델 리소스(Azure Resource Model 리소스가 아님)와 동일한 탄력적인 크기 조정 기능을 가져올 수 있습니다. Azure VM에 RDS를 배포하는 경우 Azure RemoteApp을 지원했던 것처럼 전문적인 [Azure 지원](https://azure.microsoft.com/support/plans/)을 통해 지원됩니다. [Azure 지원](https://azure.microsoft.com/support/plans/)에 문의하여 기존 사용량에 기반한 견적 비용을 확인하거나 곧 출시될 비용 계산기를 통해 직접 계산할 수 있습니다.  또한 N 계열 VM(현재 비공개 미리 보기로 있음)을 사용하면 vGPU를 추가할 수 있습니다. vGPU를 추가하는 방법과 [Windows Server 2016의 RDS 향상 기능 활용](https://myignite.microsoft.com/videos/2794) 방법을 자세히 참조하세요.   

[Windows Server 2012 R2](http://aka.ms/rdsonazure) 및 [Windows Server 2016](http://aka.ms/rdsonazure2016)에 대한 단계별 배포 가이드를 통해 배포를 지원하고 있습니다. [원격 데스크톱 블로그](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services)에서 최신 소식을 확인하세요.

### <a name="citrix-on-iaas"></a>**IaaS의 Citrix**
세션 기반 XenApp 또는 XenDesktop의 네이티브 Citrix 배포본은 온-프레미스 또는 호스팅된 환경(예: Azure VM)에 배포할 수 있습니다. 

자세한 내용은 [Azure에서 Citrix XA 7.6](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx) 단계별 배포 가이드를 확인하세요. 가격 계산기를 포함하여 [Azure에서 Citrix](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx)에 대해 자세히 알아보세요. [Citrix 연락처](http://citrix.com/English/contact/index.asp)를 사용하면 옵션에 대해 논의할 수도 있습니다.

## <a name="fully-managed-paassaas-offerings"></a>완전 관리형 제품(PaaS/SaaS)
### <a name="citrix-cloud"></a>**Citrix Cloud**
[기존 Citrix 클라우드 솔루션](https://www.citrix.com/products/citrix-cloud/)은 Citrix XenApp Express와 구조적으로 동일합니다. Citrix는 기존 Azure RemoteApp 고객을 위해 [50% 할인 프로모션](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/)을 제공하고 있습니다. 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express(기술 미리 보기)**
[자체 기술 미리 보기에서 등록](http://now.citrix.com/remoteapp)(영문)을 수행하고 [Ignite 세션](https://myignite.microsoft.com/videos/2792)(20분 30초 시점에서 시작)을 살펴봅니다. XenApp Express는 간소화된 관리 UI 및 Azure RemoteApp과 비슷한 다른 기능을 포함한다는 점을 제외하고는 Citrix Cloud와 구조적으로 동일합니다. 

[Citrix XenApp Express](http://now.citrix.com/remoteapp)에 대해 자세히 알아보세요.   

### <a name="citrix-service-provider-program"></a>**Citrix 서비스 공급자 프로그램**
Citrix 서비스 공급자 프로그램을 사용하면 서비스 공급자가 가상 클라우드 컴퓨팅의 단순성을 SMB에 제공하여 간편한 종량제 모델에서 원하는 서비스를 손쉽게 제공할 수 있습니다. Citrix 서비스 공급자는 모든 장치에서 어디서든 액세스 가능, 광범위한 응용 프로그램 지원, 풍부한 경험, 보안 강화 및 향상된 확장성을 통해 Microsoft SPLA 비즈니스를 성장시키고 RDS 플랫폼 투자를 확대합니다. 이에 따라 Citrix 서비스 공급자는 구독자를 더 많이 유치하며, 고객 만족도를 높이고, 운영 비용을 절감합니다. [자세한 정보](http://www.citrix.com/products/service-providers.html) 또는 [파트너 찾기](https://www.citrix.com/buy/partnerlocator.html)

### <a name="microsoft-hosted-service-provider"></a>**Microsoft 호스티드 서비스 공급자**
호스팅 파트너는 일반적으로 완전히 관리되는 호스팅된 Windows 데스크톱 및 응용 프로그램 서비스를 제공합니다. 여기에는 Azure 리소스, 운영 체제, 응용 프로그램 및 기술 지원팀이 포함되며, Microsoft 및 기타 소프트웨어 공급자와 함께 파트너 라이선스 계약을 통해 SAL(Subscriber Access License)의 재판매를 허용하는 서비스 공급자 라이센스 계약을 체결해야 합니다. 다음 정보는 Azure RemoteApp 마이그레이션으로 고객을 지원하는 데 전문화된 일부 호스팅 업체의 세부 정보 및 연락처 정보를 제공합니다. IaaS 학습 경로 및 평가에서 RDS를 완료한 [호스티드 서비스 공급자의 현재 목록](http://aka.ms/rdsonazurecertified)을 확인하세요.  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en)는 ISV(Independent Software Vendor)에서 클라우드로 전환하여 현재의 클라우드 설정을 최적화하는 것을 전문적으로 다루고 있습니다. ASPEX에서는 광범위한 관리, 개발 및 컨설팅 서비스를 제공합니다.  

기본 위치: 벨기에 앤트워프

작업 영역: 서유럽

파트너 관계 상태: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Microsoft 클라우드 서비스 공급자: 예

세션 기반 RemoteApp 및 데스크톱 솔루션 제공: 예, 둘 다

Azure RemoteApp 마이그레이션 솔루션: 예, [자세한 정보](https://www.aspex.be/en/azure-remote-apps)

**연락처:**

* 전화 번호: +3232202198
* 메일: [info@aspex.be](mailto:info@aspex.be)
* 웹: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink(플랫폼 이름: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com)는 IT 회사에서 Microsoft Azure Cloud의 원격 데스크톱, 원격 응용 프로그램 및 인프라에 대한 마이그레이션 및 배포를 단순화, 최적화 및 확장할 수 있도록 하는 자동화 플랫폼입니다. 

MyCloudIT 플랫폼을 사용하면 배포 시간을 95% 단축하며, Azure 비용을 30% 절감하고, 몇몇의 키 입력으로 고객의 전체 IT 인프라를 클라우드로 옮길 수 있습니다. 이제 파트너는 하나의 전역 대시보드에서 고객을 관리하고, 이전과는 완전히 다르게 전 세계의 최종 사용자에게 서비스를 제공하며, 추가 오버 헤드 또는 광범위한 Azure 교육을 추가하지 않고도 수익을 올릴 수 있습니다.  

기본 위치: 미국 텍사스주 댈러스

작업 영역: 전 세계

파트너 관계 상태: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Microsoft 클라우드 서비스 공급자: 예

세션 기반 RemoteApp 및 데스크톱 솔루션 제공: 예, 둘 다

Azure RemoteApp 마이그레이션 솔루션: 예, [자세한 정보](https://mycloudit.com/remote-app-microsoft/)

**연락처:**

* Brian Garoutte, 비즈니스 개발 부사장
  
   전화 번호: 972-218-0741
  
   메일: [brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com)는 호스팅된 데스크톱 솔루션을 제공하고, Azure 및 자체 데이터 센터의 전역 클라이언트에 기반하여 Microsoft 기술로 구현되는 완벽한 데스크톱 및 ISV 응용 프로그램 환경을 배포하는 것을 전문적으로 다루고 있습니다.

기본 위치: 영국 런던, 싱가포르, 미국 텍사스주 휴스턴

작업 영역: 전 세계

파트너 관계 상태: Gold

Microsoft 클라우드 서비스 공급자: 예

세션 기반 RemoteApp 및 데스크톱 솔루션 제공: 예, 둘 다

Azure RemoteApp 마이그레이션 솔루션: 예, [자세한 정보](http://www.acuutech.com/ara-migration/)

**연락처:**

* 영국:
  
  5/6 York House, Langston Road,
  
  Loughton, Essex IG10 3TQ
  
  전화 번호: +44 (0) 20 8502 2155
* 싱가포르:
  
  100 Cecil Street, #09-02, 
  
  The Globe, Singapore 069532
  
  전화 번호: +65 6709 4933
* 북아메리카: 
  
  3601 S. Sandman St.
  
  Suite 200, Houston, TX 77098
  
  전화 번호: +1 713 691 0800

#### <a name="saasplaza"></a>**SaaSplaza**
[SaaSplaza](http://www.saasplaza.com/)는 모든 Microsoft Dynamics 포트폴리오(NAV, AX, GP, SL, CRM) 개인 및 공용 클라우드(Azure)를 제공 합니다.

기본 위치: 네덜란드

작업 영역: 전 세계

파트너 관계 상태: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)

Microsoft 클라우드 서비스 공급자: 예

세션 기반 RemoteApp 및 데스크톱 솔루션 제공: 예, 둘 다

**연락처:**

- EMEA:

   Prins Mauritslaan 29-35

   71 LP Badhoevedorp

   네덜란드

   전화 번호: +31 20 547 8060 

- 아메리카:

   171 Saxony Road, Suite 105

   Encinitas, CA 92024

   샌디에이고

   미국

   전화 번호: +1 858 385 8900 

- APAC:

   105 Cecil Street
   
   \#11-08, The Octagon

   Singapore 069534

   싱가포르
   
   전화 번호 - 싱가포르: + 65 6222 6591

   전화 번호 - 오스트레일리아: +61 2 8310 5568 
   
   전화 번호 - 뉴질랜드: +64 4 488 0321

## <a name="need-more-help"></a>도움이 더 필요하세요?
여전히 도움이 필요하거나 추가 질문이 있나요? 다음 방법 중 하나를 사용하여 도움을 받습니다. 

1. [arainfo@microsoft.com](mailto:arainfo@microsoft.com)으로 메일을 보내세요.
2. [Azure 지원](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)에 문의하세요. [Azure 지원 사례](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)를 열면서 시작합니다.
3. 전화로 문의하세요. [해당 지역 영업 담당자 전화번호 찾기](https://azure.microsoft.com/overview/sales-number/)




<!--HONumber=Dec16_HO2-->


