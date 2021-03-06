---
title: "자습서: Work.com와 Azure Active Directory 통합 | Microsoft Docs"
description: "Azure Active Directory에서 Work.com을 사용하여 Single Sign-On, 자동화된 프로비전 등을 사용하도록 설정하는 방법을 알아봅니다."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: f454e7e218764e00cc19ca67b0edade213834b75
ms.openlocfilehash: 69d94659f4eff72e1c449fd915616d81fd4712de


---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>자습서: Work.com과 Azure Active Directory 통합
이 자습서는 Azure와 Work.com의 통합을 보여주기 위한 것입니다.  
이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 항목이 있다고 가정합니다.

* 유효한 Azure 구독
* Work.com Single Sign-On이 설정된 구독

이 자습서를 완료한 후 Work.com에 할당한 Azure AD 사용자가 Work.com 회사 사이트 (서비스 공급자가 시작한 로그온)에서나 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 사용하여 응용 프로그램에 Single Sign-On할 수 있습니다.

이 자습서에 설명된 시나리오는 다음 구성 요소로 이루어져 있습니다.

1. Work.com에 응용 프로그램 통합 사용
2. Single Sign-On 구성
3. 사용자 프로비전 구성
4. 사용자 할당

![시나리오](./media/active-directory-saas-work-com-tutorial/IC794105.png "Scenario")

## <a name="enabling-the-application-integration-for-workcom"></a>Work.com에 응용 프로그램 통합 사용
이 섹션은 Work.com에 응용 프로그램 통합을 사용하도록 설정하는 방법을 간략하게 설명하기 위한 것입니다.

