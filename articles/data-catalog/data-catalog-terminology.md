---
title: "Azure 데이터 카탈로그 용어 | Microsoft Docs"
description: "이 문서는 Azure 데이터 카탈로그 설명서에 사용된 용어 및 개념에 대한 소개를 제공합니다."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6fec74d9-4a3c-4b4b-88ba-cad5ad143331
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/23/2017
ms.author: maroche
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: e9e1009bc20679a907e4bd2387865a6000b4a898


---
# <a name="azure-data-catalog-terminology"></a>Azure 데이터 카탈로그 용어
## <a name="catalog"></a>카탈로그
Azure 데이터 카탈로그는 데이터 원본이 있는 클라우드 기반 메타데이터 리포지토리이며 데이터 자산을 등록할 수 있습니다. 카탈로그는 데이터 원본에서 추출한 구조적 메타데이터 및 사용자가 추가한 설명이 포함된 메타데이터를 위한 중앙 저장소 위치로 제공됩니다.

## <a name="data-source"></a>데이터 원본
데이터 원본은 데이터 자산을 관리하는 시스템 또는 컨테이너입니다. SQL Server 데이터베이스, Oracle 데이터베이스, SQL Server Analysis Services 데이터베이스(테이블 형식 또는 다차원) 및 SQL Server Reporting Services 서버를 예로 들 수 있습니다.

## <a name="data-asset"></a>데이터 자산
데이터 자산에는 카탈로그와 함께 등록될 수 있는 데이터 원본 내에 포함되는 개체입니다. SQL Server 테이블 및 뷰, Oracle 테이블 및 뷰, SQL Server Analysis Services 측정값, 차원 및 KPI, SQL Server Reporting Services 보고서를 예로 들 수 있습니다.

## <a name="data-asset-location"></a>데이터 자산 위치
카탈로그는 클라이언트 응용 프로그램을 사용하여 원본에 연결하는 데 사용할 수 있는 데이터 원본 또는 데이터 자산 위치를 저장합니다. 위치의 형식 및 세부 정보는 데이터 원본 유형에 따라 달라집니다. 예를 들어, SQL Server Reporting Services 보고서가 URL로 식별되는 반면, SQL Server 테이블은 네 부분의 이름(서버 이름, 데이터베이스 이름, 스키마 이름, 개체 이름)으로 식별됩니다.

## <a name="structural-metadata"></a>구조적 메타데이터
구조적 메타데이터는 데이터 자산의 구조를 설명하는 데이터 원본에서 추출된 메타데이터입니다. 여기에는 자산 위치, 해당 개체 이름 및 유형, 추가 유형의 특징이 포함됩니다. 예를 들어, 테이블 및 뷰에 대한 구조적 메타데이터에는 이름 및 개체의 열에 대한 데이터 형식 포함됩니다.

