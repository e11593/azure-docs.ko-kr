---
title: "자습서: ThousandEyes와 Azure Active Directory 통합 | Microsoft Docs"
description: "Azure Active Directory에서 ThousandEyes를 사용하여 Single Sign-On, 자동화된 프로비전 등을 사용하도록 설정하는 방법을 알아봅니다."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/11/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 28cd9db7e49328298f2be5788b6d94baaafb6651


---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>자습서: ThousandEyes와 Azure Active Directory 통합
이 자습서는 Azure Active Directory(Azure AD)와 ThousandEyes 사이에 Single Sign-On을 설정하는 방법을 보여주기 위한 것입니다.

이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 항목이 있다고 가정합니다.

* 유효한 Azure 구독
* ThousandEyes Single Sign-On이 설정된 구독

이 자습서를 완료한 후 ThousandEyes에 할당한 Azure AD 사용자가 ThousandEyes 회사 사이트 (서비스 공급자가 시작한 로그온)에서나 액세스 패널 소개를 사용하여 응용 프로그램에 Single Sign-On할 수 있습니다.

1. ThousandEyes에 응용 프로그램 통합 사용
2. Single Sign-On 구성
3. 사용자 프로비전 구성
4. 사용자 할당

![시나리오](./media/active-directory-saas-thousandeyes-tutorial/IC790059.png "Scenario")

## <a name="enabling-the-application-integration-for-thousandeyes"></a>ThousandEyes에 응용 프로그램 통합 사용
이 섹션은 ThousandEyes에 응용 프로그램 통합을 사용하도록 설정하는 방법을 간략하게 설명하기 위한 것입니다.

### <a name="to-enable-the-application-integration-for-thousandeyes-perform-the-following-steps"></a>ThousandEyes에 응용 프로그램 통합을 사용하도록 설정하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털의 왼쪽 탐색 창에서 **Active Directory**를 클릭합니다.
   
   ![Active Directory](./media/active-directory-saas-thousandeyes-tutorial/IC700993.png "Active Directory")
2. **디렉터리** 목록에서 디렉터리 통합을 사용하도록 설정할 디렉터리를 선택합니다.
3. 응용 프로그램 보기를 열려면 디렉터리 보기의 최상위 메뉴에서 **응용 프로그램** 을 클릭합니다.
   
   ![응용 프로그램](./media/active-directory-saas-thousandeyes-tutorial/IC700994.png "Applications")
4. 페이지 맨 아래에 있는 **추가** 를 클릭합니다.
   
   ![응용 프로그램 추가](./media/active-directory-saas-thousandeyes-tutorial/IC749321.png "Add application")
5. **수행할 작업** 대화 상자에서 **갤러리에서 응용 프로그램 추가**를 클릭합니다.
   
   ![갤러리에서 응용 프로그램 추가](./media/active-directory-saas-thousandeyes-tutorial/IC749322.png "Add an application from gallerry")
6. **검색 상자**에 **ThousandEyes**를 입력합니다.
   
   ![응용 프로그램 갤러리](./media/active-directory-saas-thousandeyes-tutorial/IC790060.png "Application Gallery")
7. 결과 창에서 **ThousandEyes**를 선택하고 **완료**를 클릭하여 응용 프로그램을 추가합니다.
   
   ![ThousandEyes](./media/active-directory-saas-thousandeyes-tutorial/IC790061.png "ThousandEyes")

## <a name="configuring-single-sign-on"></a>Single Sign-On 구성
이 섹션에서는 사용자가 SAML 프로토콜 기반 페더레이션을 사용하여 Azure Active Directory의 계정으로 ThousandEyes에 인증할 수 있게 하는 방법을 간략하게 설명합니다.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Single Sign-On을 구성하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털의 **ThousandEyes** 응용 프로그램 통합 페이지에서 **Single Sign-On 구성**을 클릭하여 **Single Sign-On 구성** 대화 상자를 엽니다.
   
   ![Single SignOn 구성](./media/active-directory-saas-thousandeyes-tutorial/IC790062.png "Configure Single SignOn")
2. **ThousandEyes에 대한 사용자 로그온 방법을 선택하세요.** 페이지에서 **Microsoft Azure AD Single Sign-On**을 선택하고 **다음**을 클릭합니다.
   
   ![Single SignOn 구성](./media/active-directory-saas-thousandeyes-tutorial/IC790063.png "Configure Single SignOn")
3. **앱 URL 구성** 페이지의 **ThousandEyes 로그온 URL** 텍스트 상자에 ThousandEyes 응용 프로그램에 로그온하기 위해 사용자가 사용하는 URL(예: "*https://app.thousandeyes.com/login/sso*")을 입력한 후 **다음**을 클릭합니다. 
   
   ![앱 URL 구성](./media/active-directory-saas-thousandeyes-tutorial/IC790064.png "Configure App URL")