### <a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Work.com에 응용 프로그램 통합을 사용하도록 설정하려면
1. Azure 클래식 포털의 왼쪽 탐색 창에서 **Active Directory**를 클릭합니다.
   
    ![Active Directory](./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2. **디렉터리** 목록에서 디렉터리 통합을 사용하도록 설정할 디렉터리를 선택합니다.

3. 응용 프로그램 보기를 열려면 디렉터리 보기의 최상위 메뉴에서 **응용 프로그램** 을 클릭합니다.
   
    ![응용 프로그램](./media/active-directory-saas-work-com-tutorial/IC700994.png "Applications")

4. 페이지 맨 아래에 있는 **추가** 를 클릭합니다.
   
    ![응용 프로그램 추가](./media/active-directory-saas-work-com-tutorial/IC749321.png "Add application")

5. **수행할 작업** 대화 상자에서 **갤러리에서 응용 프로그램 추가**를 클릭합니다.
   
    ![갤러리에서 응용 프로그램 추가](./media/active-directory-saas-work-com-tutorial/IC749322.png "Add an application from gallerry")

6. **검색 상자**에 **Work.com**을 입력합니다.
   
    ![응용 프로그램 갤러리](./media/active-directory-saas-work-com-tutorial/IC794106.png "Application Gallery")

7. 결과 창에서 **Work.com**을 선택하고 **완료**를 클릭하여 응용 프로그램을 추가합니다.
   
    ![Work.com](./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

## <a name="configuring-single-sign-on"></a>Single Sign-On 구성
이 섹션은 사용자가 SAML 프로토콜 기반 페더레이션을 사용하여 Azure AD의 계정으로 Work.com에 인증할 수 있게 하는 방법을 간략하게 설명하기 위한 것입니다.  
이 절차의 일부로 Work.com으로 인코딩된 인증서 파일을 만들어야 합니다.

> [!NOTE]
> Single Sign-On을 구성하려면 사용자 할당 도메인 이름을 Work.com로 설정해야 합니다. 최소한 한 개의 도메인 이름을 정의하고, 도메인 이름을 테스트 한 후 전체 조직에 배포해야 합니다.
> 
> 

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Single Sign-On을 구성하려면 다음 단계를 수행합니다.
1. 관리자 권한으로 Work.com 테넌트에 로그인합니다.
2. **설정**으로 이동합니다.
   
    ![설정](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3. 왼쪽 탐색창의 **관리** 섹션에서 **도메인 관리**를 클릭해 관련된 섹션을 확장한 다음 **내 도메인**을 클릭해 **내 도메인** 페이지를 엽니다. 
   
    ![내 도메인](./media/active-directory-saas-work-com-tutorial/IC767825.png "My Domain")

4. 도메인이 올바르게 설정되었는지 확인하기 위해 “**4단계 사용자에게 배포**”에 있는지 확인하고 “**내 도메인 설정**”을 검토합니다.
   
    ![사용자에게 배포된 도메인](./media/active-directory-saas-work-com-tutorial/IC784377.png "Doman Deployed to User")

5. 다른 웹 브라우저 창에서 Azure 클래식 포털에 로그인합니다.

6. **Work.com** 응용 프로그램 통합 페이지에서 **Single Sign-On 구성**을 클릭하여 **Single Sign-On 구성** 대화 상자를 엽니다.
   
    ![Single Sign-on 구성](./media/active-directory-saas-work-com-tutorial/IC794109.png "Configure Single Sign-On")

7. **Work.com에 대한 사용자 로그온 방법을 선택하세요.** 페이지에서 **Microsoft Azure AD Single Sign-On**을 선택하고 **다음**을 클릭합니다.
   
    ![Single Sign-On 구성](./media/active-directory-saas-work-com-tutorial/IC794110.png "Configure Single Sign-On")

8. **앱 URL 구성** 페이지의 **Work.com 로그온 URL** 텍스트 상자에 Work.com 응용 프로그램에 로그온하기 위해 사용자가 사용하는 URL(예: ”*http://company.my.salesforce.com*”)을 입력한 후 **다음**을 클릭합니다. 
   
    ![앱 URL 구성](./media/active-directory-saas-work-com-tutorial/IC794111.png "Configure App URL")

9. **Work.com에서 Single Sign-On 구성** 페이지에서 인증서를 다운로드하려면 **인증서 다운로드**를 클릭한 다음 컴퓨터에 로컬로 인증서 파일을 저장합니다.
   
    ![Single Sign-On 구성](./media/active-directory-saas-work-com-tutorial/IC794112.png "Configure Single Sign-On")

10. Work.com 테넌트에 로그인합니다.

11. **설정**으로 이동합니다.
    
    ![설정](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

12. **보안 제어** 메뉴를 확장한 다음 **Single Sign-On 설정**을 클릭합니다.
    
    ![Single Sign-On 설정](./media/active-directory-saas-work-com-tutorial/IC794113.png "Single Sign-On Settings")

13. **Single Sign-On 설정** 대화 상자 페이지에서 다음 단계를 수행합니다.
    
    ![SAML 사용](./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML Enabled")
    
    a. **SAML 사용**을 선택합니다.
    
    b. **새로 만들기**를 클릭합니다.

14. **SAML Single Sign-On 설정** 섹션에서 다음 단계를 수행합니다.
    
    ![SAML Single Sign-On 설정](./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML Single Sign-On Setting")
    
    a. **이름** 텍스트 상자에 구성할 이름을 입력합니다.  
       
    > [!NOTE]
    > **이름**에 값을 제공하면 **API 이름** 텍스트 상자가 자동으로 채워집니다.
    > 
    > 
    
    b. Azure 클래식 포털의 **Work.com에서 Single Sign-On 구성** 대화 상자 페이지에서 **발급자 URL** 값을 복사한 다음 **발급자 URL** 텍스트 상자에 붙여넣습니다.
    
    c. 다운로드한 인증서를 업로드하려면 **찾아보기**를 클릭합니다.
    
    ㄹ. **엔터티 Id** 텍스트 상자에 **https://salesforce-work.com**을 입력합니다.
    
    e. **SAML ID 유형**으로 **사용자 개체에서 페더레이션 ID를 포함하는 어설션**을 선택합니다.
    
    f. **SAML ID 위치**에서 **Subject 문의 NameIdentifier 요소에 ID 포함**을 선택합니다.
    
    g. Azure 클래식 포털의 **Work.com에 대한 Single Sign-On 구성** 대화 상자 페이지에서 **원격 로그인 URL** 값을 복사한 다음 **ID 공급자 로그인 URL** 텍스트 상자에 붙여 넣습니다.
    
    h. Azure 클래식 포털의 **Work.com에 대한 Single Sign-On 구성** 대화 상자 페이지에서 **원격 로그아웃 URL** 값을 복사한 다음 **ID 공급자 로그아웃 URL** 텍스트 상자에 붙여 넣습니다.
    
    i. **서비스 공급자가 시작한 요청 바인딩**에서 **HTTP Post**를 선택합니다.
    
    j. **저장**을 클릭합니다.

15. Work.com 클래식 포털의 왼쪽 탐색창에서 **도메인 관리**를 클릭해 관련된 섹션을 확장한 다음 **내 도메인**을 클릭해 **내 도메인** 페이지를 엽니다. 
    
    ![내 도메인](./media/active-directory-saas-work-com-tutorial/IC794115.png "My Domain")

16. **내 도메인** 페이지의 **로그인 페이지 브랜딩** 섹션에서 **편집**을 클릭합니다.
    
    ![로그인 페이지 브랜딩](./media/active-directory-saas-work-com-tutorial/IC767826.png "Login Page Branding")

17. **로그인 페이지 브랜딩** 페이지의 **인증 서비스** 섹션에 **SAML SSO 설정** 이름이 표시됩니다. 이름을 선택하고 **저장**을 클릭합니다.
    
    ![로그인 페이지 브랜딩](./media/active-directory-saas-work-com-tutorial/IC784366.png "Login Page Branding")

18. Azure 클래식 포털에서 Single Sign-On 구성 확인을 선택하고 **완료**를 클릭하여 **Single Sign-On 구성** 대화 상자를 닫습니다.
    
    ![Single Sign-on 구성](./media/active-directory-saas-work-com-tutorial/IC794116.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>사용자 프로비전 구성
Azure Active Directory 사용자가 로그인하려면, Work.com에 프로비전되어야 합니다.  
Work.com의 경우 프로비전은 수동 작업입니다.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>사용자 프로비저닝을 구성하려면
1. Work.com 회사 사이트에 관리자 권한으로 로그인합니다.

2. **설정**으로 이동합니다.
   
    ![설정](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")
3. **사용자 관리 \> 사용자**로 이동합니다.
   
    ![사용자 관리](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")

4. **새 사용자**를 클릭합니다.
   
    ![모든 사용자](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")

5. 사용자 편집 섹션에서 다음 단계를 수행합니다.
   
    ![사용자 편집](./media/active-directory-saas-work-com-tutorial/IC794118.png "User Edit")
   
    a. 관련된 텍스트 상자에 프로비전할 유효한 Azure Active Directory 계정의 **성**, **별칭**, **이메일**, **사용자 이름** 및 **애칭** 특성을 입력합니다.
   
    b. **역할**, **사용자 라이선스** 및 **프로필**을 차례로 선택합니다.
   
    c. **저장**을 클릭합니다.  
      
    > [!NOTE]
    > Azure Active Directory 계정 보유자는 활성화되기 전에 계정을 확인하기 위한 링크를 포함한 이메일을 받습니다.
    > 
    > 

## <a name="assigning-users"></a>사용자 할당
구성을 테스트하려면 응용 프로그램 사용을 허용하려는 Azure AD 사용자를 할당하여 액세스 권한을 부여해야 합니다.

### <a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Work.com에 사용자를 할당하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털에서 테스트 계정을 만듭니다.

2. Work.com 응용 프로그램 통합 페이지에서 **사용자 할당**을 클릭합니다.
   
    ![사용자 할당](./media/active-directory-saas-work-com-tutorial/IC794119.png "Assign Users")

3. 테스트 사용자를 선택하고 **할당**을 클릭한 다음 **예**를 클릭하여 할당을 확인합니다.
   
    ![예](./media/active-directory-saas-work-com-tutorial/IC767830.png "Yes")

이제 10분 동안 기다린 후 계정이 Work.com에 동기화되었는지 확인해야 합니다.

Single Sign-On 설정을 테스트하려면 액세스 패널을 엽니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 참조하세요.




<!--HONumber=Dec16_HO1-->