## <a name="descriptive-metadata"></a>설명이 포함된 메타데이터
설명이 포함된 메타데이터는 목적 또는 데이터 자산의 의도를 설명하는 메타데이터입니다. 일반적으로 설명이 포함된 메타데이터는 Azure 데이터 카탈로그 포털을 사용하여 카탈로그 사용자가 추가하지만 등록 중에 데이터 원본에서 추출할 수도 있습니다. 예를 들어 Azure Data Catalog Registration Tool은 SQL Server Analysis Services 및 SQL Server Reporting Services의 Description 속성 및 SQL Server의 [ms_description extended property](https://technet.microsoft.com/library/ms190243.aspx)(이러한 속성이 값으로 채워진 경우)에서 설명을 추출합니다.

## <a name="request-access"></a>액세스 요청
데이터 자산의 설명이 포함된 메타데이터는 데이터 자산이나 데이터 원본에 대한 액세스를 요청하는 방법에 대한 정보를 포함할 수 있습니다. 이 정보는 데이터 자산 위치에 함께 표시되며 다음 중 하나 이상의 선택적 정보를 포함할 수 있습니다.

* 데이터 원본에 대한 액세스를 허용하는 역할을 하는 사용자나 팀의 전자 메일 주소.
* 데이터 원본에 대한 액세스 권한을 얻기 위해 사용자가 따라야 하는 절차가 설명된 문서의 URL.
* 데이터 원본에 대한 액세스 권한을 얻는 데 사용되는 ID 및 액세스 관리 도구(예: Microsoft Identity Manager)의 URL.
* 데이터 원본에 대한 액세스 권한을 얻는 방법을 설명하는 자유로운 형식의 텍스트 항목.

## <a name="preview"></a>미리 보기
Azure 데이터 카탈로그에 있는 미리 보기는 등록 시 데이터 원본에서 추출할 수 있고 데이터 자산 메타데이터와 함께 카탈로그에 저장할 수 있는 최대 20개 레코드의 스냅숏입니다. 미리 보기는 사용자가 해당 기능 및 용도를 더 잘 이해하기 위해 데이터 자산을 검색할 수 있도록 돕습니다. 즉, 샘플 데이터를 보는 것이 열 이름 및 데이터 유형을 보는 것보다 더 중요할 수 있습니다.
미리 보기는 테이블 및 뷰에 대해서만 지원하며, 등록 중에 사용자가 명시적으로 선택해야 합니다.

## <a name="data-profile"></a>데이터 프로필
Azure 데이터 카탈로그에 있는 데이터 프로필은 등록 시 데이터 원본에서 추출할 수 있고 데이터 자산 메타데이터와 함께 카탈로그에 저장할 수 있는 등록된 데이터 자산에 대한 테이블 수준 및 열 수준 메타데이터의 스냅숏입니다. 데이터 프로필은 사용자가 해당 기능 및 용도를 더 잘 이해하기 위해 데이터 자산을 검색할 수 있도록 돕습니다. 미리 보기와 마찬가지로, 데이터 프로필은 등록 시 사용자가 명시적으로 선택해야 합니다.

> [!NOTE]
> 대형 테이블 및 뷰에 대한 데이터 프로필 추출은 비용이 많이 드는 작업일 수 있으며 데이터 원본을 등록하는 데 필요한 시간이 상당히 늘어날 수 있습니다.
>
>

## <a name="user-perspective"></a>사용자 관점
Azure 데이터 카탈로그에서 모든 사용자가 등록된 데이터 자산에 대해 설명이 포함된 메타데이터를 제공할 수 있습니다. 각 사용자는 데이터와 용도에 대해 고유한 관점을 갖습니다. 예를 들어, 서버 담당자는 해당 SLA(서비스 수준 계약) 또는 백업 창에 대한 세부 정보를 제공할 수 있습니다. 데이터 관리자는 데이터 지원을 처리하는 비즈니스에 대한 설명서 링크를 제공할 수 있습니다. 분석가는 다른 분석가와 가장 관련된 용어 및 데이터를 검색하고 이해하는 데 필요한 사용자에게 가장 중요할 수 있는 용어에 대한 설명을 제공할 수 있습니다.

이러한 각각의 견해는 본질적으로 중요합니다. 모든 사용자가 데이터 및 해당 목적을 이해하기 위한 정보를 사용할 수 있으며, Azure 데이터 카탈로그를 사용하여 각각의 사용자는 해당 사용자에게 의미 있는 정보를 제공할 수 있습니다.

## <a name="expert"></a>전문가
전문가는 데이터 자산에 대해 잘 아는 “전문가” 관점이 있는 사용자입니다. 모든 사용자는 사용자 자신을 추가하거나 다른 사용자를 자산에 대한 전문가로 추가할 수 있습니다. 전문가로 지정되면 Azure 데이터 카탈로그에서 추가 권한을 전달하지 않습니다. 이를 통해 사용자는 자산의 설명이 포함된 메타데이터를 검토할 때 가장 유용하게 사용할 수 있는 관점을 쉽게 찾을 수 있습니다.

## <a name="owner"></a>소유자
소유자는 Azure 데이터 카탈로그에서 데이터 자산을 관리하기 위한 추가 권한이 있는 사용자입니다. 사용자는 등록된 자산 데이터의 소유권을 가져올 수 있으며 소유자는 다른 사용자를 공동 소유자로 추가할 수 있습니다. 자세한 내용은 [데이터 자산을 관리하는 방법](data-catalog-how-to-manage.md)을 참조하세요.  

> [!NOTE]
> 소유권 및 관리는 Azure 데이터 카탈로그의 표준 버전에서만 사용할 수 있습니다.
>
>

## <a name="registration"></a>등록
등록은 데이터 원본에서 데이터 자산 메타데이터를 추출하고 Azure 데이터 카탈로그 서비스에 이를 복사하는 작업입니다. 등록된 데이터 자산은 주석을 첨부하고 검색할 수 있습니다.

## <a name="see-also"></a>참고 항목
* [Azure 데이터 카탈로그란?](data-catalog-what-is-data-catalog.md)  - 이 문서에서는 Azure 데이터 카탈로그 서비스, 제공하는 값, 지원하는 시나리오에 대한 개요를 제공합니다.
* [Azure 데이터 카탈로그 시작](data-catalog-get-started.md) - 이 문서에서는 데이터 원본 검색을 위해 Azure 데이터 카탈로그를 사용하는 방법을 보여주는 포괄적인 자습서를 제공합니다.  



<!--HONumber=Nov16_HO3-->


