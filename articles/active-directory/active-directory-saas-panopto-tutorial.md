---
title: "자습서: Panopto와 Azure Active Directory 통합 | Microsoft Docs"
description: "Azure Active Directory에서 Panopto를 사용하여 Single Sign-On, 자동화된 프로비전 등을 사용하도록 설정하는 방법을 알아봅니다."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 89c88e23-93ce-4970-9baa-1104c4e8fe4a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6084b2761e33609310461d90868179770d7acc01


---
# <a name="tutorial-azure-active-directory-integration-with-panopto"></a>자습서: Panopto와 Azure Active Directory 통합
이 자습서는 Azure 및 Panopto의 통합을 보여주기 위한 것입니다.  
이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 항목이 있다고 가정합니다.

* 유효한 Azure 구독
* Panopto 테넌트

이 자습서를 완료한 후 Panopto에 할당한 Azure AD 사용자가 Panopto 회사 사이트(서비스 공급자가 시작한 로그온)에서 또는 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 사용하여 응용 프로그램에 Single Sign-On 할 수 있습니다.

이 자습서에 설명된 시나리오는 다음 구성 요소로 이루어져 있습니다.

1. Panopto에 응용 프로그램 통합 사용
2. Single Sign-On 구성
3. 사용자 프로비전 구성
4. 사용자 할당

![시나리오](./media/active-directory-saas-panopto-tutorial/IC777665.png "Scenario")

## <a name="enabling-the-application-integration-for-panopto"></a>Panopto에 응용 프로그램 통합 사용
이 섹션은 Panopto에 응용 프로그램 통합을 사용하도록 설정하는 방법을 간략하게 설명하기 위한 것입니다.

