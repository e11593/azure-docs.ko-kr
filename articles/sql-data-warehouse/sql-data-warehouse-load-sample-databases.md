---
title: "샘플 데이터를 SQL Data Warehouse에 로드 | Microsoft Docs"
description: "SQL 데이터 웨어하우스로 샘플 데이터를 로드"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9edad2037787a7a2da4e2a6fcce15ace51d41032


---
# <a name="load-sample-data-into-sql-data-warehouse"></a>SQL 데이터 웨어하우스로 샘플 데이터를 로드
다음의 간단한 단계에 따라 Adventure Works 샘플 데이터베이스를 로드하고 쿼리합니다. 이러한 스크립트는 먼저 sqlcmd를 사용하여 테이블 및 뷰를 만드는 SQL을 실행합니다. 테이블을 만든 후 스크립트는 bcp를 사용하여 데이터를 로드합니다.  sqlcmd 및 bcp가 아직 설치되어 있지 않다면 다음 링크를 따라 [bcp를 설치][bcp를 설치]하고 [sqlcmd를 설치][sqlcmd를 설치]합니다.

## <a name="load-sample-data"></a>샘플 데이터 로드
1. [SQL 데이터 웨어하우스용 Adventure Works 샘플 스크립트][SQL 데이터 웨어하우스용 Adventure Works 샘플 스크립트] zip 파일을 다운로드합니다.
2. 사용자의 로컬 컴퓨터 디렉터리에 다운로드한 zip 파일의 압축을 풉니다.
3. 압축을 푼 aw_create.bat 파일을 편집하고 파일 위쪽에서 다음 변수를 설정합니다.  "="와 매개 변수 사이에 공백이 없도록 해야 합니다.  다음은 사용자가 편집한 예입니다.
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. 편집한 aw_create.bat 파일을 Windows cmd 프롬프트에서 실행합니다.  aw_create.bat 파일의 편집 버전을 저장한 디렉터리에 있어야 합니다.
   이 스크립트는 다음과 같은 기능을 수행합니다.
   
   * 데이터베이스에 존재하는 Adventure Works 테이블 또는 뷰를 제거합니다.
   * Adventure Works 테이블 및 뷰를 만듭니다.
   * bcp를 사용하여 각각의 Adventure Works 테이블을 로드합니다.
   * 각 Adventure Works 테이블의 행 개수를 확인합니다.
   * 각 Adventure Works 테이블의 모든 열에 대해 통계를 수집합니다.

## <a name="query-sample-data"></a>쿼리 샘플 데이터
SQL 데이터 웨어하우스로 샘플 데이터를 로드하고 나면, 몇 가지 쿼리를 신속하게 실행할 수 있습니다.  쿼리를 실행하려면 [Visual Studio를 사용하여 쿼리][Visual Studio를 사용하여 쿼리] 문서의 설명대로, Visual Studio 및 SSDT를 사용하여 Azure SQL DW에 새로 만든 Adventure Works 데이터베이스에 연결합니다.

직원의 모든 정보를 가져오는 간단한 select 문의 예입니다.

```sql
SELECT * FROM DimEmployee;
```

각 날짜의 모든 판매에 대한 총 금액을 살펴보기 위해 GROUP BY 같은 구문을 사용하는 더 복잡한 쿼리의 예입니다.

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

특정 날짜 이전의 주문을 필터링하기 위한 SELECT와 WHERE 절의 예입니다.

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL 데이터 웨어하우스는 SQL Server가 지원하는 거의 모든 T-SQL 구문을 지원합니다.  차이점은 [코드 마이그레이션][코드 마이그레이션] 문서에 설명되어 있습니다.

## <a name="next-steps"></a>다음 단계
샘플 데이터로 몇 가지 쿼리를 시도해 볼 기회를 가졌으니, SQL Data Warehouse로 [개발][개발], [로드][로드] 또는 [마이그레이션][마이그레이션]하는 방법에 대해 알아보겠습니다.

<!--Image references-->

<!--Article references-->
[마이그레이션]: sql-data-warehouse-overview-migrate.md
[개발]: sql-data-warehouse-overview-develop.md
[로드]: sql-data-warehouse-overview-load.md
[Visual Studio를 사용하여 쿼리]: sql-data-warehouse-query-visual-studio.md
[코드 마이그레이션]: sql-data-warehouse-migrate-code.md
[bcp를 설치]: sql-data-warehouse-load-with-bcp.md
[sqlcmd를 설치]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[SQL 데이터 웨어하우스용 Adventure Works 샘플 스크립트]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip



<!--HONumber=Nov16_HO3-->


