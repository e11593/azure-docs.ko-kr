---
title: "Azure 서비스 관리에서 Azure Resource Manager로의 마이그레이션을 위한 커뮤니티 도구"
description: "이 문서에는 Azure 서비스 관리에서 Azure Resource Manager 스택으로의 IaaS 리소스 마이그레이션을 지원하기 위해 커뮤니티에서 제공한 도구의 카탈로그가 포함되어 있습니다."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: singhkay
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 032a7068cce78036caec587bfc53abddf5a36659


---
# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Azure 서비스 관리에서 Azure Resource Manager로의 마이그레이션을 위한 커뮤니티 도구
이 문서에는 Azure 서비스 관리에서 Azure Resource Manager 스택으로의 IaaS 리소스 마이그레이션을 지원하기 위해 커뮤니티에서 제공한 도구의 카탈로그가 포함되어 있습니다.

> [!NOTE]
> 이러한 도구는 Microsoft 지원을 통해 공식적으로 지원되지는 않습니다. 따라서 Github에 오픈 소스로 제공되며 수정 또는 추가적인 시나리오를 위한 PR을 기꺼이 받아들입니다. 문제를 보고하려면 Github의 문제 기능을 사용합니다.
> 
> 이러한 도구를 사용하여 마이그레이션을 수행하면 클래식 Virtual Machine의 가동이 중지됩니다. 플랫폼에서 지원하는 마이그레이션 방법은 다음 페이지를 참조하세요. 
> 
> * [클래식에서 Azure Resource Manager 스택으로 IaaS 리소스의 플랫폼 지원 마이그레이션](virtual-machines-windows-migration-classic-resource-manager.md)
> * [클래식에서 Azure Resource Manager로의 플랫폼 지원 마이그레이션에 대한 기술 정보](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
> * [Azure PowerShell을 사용하여 클래식에서 Azure Resource Manager로 IaaS 리소스 마이그레이션](virtual-machines-windows-ps-migration-classic-resource-manager.md)
> 
> 

## <a name="asm2arm"></a>ASM2ARM
**단일** VM(Virtual Machine)을 Azure 서비스 관리 스택에서 Azure Resource Manager 스택으로 마이그레이션하는 데 사용할 수 있는 PowerShell 스크립트 모듈입니다. 

[도구 설명서 링크](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz
migAz는 전체 Azure 서비스 관리 IaaS 리소스 집합을 Azure Resource Manager IaaS 리소스로 마이그레이션할 수 있는 추가 옵션입니다. 동일한 구독 내에서 또는 서로 다른 구독 및 구독 형식(예: CSP 구독) 간에 마이그레이션을 수행할 수 있습니다.

[도구 설명서 링크](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)




<!--HONumber=Nov16_HO3-->


