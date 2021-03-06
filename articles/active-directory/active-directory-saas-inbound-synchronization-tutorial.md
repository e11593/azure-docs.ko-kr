---
title: "자습서: 인바운드 동기화를 위한 Workday 구성 | Microsoft Docs"
description: "Azure Active Directory에서 인바운드 동기화를 사용하여 Single Sign-On, 자동화된 프로비저닝 등을 사용하도록 설정하는 방법을 알아봅니다."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8fe96f0a-f142-4d66-b53d-3ac3eb41a661
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/06/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 7f45241e96df7e0381d775afc9a5cfa36fca49c4


---
# <a name="tutorial-configuring-workday-for-inbound-synchronization"></a>자습서: 인바운드 동기화를 위한 Workday 구성
> [!NOTE]
> Azure AD(Active Directory) Premium은 Azure AD의 전 세계 인스턴스를 사용하여 중국의 고객에게 제공됩니다.    
> 중국의 21Vianet에서 운영하는 Microsoft Azure 서비스는 현재 Azure AD Premium을 지원하지 않습니다.    
> 
> 

이 자습서는 Workday에서 Microsoft Azure AD로 사람들을 가져오기 위해 Workday 및 Microsoft Azure AD에서 수행해야 하는 단계를 설명하기 위한 것입니다.    
 이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 항목이 있다고 가정합니다.  

* 유효한 Azure 구독  
* Workday 내의 테넌트  

이 자습서에 설명된 시나리오는 다음 구성 요소로 이루어져 있습니다.  

1. Workday에 응용 프로그램 통합 사용  
2. 통합 시스템 사용자 만들기  
3. 보안 그룹 만들기  
4. 보안 그룹에 통합 시스템 사용자 할당  
5. 보안 그룹 옵션 구성  
6. 보안 정책 변경 사항 활성화  
7. Microsoft Azure AD에서 사용자 가져오기 구성  

## <a name="enabling-the-application-integration-for-workday"></a>Workday에 응용 프로그램 통합 사용
이 섹션에서는 Workday에 응용 프로그램 통합을 사용하도록 설정하는 방법을 간략하게 설명합니다.    

