---
title: "자습서: LogicMonitor와 Azure Active Directory 통합 | Microsoft Docs"
description: "Azure Active Directory에서 LogicMonitor를 사용하여 Single Sign-On, 자동화된 프로비전 등을 사용하도록 설정하는 방법을 알아봅니다."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6dbb19605f92ff0c0d58478a56e564bab71123e4


---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>자습서: LogicMonitor와 Azure Active Directory 통합
이 자습서는 Azure 및 LogicMonitor의 통합을 보여주기 위한 것입니다.  
이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 항목이 있다고 가정합니다.

* 유효한 Azure 구독
* LogicMonitor 테넌트

이 자습서에 설명된 시나리오는 다음 구성 요소로 이루어져 있습니다.

1. LogicMonitor에 응용 프로그램 통합 사용
2. Single Sign-On 구성
3. 사용자 프로비전 구성
4. 사용자 할당

![시나리오](./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Scenario")

## <a name="enabling-the-application-integration-for-logicmonitor"></a>LogicMonitor에 응용 프로그램 통합 사용
이 섹션은 LogicMonitor에 응용 프로그램 통합을 사용하도록 설정하는 방법을 간략하게 설명하기 위한 것입니다.

### <a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>LogicMonitor에 응용 프로그램 통합을 사용하도록 설정하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털의 왼쪽 탐색 창에서 **Active Directory**를 클릭합니다.
   
   ![Active Directory](./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")
2. **디렉터리** 목록에서 디렉터리 통합을 사용하도록 설정할 디렉터리를 선택합니다.
3. 응용 프로그램 보기를 열려면 디렉터리 보기의 최상위 메뉴에서 **응용 프로그램** 을 클릭합니다.
   
   ![응용 프로그램](./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Applications")
4. 페이지 맨 아래에 있는 **추가** 를 클릭합니다.
   
   ![응용 프로그램 추가](./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Add application")
5. **수행할 작업** 대화 상자에서 **갤러리에서 응용 프로그램 추가**를 클릭합니다.
   
   ![갤러리에서 응용 프로그램 추가](./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Add an application from gallerry")
6. **검색 상자**에 **logicmonitor**를 입력합니다.
   
   ![응용 프로그램 갤러리](./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Application Gallery")
7. 결과 창에서 **LogicMonitor**를 선택하고 **완료**를 클릭하여 응용 프로그램을 추가합니다.
   
   ![logicmonitor](./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
   
   ## <a name="configuring-single-sign-on"></a>Single Sign-On 구성

이 섹션은 사용자가 SAML 프로토콜 기반 페더레이션을 사용하여 Azure AD의 계정으로 LogicMonitor에 인증할 수 있게 하는 방법을 간략하게 설명하기 위한 것입니다.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Single Sign-On을 구성하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털의 **LogicMonitor** 응용 프로그램 통합 페이지에서 **Single Sign-On 구성**을 클릭하여 **Single Sign-On 구성** 대화 상자를 엽니다.
   
   ![Single Sign-on 구성](./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Configure Single Sign-On")
2. **LogicMonitor에 대한 사용자 로그온 방법을 선택하세요.** 페이지에서 **Microsoft Azure AD Single Sign-On**을 선택하고 **다음**을 클릭합니다.
   
   ![Single Sign-on 구성](./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Configure Single Sign-On")
3. **앱 URL 구성** 페이지의 **LogicMonitor 로그온 URL** 텍스트 상자에서 사용자가 사용한 URL\(예: "*http://company.logicmonitor.com*"\))을 입력하여 LogicMonitor에 로그인하고 **다음**을 클릭합니다.
   
   ![앱 URL 구성](./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Configure App URL")
4. **LogicMonitor에서 Single Sign-On 구성** 페이지에서 **메타데이터 다운로드**를 클릭한 다음 컴퓨터에 메타데이터 파일을 저장합니다.
   
   ![Single Sign-on 구성](./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Configure Single Sign-On")
5. **LogicMonitor** 회사 사이트에 관리자 권한으로 로그인합니다.
6. 위쪽의 메뉴에서 **설정**을 클릭합니다.
   
   ![설정](./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Settings")
7. 왼쪽의 탐색 모음에서 **Single Sign-On**
   
   ![SSO(Single sign-on)](./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Single Sign-On")
8. **SSO(Single Sign-On) 설정** 섹션에서 다음 단계를 수행합니다.
   
   ![Singl Sign-On 설정](./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Single Sign-On Settings")
   
   1. **Single Sign-On 사용**을 선택합니다.
   2. **기본 역할 할당**으로 **readonly**를 선택합니다.
   3. 다운로드한 메타데이터 파일을 메모장에서 열고 파일 내용을 **ID 공급자 메타데이터** 텍스트 상자에 붙여넣습니다.
   4. **변경 내용 저장**을 클릭합니다.
9. Azure 클래식 포털에서 Single Sign-On 구성 확인을 선택하고 **완료**를 클릭하여 **Single Sign-On 구성** 대화 상자를 닫습니다.
   
   ![Single Sign-on 구성](./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>사용자 프로비전 구성

AAD 사용자가 로그인 할 수 있도록 Azure Active Directory 사용자 이름을 사용하여 LogicMonitor 응용 프로그램에 프로비전되어야 합니다.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>사용자 프로비전을 구성하려면
1. LogicMonitor 회사 사이트에 관리자 권한으로 로그인합니다.
2. 위쪽 메뉴에서 **설정**을 클릭한 다음 **역할 및 사용자**를 클릭합니다.
   
   ![역할 및 사용자](./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Roles and Users")
3. **추가**를 클릭합니다.
4. **계정 추가** 섹션에서 다음 단계를 수행합니다.
   
   ![계정 추가](./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Add an account")
   
   1. 관련된 텍스트 상자에 프로비전할 유효한 Azure Active Directory 사용자의 **이름**, **전자 메일**, **암호**, **암호 확인** 값을 입력합니다.
   2. **역할**, **보기 권한** 및 **상태**를 선택합니다.
   3. **제출**을 클릭합니다.

> [!NOTE]
> LogicMonitor에서 제공하는 다른 LogicMonitor 사용자 계정 만들기 도구 또는 API를 사용하여 Azure Active Directory 사용자 계정을 프로비전합니다.
> 
> 

## <a name="assigning-users"></a>사용자 할당
구성을 테스트하려면 응용 프로그램 사용을 허용하려는 Azure AD 사용자를 할당하여 액세스 권한을 부여해야 합니다.

### <a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>LogicMonitor에 사용자를 할당하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털에서 테스트 계정을 만듭니다.
2. **LogicMonitor** 응용 프로그램 통합 페이지에서 **사용자 할당**을 클릭합니다.
   
   ![사용자 할당](./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Assign Users")
3. 테스트 사용자를 선택하고 **할당**을 클릭한 다음 **예**를 클릭하여 할당을 확인합니다.
   
   ![예](./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Yes")

Single Sign-On 설정을 테스트하려면 액세스 패널을 엽니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 참조하세요.




<!--HONumber=Nov16_HO3-->


