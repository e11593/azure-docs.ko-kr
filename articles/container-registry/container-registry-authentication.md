---
title: "Azure 컨테이너 레지스트리로 인증 | Microsoft 문서"
description: "Azure Active Directory 서비스 주체 또는 관리자 계정을 사용하여 Azure 컨테이너 레지스트리에 로그인하는 방법에 대해 설명합니다."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
translationtype: Human Translation
ms.sourcegitcommit: 1a5af0b498cfdf1946f5c405d9557b0c2d2c8e63
ms.openlocfilehash: 1e9e54ee935b4c27eb93f72eb99c3ce52cc6c7e2

---
# <a name="authenticate-with-a-container-registry"></a>컨테이너 레지스트리로 인증
Azure 컨테이너 레지스트리에서 컨테이너 이미지를 사용하려면 `docker login` 명령을 사용하여 로그인합니다. **[Azure Active Directory 서비스 주체](../active-directory/active-directory-application-objects.md)** 또는 레지스트리 특정 **관리자 계정**을 사용하여 로그인할 수 있습니다. 이 문서에서는 이러한 ID에 대해 자세히 설명합니다. 


> [!NOTE]
> Container Registry는 현재 미리 보기 상태입니다. 미리 보기에서 개별 Azure Active Directory ID(사용자별 액세스 및 제어 활성화)는 컨테이너 레지스트리 인증을 지원하지 않습니다. 
> 





## <a name="service-principal"></a>서비스 주체

레지스트리에 [서비스 주체를 할당](container-registry-get-started-azure-cli.md#assign-a-service-principal)하고 기본 Docker 인증에 사용할 수 있습니다. 대부분의 시나리오에서는 서비스 주체를 사용하도록 권장됩니다. 다음 예제와 같이 서비스 주체의 앱 ID와 암호를 `docker login` 명령에 제공합니다.

```
docker login myregistry-contoso.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

로그인하면 Docker에서 자격 증명을 캐시하므로 앱 ID를 기억할 필요가 없습니다.

> [!TIP]
> 원하는 경우 `az ad sp reset-credentials` 명령을 실행하여 서비스 주체의 암호를 다시 생성할 수 있습니다.
> 


서비스 주체는 [역할 기반 액세스](../active-directory/role-based-access-control-configure.md)를 레지스트리에 허용합니다. 사용 가능한 역할은 읽기 권한자(끌어오기만), 참가자(끌어오기 및 밀어넣기) 및 소유자(끌어오기, 밀어넣기 및 다른 사용자에게 역할 할당)입니다. 여러 서비스 주체를 레지스트리에 할당할 수 있으므로 여러 사용자 또는 응용 프로그램에 대한 액세스를 정의할 수 있습니다. 또한 서비스 주체는 다음과 같은 개발자 또는 DevOps 시나리오에서 레지스트리에 대한 "헤드리스" 연결을 사용하도록 설정할 수 있습니다.

  * 레지스트리에서 DC/OS, Docker Swarm 및 Kubernetes를 포함한 오케스트레이션 시스템으로 컨테이너 배포 - 컨테이너 레지스트리를 관련 Azure 서비스(예: [Container Service](../container-service/index.md), [App Service](../app-service/index.md), [Batch](../batch/index.md) 및 [Service Fabric](../service-fabric/index.md))로 가져올 수도 있습니다.
  
  * 컨테이너 이미지를 작성하고 레지스트리로 푸시하는 지속적인 통합 및 배포 솔루션(예: Visual Studio Team Services 또는 Jenkins)
  
  



## <a name="admin-account"></a>관리자 계정
만드는 각 레지스트리에는 관리자 계정이 자동으로 만들어집니다. 기본적으로 계정은 사용할 수 없지만 [포털](container-registry-get-started-portal.md#manage-registry-settings) 또는 [Azure CLI 2.0 미리 보기 명령](container-registry-get-started-azure-cli.md#manage-admin-credentials)을 통해 계정을 사용하도록 설정하고 자격 증명을 관리할 수 있습니다. 계정을 사용할 수 있으면 레지스트리에 대한 기본 인증을 위해 사용자 이름과 암호를 `docker login` 명령에 전달할 수 있습니다. 예:

```
docker login myregistry-contoso.azurecr.io -u myAdminName -p myPassword
```

> [!IMPORTANT]
> 관리자 계정은 주로 테스트 용도로 단일 사용자가 레지스트리에 액세스하도록 설계되었습니다. 관리자 계정 자격 증명을 다른 사용자와 공유하지 않는 것이 좋습니다. 모든 사용자는 레지스트리에서 단일 사용자로 나타납니다. 이 계정을 변경하거나 사용하지 않도록 설정하면 자격 증명을 사용하는 모든 사용자의 레지스트리 액세스는 허용되지 않습니다. 
> 


### <a name="next-steps"></a>다음 단계
* [Docker CLI를 사용하여 첫 번째 이미지 푸시](container-registry-get-started-docker-cli.md)
* 컨테이너 레지스트리 미리 보기의 인증에 대한 자세한 내용은 [블로그 게시물](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/)을 참조하세요. 





<!--HONumber=Nov16_HO4-->


