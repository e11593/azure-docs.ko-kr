---
title: "Azure SQL Data Warehouse의 계산 능력 관리(Azure Portal) | Microsoft Docs"
description: "계산 능력을 관리하는 Azure 포털 작업 DWU를 조정하여 계산 리소스 크기를 조정합니다. 또는 계산 리소스를 일지 중지한 다음 다시 시작하여 비용을 절감합니다."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: c1c23ab46d9b4e43154a62080cb8865b246489f9
ms.openlocfilehash: 2e11486203f04d671548b6132e61f804acb7f90a


---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Azure SQL 데이터 웨어하우스의 계산 능력 관리(Azure 포털)
> [!div class="op_single_selector"]
> * [개요](sql-data-warehouse-manage-compute-overview.md)
> * [포털](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST (영문)](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

워크로드의 변화하는 요구를 충족시키도록 계산 리소스와 메모리를 확장하여 크기 조정을 실시합니다. 사용량이 많지 않은 시간 동안 리소스를 다시 조정하거나 계산 전체를 일시 중지하여 비용을 절감합니다.

이 작업 컬렉션은 Azure 포털을 사용하여 다음을 수행합니다.

* 계산 조정
* 계산 일시 중지
* 계산 다시 시작

자세한 내용은 [계산 관리 개요][계산 관리 개요]를 참조하세요.

## <a name="scale-compute-power"></a>계산 능력 크기 조정
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

계산 리소스를 변경하려면

1. [Azure 포털][Azure 포털]을 열고 데이터베이스를 연 다음 **조정**을 클릭합니다.

    ![크기 조정을 클릭합니다.][1]
2. 확장 블레이드에서 슬라이더를 왼쪽 또는 오른쪽으로 이동해 DWU 설정을 변경합니다.

    ![슬라이더를 이동합니다][2]
3. **Save**를 클릭합니다. 확인 메시지가 표시됩니다. **예**를 클릭하여 확인하거나 **아니요**를 클릭하여 취소합니다.

    ![저장을 클릭합니다.][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>계산 일시 중지
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

데이터베이스를 일시 중지하려면

1. [Azure 포털][Azure 포털]을 열고 데이터베이스를 엽니다. 상태가 **온라인**입니다.

    ![온라인 상태][6]
2. 계산과 메모리 리소스를 일시 중지하기 위해 **일시 중지**를 클릭하면 확인 메시지가 나타납니다. **예**를 클릭하여 확인하거나 **아니요**를 클릭하여 취소합니다.

    ![일시 중지 확인][7]
3. SQL 데이터 웨어하우스가 시작되는 동안 데이터베이스 상태는 **일시 중지하는 중**이 됩니다.
4. 상태가 **일시 중지됨**일 경우 일시 중지 작업이 완료된 것이며 더 이상 DWU 비용이 청구되지 않습니다.

    ![일시 중지 상태][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>계산 다시 시작
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

데이터베이스를 다시 시작하려면

1. [Azure 포털][Azure 포털]을 열고 데이터베이스를 엽니다. 상태가 **일시 중지됨**입니다.

    ![데이터베이스 일시 중지][4]
2. 데이터베이스를 다시 시작하기 위해 **시작**을 클릭하면 확인 메시지가 나타납니다. **예**를 클릭하여 확인하거나 **아니요**를 클릭하여 취소합니다.

    ![다시 시작 확인][5]
3. SQL 데이터 웨어하우스가 시작되는 동안 데이터베이스 상태는 "일시 중지하는 중"이 됩니다.
4. 상태가 **온라인**이면 데이터가 준비된 것입니다.

    ![온라인 상태][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>다음 단계
자세한 내용은 [관리 개요][관리 개요]를 참조하세요.

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[관리 개요]: ./sql-data-warehouse-overview-manage.md
[계산 관리 개요]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure 포털]: http://portal.azure.com/



<!--HONumber=Nov16_HO3-->