### <a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Panopto에 응용 프로그램 통합을 사용하도록 설정하려면
1. Azure 클래식 포털의 왼쪽 탐색 창에서 **Active Directory**를 클릭합니다.
   
   ![Active Directory](./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")
2. **디렉터리** 목록에서 디렉터리 통합을 사용하도록 설정할 디렉터리를 선택합니다.
3. 응용 프로그램 보기를 열려면 디렉터리 보기의 최상위 메뉴에서 **응용 프로그램** 을 클릭합니다.
   
   ![응용 프로그램](./media/active-directory-saas-panopto-tutorial/IC700994.png "Applications")
4. 페이지 맨 아래에 있는 **추가** 를 클릭합니다.
   
   ![응용 프로그램 추가](./media/active-directory-saas-panopto-tutorial/IC749321.png "Add application")
5. **수행할 작업** 대화 상자에서 **갤러리에서 응용 프로그램 추가**를 클릭합니다.
   
   ![갤러리에서 응용 프로그램 추가](./media/active-directory-saas-panopto-tutorial/IC749322.png "Add an application from gallerry")
6. **검색 상자**에 **Panopto**를 입력합니다.
   
   ![Appkication 갤러리](./media/active-directory-saas-panopto-tutorial/IC777666.png "Appkication Gallery")
7. 결과 창에서 **Panopto**를 선택하고 **완료**를 클릭하여 응용 프로그램을 추가합니다.
   
   ![Panopto](./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
   
   ## <a name="configuring-single-sign-on"></a>Single Sign-On 구성

이 섹션은 사용자가 SAML 프로토콜 기반 페더레이션을 사용하여 Azure AD의 계정으로 Panopto에 인증할 수 있게 하는 방법을 간략하게 설명하기 위한 것입니다.  
이 절차의 일부로 base-64로 인코딩된 인증서 파일을 만들어야 합니다.  
이 절차를 잘 모르는 경우 [이진 인증서를 텍스트 파일로 변환하는 방법](http://youtu.be/PlgrzUZ-Y1o)을 참조하십시오.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Single Sign-On을 구성하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털의 **Panopto** 응용 프로그램 통합 페이지에서 **Single Sign-On 구성**을 클릭하여 **Single Sign-On 구성** 대화 상자를 엽니다.
   
   ![Single Sign-On 구성](./media/active-directory-saas-panopto-tutorial/IC777667.png "Configure single sign-on")
2. **Panopto에 대한 사용자 로그온 방법을 선택하세요.** 페이지에서 **Microsoft Azure AD Single Sign-On**을 선택하고 **다음**을 클릭합니다.
   
   ![Single Sign-On 구성](./media/active-directory-saas-panopto-tutorial/IC777668.png "Configure single sign-on")
3. **앱 URL 구성** 페이지의 **Panopto 로그인 URL** 텍스트 상자에 "*https://\<tenant-name\>. Panopto.com*" 패턴을 사용하여 URL을 입력하고 **다음**을 클릭합니다.
   
   ![앱 URL 구성](./media/active-directory-saas-panopto-tutorial/IC777528.png "Configure app URL")
4. **Panopto에서 Single Sign-On 구성** 페이지에서 인증서를 다운로드하려면 **인증서 다운로드**를 클릭한 다음 컴퓨터에 인증서 파일을 저장합니다.
   
   ![Single Sign-On 구성](./media/active-directory-saas-panopto-tutorial/IC777669.png "Configure single sign-on")
5. 다른 웹 브라우저 창에서 Panopto 회사 사이트에 관리자로 로그인합니다.
6. 왼쪽의 도구 모음에서 **시스템**을 클릭한 다음 **ID 공급자**를 클릭합니다.
   
   ![시스템](./media/active-directory-saas-panopto-tutorial/IC777670.png "System")
7. **공급자 추가**를 클릭합니다.
   
   ![ID 공급자](./media/active-directory-saas-panopto-tutorial/IC777671.png "Identity Providers")
8. SAML 공급자 섹션에서 다음 단계를 수행합니다.
   
   ![SaaS 구성](./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS configuration")
   
   1. **공급자 유형** 목록에서 **SAML20**을 선택합니다.
   2. **인스턴스 이름** 텍스트 상자에 인스턴스 이름을 입력합니다.
   3. **간단한 설명** 텍스트 상자에 간단한 설명을 입력합니다.
   4. Azure 클래식 포털의 **Panopto에서 Single Sign-On 구성** 대화 상자 페이지에서 **발급자 URL** 값을 복사하여 **발급자** 텍스트 상자에 붙여넣습니다.
   5. Azure 클래식 포털의 **Panopto에서 Single Sign-On 구성** 대화 상자 페이지에서 **SAML SSO URL** 값을 복사한 다음 **바운스 페이지 URL** 텍스트 상자에 붙여넣습니다.
   6. 다운로드한 인증서에서 **Base-64로 인코딩된** 파일을 만듭니다.  
      
      > [!TIP]
      > 자세한 내용은 [이진 인증서를 텍스트 파일로 변환하는 방법](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   7. Base 64로 인코딩된 인증서를 메모장에서 열고, 내용을 클립보드에 복사한 다음 전체 인증서를 **공용 키** 텍스트 상자에 붙여넣습니다.
   8. **저장**을 클릭합니다.
      ![저장](./media/active-directory-saas-panopto-tutorial/IC777673.png "Save")
9. Azure 클래식 포털에서 Single Sign-On 구성 확인을 선택하고 **완료**를 클릭하여 **Single Sign-On 구성** 대화 상자를 닫습니다.
   
   ![Single Sign-On 구성](./media/active-directory-saas-panopto-tutorial/IC777674.png "Configure single sign-on")
   
   ## <a name="configuring-user-provisioning"></a>사용자 프로비전 구성

Panopto를 프로비전하는 사용자를 구성할 작업 항목이 없습니다.  
할당된 사용자가 액세스 패널을 사용하여 Panopto에 로그인하려는 경우 Panopto는 사용자가 존재하는지를 확인합니다.  
사용할 수 있는 사용자 계정이 없으면 자동으로 Panopto에 의해 생성됩니다.

> [!NOTE]
> 다른 Panopto 사용자 계정 생성 도구 또는 Panopto가 제공한 API를 사용하여 Azure AD 사용자 계정을 프로비전할 수 있습니다.
> 
> 

## <a name="assigning-users"></a>사용자 할당
구성을 테스트하려면 응용 프로그램 사용을 허용하려는 Azure AD 사용자를 할당하여 액세스 권한을 부여해야 합니다.

### <a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Panopto에 사용자를 할당하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털에서 테스트 계정을 만듭니다.
2. **Panopto** 응용 프로그램 통합 페이지에서 **사용자 할당**을 클릭합니다.
   
   ![사용자 할당](./media/active-directory-saas-panopto-tutorial/IC777675.png "Assign users")
3. 테스트 사용자를 선택하고 **할당**을 클릭한 다음 **예**를 클릭하여 할당을 확인합니다.
   
   ![예](./media/active-directory-saas-panopto-tutorial/IC767830.png "Yes")

Single Sign-On 설정을 테스트하려면 액세스 패널을 엽니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 참조하세요.




<!--HONumber=Nov16_HO3-->