### <a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Workday에 응용 프로그램 통합을 사용하도록 설정하려면
1. Azure 관리 포털의 왼쪽 탐색 창에서 **Active Directory**를 클릭합니다.    
   
   ![Active Directory](./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")  
2. **디렉터리** 목록에서 디렉터리 통합을 사용하도록 설정할 디렉터리를 선택합니다.    
3. 응용 프로그램 보기를 열려면 디렉터리 보기의 최상위 메뉴에서 **응용 프로그램** 을 클릭합니다.    
   
   ![응용 프로그램](./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Applications")  
4. **응용 프로그램 갤러리**를 열려면 **앱 추가**를 클릭한 다음 **조직에서 사용할 응용 프로그램 추가**를 클릭합니다.    
   
   ![원하는 작업을 선택하세요.](./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "What do you want to do?")  
5. **검색 상자**에 **Workday**를 입력합니다.    
   
   ![Workday](./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Workday")  
6. 결과 창에서 **Workday**를 선택하고 **완료**를 클릭하여 응용 프로그램을 추가합니다.    
   
   ![Workday](./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Workday")  

## <a name="creating-an-integration-system-user"></a>통합 시스템 사용자 만들기
1. **Workday Workbench**의 검색 상자에서 **사용자 만들기**를 입력하고 **통합 시스템 사용자 만들기** 링크를 클릭합니다.     
   
   ![사용자 만들기](./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "create user")  
2. 새 통합 시스템 사용자에 대한 사용자 이름과 암호를 입력하여 통합 시스템 사용자 만들기 작업을 완료합니다.  이 사용자는 프로그래밍 방식으로 로그인할 것이므로 다음 로그인할 때 새 암호 필요 옵션을 선택하지 않습니다.    
   사용자의 세션이 너무 빨리 시간 초과되지 않도록 세션 시간 초과 분을 기본값인 0으로 둡니다.    
   
   ![통합 시스템 사용자 만들기](./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Create Integration System User")  

## <a name="creating-a-security-group"></a>보안 그룹 만들기
이 자습서에서 설명하는 시나리오의 경우 무제한 통합 시스템 보안 그룹을 만들고 사용자를 할당해야 합니다.    

1. 검색 상자에 보안 그룹 만들기를 입력하고 보안 그룹 만들기 링크를 클릭합니다.     
   
   ![보안 그룹 만들기](./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "CreateSecurity Group")  
2. 보안 그룹 만들기 작업을 완료합니다.  멤버를 명시적으로 추가할 보안 그룹을 만들려면 테넌트 보안 그룹의 유형에서 통합 시스템 보안 그룹 - 제한되지 않음을 선택합니다.     
   
   ![보안 그룹 만들기](./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "CreateSecurity Group")  

## <a name="assigning-the-integration-system-user-to-the-security-group"></a>보안 그룹에 통합 시스템 사용자 할당
1. 검색 상자에 보안 그룹 편집을 입력하고 **보안 그룹 편집** 링크를 클릭합니다.     
   
   ![보안 그룹 편집](./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Edit Security Group")  
2. 이름으로 새 통합 보안 그룹을 검색하고 선택합니다.    
   
   ![보안 그룹 편집](./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Edit Security Group")  
3. 새 보안 그룹에 새 통합 시스템 사용자 할당       
   
   ![시스템 보안 그룹](./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "System Security Group")  

## <a name="configuring-security-group-options"></a>보안 그룹 옵션 구성
이 단계에서는 다음 도메인 보안 정책에 의해 보호되는 개체에 대한 Get 및 Put 작업을 위해 새 보안 그룹 사용 권한을 할당합니다.  

* 외부 계정 프로비저닝  
* 작업자 데이터: 공용 작업자 보고서  
* 작업자 데이터: 모든 위치  
* 작업자 데이터: 현재 인력 관리 정보  
* 작업자 데이터: 작업자 프로필 직함  

&nbsp;  

1. 검색 상자에 도메인 보안 정책을 입력하고 기능 영역에 대한 보안 정책 링크를 클릭합니다.     
   
   ![도메인 보안 정책](./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Domain Security Policies")  
2. 시스템을 검색하여 시스템 기능 영역을 선택합니다.  확인이라는 레이블이 지정된 단추를 클릭합니다.     
   
   ![도메인 보안 정책](./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Domain Security Policies")  
3. 시스템 기능 영역에 대한 보안 정책 목록에서 보안 관리를 확장하고,도메인 보안 정책, 외부 계정 프로비저닝을 선택합니다.     
   
   ![도메인 보안 정책](./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Domain Security Policies")  
4. 사용 권한 편집 단추를 클릭한 다음, 사용 권한 편집 화면에서 Get 및 Put 통합 사용 권한이 있는 보안 그룹의 목록에 새 보안 그룹을 추가합니다.     
   
   ![편집 권한](./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Edit Permission")  
5. 1단계를 반복하여 기능 영역을 선택하는 화면으로 돌아간 다음, 이번에는 인력 관리를 검색하고 인력 관리 기능 영역을 선택한 다음 확인이라는 레이블이 지정된 단추를 클릭합니다.    
   
   ![도메인 보안 정책](./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Domain Security Policies")  
6. 인력 관리 영역에 대한 보안 정책 목록에서 작업자 데이터: 인력 관리를 확장하고 다음과 같은 남은 각 보안 정책에 대해 위의 4단계를 반복합니다.    
   
   * 작업자 데이터: 공용 작업자 보고서  
   * 작업자 데이터: 모든 위치  
   * 작업자 데이터: 현재 인력 관리 정보  
   * 작업자 데이터: 작업자 프로필 직함    
   
   ![도메인 보안 정책](./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Domain Security Policies")  

## <a name="activating-security-policy-changes"></a>보안 정책 변경 사항 활성화
1. 검색 상자에 활성화를 입력하고 보류 중인 보안 정책 변경 내용 활성화 링크를 클릭합니다.    
   
   ![활성화](./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Activate")  
2. 감사 목적에 대한 메모를 입력하고 확인이라는 레이블이 지정된 단추를 클릭하여 보류 중인 보안 정책 변경 내용의 활성화 작업을 시작합니다.      
   
   ![보류 중인 보안 활성화](./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Activate Pending Security")  
3. 다음 화면에서 확인이라는 레이블이 지정된 확인란을 선택하고 확인이라는 레이블이 지정된 단추를 클릭하여 작업을 완료합니다.     
   
   ![보류 중인 보안 활성화](./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Activate Pending Security")  

## <a name="configuring-user-import-in-microsoft-azure-ad"></a>Microsoft Azure AD에서 사용자 가져오기 구성
이 섹션에서는 Workday에서 사람들을 가져오기 위해 Microsoft Azure AD를 구성하는 방법을 설명합니다.    

### <a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Microsoft Azure AD에서 사용자 가져오기를 구성하려면 다음 단계를 수행합니다.
1. **Workday** 응용 프로그램 통합 페이지에서 **사용자 가져오기 구성**을 클릭하여 **프로비저닝 구성** 대화 상자를 엽니다.    
2. **설정 및 관리자 자격 증명** 페이지에서 다음 단계를 수행하고 다음을 클릭합니다.    
   
   ![설정 및 관리자 자격 증명](./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Settings and admin credentials")    
   
   1. **Workday 관리자 이름** 텍스트 상자에 [통합 시스템 사용자 만들기](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) 섹션에서 만든 사용자의 이름을 입력합니다.    
   2. **Workday 관리자 암호** 텍스트 상자에 [통합 시스템 사용자 만들기](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) 섹션에서 만든 사용자의 암호를 입력합니다.    
   3. **Workday 테넌트 URL** 텍스트 상자에 Workday 테넌트의 URL을 입력합니다.    
3. **연결 테스트** 페이지에서 **테스트 시작**을 클릭하여 연결을 확인하고 **다음**을 클릭합니다.    
   
   ![연결 테스트](./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Test connection")  
4. **프로비저닝 옵션** 페이지에서 **다음**을 클릭합니다.    
   
   ![프로비저닝 옵션](./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Provisioning options")  
5. **프로비저닝 시작** 대화 상자에서 **완료**를 클릭합니다.    
   
   ![프로비저닝 시작](./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Start provisioning")  

이제 **사용자** 섹션으로 이동하여 Workday 사용자를 가져왔는지 확인할 수 있습니다.    




<!--HONumber=Nov16_HO3-->