4. **ThousandEyes에서 Single Sign-On 구성** 페이지에서 인증서를 다운로드하려면 **인증서 다운로드**를 클릭한 다음 컴퓨터에 로컬로 인증서 파일을 저장합니다.
   
   ![Single SignOn 구성](./media/active-directory-saas-thousandeyes-tutorial/IC790065.png "Configure Single SignOn")
5. 다른 웹 브라우저 창에서 **ThousandEyes** 회사 사이트에 관리자로 로그인합니다.
6. 위쪽의 메뉴에서 **설정**을 클릭합니다.
   
   ![설정](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")
7. 페이지 맨 아래에 있는 **계정**
   
   ![계정](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")
8. **보안 및 인증** 탭을 클릭합니다.
   
   ![보안 및 인증](./media/active-directory-saas-thousandeyes-tutorial/IC790068.png "Security & Authentication")
9. **Single Sign-On 설정** 섹션에서 다음 단계를 수행합니다.
   
   ![Single Sign-On 설정](./media/active-directory-saas-thousandeyes-tutorial/IC790069.png "Setup Single Sign-On")
   
   1. **Single Sign-On 사용**을 선택합니다.
   2. Microsoft Azure 클래식 포털의 **ThousandEyes에서 Single Sign-On 구성** 대화 상자 페이지에서 **원격 로그인 URL** 값을 복사한 다음 **로그인 페이지 URL** 텍스트 상자에 붙여 넣습니다.
   3. Microsoft Azure 클래식 포털의 **ThousandEyes에서 Single Sign-On 구성** 대화 상자 페이지에서 **원격 로그아웃 URL** 값을 복사한 다음 **로그아웃 페이지 URL** 텍스트 상자에 붙여 넣습니다.
   4. Microsoft Azure 클래식 포털의 **ThousandEyes에서 Single Sign-On 구성** 대화 상자 페이지에서 **발급자 URL** 값을 복사한 다음 **ID 공급자 발급자** 텍스트 상자에 붙여 넣습니다.
   5. **ID 공급자 인증서**에서 **파일 선택**을 클릭하고 Microsoft Azure 클래식 포털에서 다운로드한 인증서를 업로드합니다.
   6. **Save**를 클릭합니다.
10. Azure 클래식 포털에서 Single Sign-On 구성 확인을 선택하고 **완료**를 클릭하여 **Single Sign-On 구성** 대화 상자를 닫습니다.
    
    ![Single SignOn 구성](./media/active-directory-saas-thousandeyes-tutorial/IC790070.png "Configure Single SignOn")

## <a name="configuring-user-provisioning"></a>사용자 프로비전 구성
Azure AD 사용자가 ThousandEyes에 로그인할 수 있도록 하려면 ThousandEyes로 프로비전되어야 합니다.  
ThousandEyes의 경우 프로비전은 수동 작업입니다.

### <a name="to-provision-a-user-account-to-thousandeyes-perform-the-following-steps"></a>사용자 계정을 ThousandEyes에 프로비전하려면 다음 단계를 수행합니다.
1. ThousandEyes 회사 사이트에 관리자 권한으로 로그인합니다.
2. **설정**을 클릭합니다.
   
   ![설정](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")
3. **계정**을 클릭합니다.
   
   ![계정](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")
4. **계정 및 사용자** 탭을 클릭합니다.
   
   ![계정 및 사용자](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts & Users")
5. **사용자 및 계정 추가** 섹션에서 다음 단계를 수행합니다.
   
   ![사용자 계정 추가](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Add User Accounts")
   
   1. 관련된 텍스트 상자에 프로비전할 유효한 Azure Active Directory 계정의 **이름**, **메일** 및 기타 세부 정보를 입력합니다.
   2. **계정에 새 사용자 추가**를 클릭합니다.
      
      > [!NOTE]
      > AAD 계정 소유자가 해당 계정을 확인 및 활성화하기 위한 링크가 포함된 이메일을 받게 됩니다.
      > 
      > 

> [!NOTE]
> 다른 ThousandEyes 사용자 계정 생성 도구 또는 ThousandEyes가 제공한 API를 사용하여 AAD 사용자 계정을 프로비전할 수 있습니다.
> 
> 

## <a name="assigning-users"></a>사용자 할당
구성을 테스트하려면 응용 프로그램 사용을 허용하려는 Azure AD 사용자를 할당하여 액세스 권한을 부여해야 합니다.

### <a name="to-assign-users-to-thousandeyes-perform-the-following-steps"></a>ThousandEyes에 사용자를 할당하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털에서 테스트 계정을 만듭니다.
2. **ThousandEyes** 응용 프로그램 통합 페이지에서 **사용자 할당**을 클릭합니다.
   
   ![사용자 할당](./media/active-directory-saas-thousandeyes-tutorial/IC790075.png "Assign Users")
3. 테스트 사용자를 선택하고 **할당**을 클릭한 다음 **예**를 클릭하여 할당을 확인합니다.
   
   ![예](./media/active-directory-saas-thousandeyes-tutorial/IC767830.png "Yes")

Single Sign-On 설정을 테스트하려면 액세스 패널을 엽니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 참조하세요.




<!--HONumber=Nov16_HO3-->


