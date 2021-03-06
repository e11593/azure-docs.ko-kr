---
title: "서버 탐색기로 저장소 리소스 찾아보기 및 관리 | Microsoft Docs"
description: "서버 탐색기로 저장소 리소스 찾아보기 및 관리"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 658dc064-4a4e-414b-ae5a-a977a34c930d
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: d35c9903fd68199f9decdf099a7e162fe664e4d5


---
# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>서버 탐색기로 저장소 리소스 찾아보기 및 관리
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>개요
Microsoft Visual Studio용 Azure 도구를 설치한 경우 Azure 저장소 계정에서 Blob, 큐, 테이블 데이터를 볼 수 있습니다. 서버 탐색기의 Azure 저장소 노드는 로컬 저장소 에뮬레이터 계정 및 다른 Azure 저장소 계정에 있는 데이터를 보여 줍니다.

Visual Studio에서 서버 탐색기를 보려면, 메뉴 모음에서 **보기**, **서버 탐색기**를 선택합니다. 저장소 노드는 연결된 Azure 구독/인증서 아래에 존재하는 모든 저장소 계정을 보여줍니다. 저장소 계정이 나타나지 않으면 [이 항목의 뒷부분에 나오는](#add-storage-accounts-by-using-server-explorer)다음의 지침을 따라 추가할 수 있습니다.

Azure SDK 2.7부터 새로운 클라우드 탐색기를 사용해 Azure 리소스를 확인 및 관리할 수 있습니다. 자세한 내용은 [클라우드 탐색기를 사용하여 Azure 리소스 관리](vs-azure-tools-resources-managing-with-cloud-explorer.md) 를 참조하세요.

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Visual Studio에서 저장소 리소스를 확인 및 관리합니다.
서버 탐색기는 저장소 에뮬레이터 계정에 있는 Blob, 큐, 테이블 목록을 자동으로 보여줍니다. 저장소 에뮬레이터 계정은 저장소 노드의 서버 탐색기에 **개발** 노드로 나열됩니다.

저장소 에뮬레이터 계정의 리소스를 보려면 **개발** 노드를 확장하세요. **개발** 노드를 확장했을 때 저장소 에뮬레이터가 시작하지 않으면, 자동으로 시작될 것입니다. 이 작업은 몇 초 정도 걸릴 수 있습니다. 저장소 에뮬레이터가 시작하는 동안 Visual Studio의 다른 영역에서 작업을 계속할 수 있습니다.

저장소 계정의 리소스를 보려면 서버 탐색기에서 저장소 계정의 노드를 확장하세요. 다음의 하위 노드가 표시됩니다.

* Blob
* 큐
* 테이블

## <a name="work-with-blob-resources"></a>Blob 리소스로 작업
Blob 노드는 선택된 저장소 계정의 컨테이너 목록을 표시합니다. Blob 컨테이너는 Blob 파일을 포함하고 있으며 이러한 Blob을 폴더와 하위 폴더로 구성할 수 있습니다. 자세한 내용은 [.NET에서 Blob 저장소를 사용하는 방법](storage/storage-dotnet-how-to-use-blobs.md) (영문)을 참조하세요.

### <a name="to-create-a-blob-container"></a>Blob 컨테이너를 생성하려면
1. **Blob** 노드에 대한 바로 가기 메뉴를 열고 **Blob 컨테이너 만들기**를 선택합니다.
2. **Blob 컨테이너 만들기** 대화 상자에서 새 컨테이너 이름을 입력하고 **확인**을 선택합니다.
   
   > [!NOTE]
   > Blob 컨테이너 이름은 숫자 (0-9) 또는 소문자 (a-z)로 시작해야 합니다.
   > 
   > 

### <a name="to-delete-a-blob-container"></a>Blob 컨테이너를 삭제하려면
* 제거하려는 Blob 컨테이너에 대한 바로 가기 메뉴를 열고 **삭제**를 선택합니다.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Blob 컨테이너에 포함된 항목의 목록을 표시하려면
* 목록에서 Blob 컨테이너 이름의 바로 가기 메뉴를 연 다음 **Blob 컨테이너 보기**를 선택합니다.
  
    Blob 컨테이너의 내용은 Blob 컨테이너 보기라는 탭에 표시됩니다.
  
    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)
  
    Blob 컨테이너 보기의 오른쪽에 있는 단추를 사용하여 Blob에서 다음의 작업을 수행할 수 있습니다.
  
  * 필터 값을 입력 및 적용하기
  * 컨테이너의 Blob 목록 새로 고침
  * 파일 업로드
  * Blob 삭제
    
    > [!NOTE]
    > Blob 컨테이너에서 파일을 삭제해도 기본 파일은 삭제되지 않으며, 해당 Blob 컨테이너에서만 제거됩니다.
    > 
    > 
  * Blob 열기
  * 로컬 컴퓨터에 Blob 저장

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Blob 컨테이너에 폴더 또는 하위 폴더를 만들려면
1. 서버 탐색기에서 Blob 컨테이너를 선택합니다. 컨테이너 창에서 **Blob 업로드** 단추를 선택합니다.
   
    ![Blob 폴더에 파일 업로드하기](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)
2. **새 파일 업로드** 대화 상자에서 **찾아보기** 단추를 선택하여 업로드하려는 파일을 지정한 후 **폴더(선택 사항)** 상자에 폴더 이름을 입력합니다.
   
    동일한 절차를 수행하여 컨테이너 폴더에 하위 폴더를 추가할 수 있습니다. 폴더 이름을 지정하지 않으면 파일은 Blob 컨테이너의 최상위 수준으로 업로드됩니다. 파일은 컨테이너에 지정된 폴더에 나타납니다.
   
    ![Blob 컨테이너에 폴더 추가하기](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)
3. 폴더를 두 번 클릭하거나 Enter 키를 눌러 폴더의 내용을 확인합니다. 컨테이너 폴더에 있는 경우 **부모 디렉터리 열기** (위쪽 화살표) 단추를 선택하여 컨테이너의 루트로 다시 이동할 수 있습니다.

### <a name="to-delete-a-container-folder"></a>컨테이너 폴더를 삭제하려면
* 폴더의 파일을 모두 삭제합니다
  
  > [!NOTE]
  > Blob 컨테이너의 폴더는 가상 폴더이기 때문에 빈 폴더를 만들거나 파일 내용을 삭제하기 위해 폴더를 삭제할 수 없습니다. 폴더를 삭제하려면 폴더의 전체 내용을 삭제해야 합니다.
  > 
  > 

### <a name="to-filter-blobs-in-a-container"></a>컨테이너의 Blob를 필터링하려면
공통 접두사를 지정하여 표시되는 Blob를 필터링할 수 있습니다.

예를 들어 필터 텍스트 상자에 접두사 `hello`를 입력한 후 **실행**(**!**) 단추를 선택하면 ‘hello’로 시작하는 Blob만 나타납니다.

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

> [!NOTE]
> 필터 필드는 대/소문자를 구분하며 와일드카드 문자 필터링을 지원하지 않습니다. Blob은 접두사로만 필터링될 수 있습니다. 구분 기호를 사용하여 가상 계층에 Blob을 구성하는 경우 접두사는 구분 기호를 포함할 수 있습니다. 예를 들어, HelloFabric/ 접두사로 시작하는 필터링은 해당 문자열로 시작하는 모든 Blob을 반환합니다.
> 
> 

### <a name="to-download-blob-data"></a>Blob 데이터를 다운로드하려면
* **서버 탐색기**에서 하나 이상의 Blob에 대한 바로 가기 메뉴를 열고 **열기**를 선택하거나, Blob 이름을 선택한 후 **열기** 단추를 선택하거나, Blob 이름을 두 번 클릭합니다.
  
    Blob 다운로드 진행률이 **Azure 활동 로그** 창에 표시됩니다.
  
    Blob는 해당 파일 형식에 대한 기본 편집기에서 열립니다. 운영 체제가 파일 형식을 인식하는 경우 파일은 로컬에 설치된 응용 프로그램에서 열립니다. 그렇지 않은 경우 Blob의 파일 형식에 대한 적절한 응용 프로그램을 선택하라는 메시지가 표시됩니다. Blob을 다운로드할 때 생성되는 로컬 파일은 읽기 전용으로 표시됩니다.
  
    Blob 데이터는 로컬로 캐시되고 Blob 서비스에서 Blob의 마지막 수정된 시간에 대해 확인됩니다. Blob이 마지막으로 다운로드된 후에 업데이트 된 경우 다시 다운로드됩니다. 그렇지 않은 경우 Blob은 로컬 디스크에서 로드됩니다. 기본적으로 Blob은 임시 디렉터리에 다운로드됩니다. 특정 디렉터리에 Blob을 다운로드하려면 선택한 Blob 이름에 대한 바로 가기 메뉴를 열고 **다른 이름으로 저장**을 선택합니다. 이 방식으로 Blob을 저장하면 Blob 파일이 열리지 않으며, 로컬 파일이 읽기/쓰기 특성으로 생성됩니다.

### <a name="to-upload-blobs"></a>Blob을 업로드하려면
* Blob 컨테이너 보기에서 컨테이너가 열려 있을 때 **Blob 업로드** 단추를 선택합니다.
  
    하나 이상의 파일을 선택해 업로드할 수 있으며 모든 형식의 파일을 업로드할 수 있습니다. **Azure 활동 로그** 에 업로드 진행률이 표시됩니다. Blob 데이터 작업 방법에 대한 자세한 내용은 [.NET에서 Azure Blob 저장소 서비스를 사용하는 방법](http://go.microsoft.com/fwlink/p/?LinkId=267911)을 참조하세요.

### <a name="to-view-logs-transferred-to-blobs"></a>Blob에 전송된 로그를 보려면
* Azure 진단을 사용하여 Azure 응용 프로그램에서 데이터를 기록하고 저장소 계정에 로그를 전송하는 경우 이러한 로그에 대해 Azure에서 만들어진 컨테이너가 표시됩니다. 서버 탐색기에서 이러한 로그를 확인하는 것은 특히 Azure에 배포된 경우 응용 프로그램에 대한 문제를 식별하는 쉬운 방법입니다. Azure 진단에 대한 자세한 내용은 [Azure 진단을 사용하여 로깅 데이터 수집](https://msdn.microsoft.com/library/azure/gg433048.aspx)을 참조하세요.

### <a name="to-get-the-url-for-a-blob"></a>Blob에 대한 URL을 가져오려면
* Blob의 바로 가기 메뉴를 열고 **URL 복사**를 선택합니다.

### <a name="to-edit-a-blob"></a>Blob을 편집하려면
* Blob을 선택한 다음 **Blob 열기** 단추를 선택합니다.
  
    파일은 임시 위치로 다운로드되고 로컬 컴퓨터에서 열립니다. 변경한 후 다시 Blob을 업로드해야 합니다.

## <a name="work-with-queue-resources"></a>큐 리소스로 작업
저장소 서비스 큐는 Azure 저장소 계정에서 호스팅되며 클라우드 서비스 역할이 메시지 전달 메커니즘에 의해 서로 및 다른 서비스와 통신할 수 있도록 사용할 수 있습니다. 클라우드 서비스 및 외부 클라이언트에 대한 웹 서비스를 통해 프로그래밍 방식으로 큐에 액세스할 수 있습니다. 또한 Visual Studio에서 서버 탐색기를 사용하여 직접 큐에 액세스할 수 있습니다.

큐를 사용하는 클라우드 서비스를 개발할 때 Visual Studio를 사용하여 큐를 생성하여 코드를 개발 및 테스트하는 동안 대화형으로 작업하는 것이 좋습니다.

서버 탐색기에서는 저장소 계정의 큐 보기, 큐 생성 및 삭제, 큐를 열어 메시지 보기, 큐에 메시지 추가하기가 가능합니다. 큐를 확인하기 위해 열 때 개별 메시지를 볼 수 있으며 왼쪽 위의 단추를 사용하여 큐에서 다음의 작업을 수행할 수 있습니다.

* 큐 보기 새로 고침
* 큐에 메시지 추가
* 최상위 메시지를 큐에서 제거
* 전체 큐 지우기

다음 그림에서는 두 개의 메시지를 포함하는 큐를 보여줍니다.

![큐 보기](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

저장소 서비스 큐에 대한 자세한 내용은 [큐 저장소 서비스를 사용하는 방법](http://go.microsoft.com/fwlink/?LinkID=264702)을 참조하세요. 저장소 서비스 큐의 웹 서비스에 대한 자세한 내용은 [큐 서비스 개념](http://go.microsoft.com/fwlink/?LinkId=264788)을 참조하세요. Visual Studio를 사용하여 저장소 서비스 큐에 메시지를 보내는 방법에 대한 자세한 내용은 [저장소 서비스 큐에 메시지 보내기](https://msdn.microsoft.com/library/azure/jj649344.aspx)를 참조하세요.

> [!NOTE]
> 저장소 서비스 큐는 서비스 버스 큐와 구별됩니다. 서비스 버스 큐에 대한 자세한 내용은 서비스 버스 큐, 항목 및 구독을 참조하세요.
> 
> 

## <a name="work-with-table-resources"></a>테이블 리소스로 작업
Azure 테이블 저장소 서비스는 다량의 구조화된 데이터를 저장합니다. 이 서비스는 Azure 클라우드 내부 및 외부에서 인증된 호출을 수락하는 NoSQL 데이터 저장소입니다. Azure 테이블은 구조화된 비관계형 데이터를 저장하는 데 적합합니다.

### <a name="to-create-a-table"></a>테이블 만들기
1. 서버 탐색기에서 저장소 계정의 **테이블** 노드를 선택한 다음 **테이블 만들기**를 선택합니다.
2. **테이블 만들기** 대화 상자에서 테이블의 이름을 입력합니다.

### <a name="to-view-table-data"></a>테이블 데이터를 보려면
1. 서버 탐색기에서 **Azure** 노드를 연 다음 **저장소** 노드를 엽니다.
2. 관심 있는 저장소 계정 노드를 연 다음 **테이블** 노드를 열어 저장소 계정에 대한 테이블 목록을 확인합니다.
3. 테이블의 바로 가기 메뉴를 열고 **테이블 보기**를 선택합니다.
   
    ![솔루션 탐색기에 있는 Azure 테이블](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

테이블은 엔터티 (행에 표시) 및 속성 (열에 표시) 별로 구성됩니다. 예를 들어 다음 그림은 **테이블 디자이너**에 나열된 엔터티를 보여 줍니다.

### <a name="to-edit-table-data"></a>테이블 데이터를 편집하려면
1. **테이블 디자이너**에서 엔터티(단일 행) 또는 속성(단일 셀)에 대한 바로 가기 메뉴를 열고 **편집**을 선택합니다.
   
    ![테이블 엔터티 추가 또는 편집](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)
   
    단일 테이블의 엔터티는 속성 (열)과 동일한 집합을 가질 필요가 없습니다. 테이블 데이터 보기 및 편집에 대한 다음의 제한 사항에 유의하십시오.
   
   * 이진 데이터(byte[] 형식)를 보거나 편집할 수 없지만 테이블에 저장할 수는 있습니다.
   * Azure의 테이블 저장소는 **PartitionKey** 또는 **RowKey** 값의 편집을 지원하지 않으므로 해당 작업을 할 수 없습니다.
   * Timestamp는 Azure 저장소 서비스가 사용하는 이름이므로 해당 이름의 속성을 만들 수 없습니다.
   * DateTime 값을 입력하는 경우 컴퓨터의 지역 및 언어 설정에 부합하는 형식을 따라야 합니다 (예: 미국 영어에 경우MM/DD/YYYY HH:MM:SS  [AM|PM]).

### <a name="to-add-entities"></a>엔터티를 추가하려면
1. **테이블 디자이너**에서 테이블 보기의 오른쪽 위에 있는 **엔터티 추가** 단추를 선택합니다.
   
    ![엔터티 추가](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)
2. **엔터티 추가** 대화 상자에서 **PartitionKey** 및 **RowKey** 속성 값을 입력합니다.
   
    ![엔터티 대화 상자 추가](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)
   
    엔터티를 삭제하고 다시 추가하지 않는 한 대화 상자를 닫은 후에는 값을 변경할 수 없으므로 신중하게 값을 입력하세요.

### <a name="to-filter-entities"></a>엔터티를 필터링하려면
쿼리 작성기를 사용하는 경우 테이블에 표시되는 엔터티 집합을 사용자 지정할 수 있습니다.

1. 쿼리 작성기를 열려면 테이블을 열어 확인합니다.
2. 테이블 보기의 도구 모음에서 맨 오른쪽 단추를 선택합니다.
   
    **쿼리 작성기** 대화 상자가 나타납니다. 다음 그림에서는 쿼리 작성기에서 작성되고 있는 쿼리를 보여줍니다.
   
    ![쿼리 작성기](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
3. 쿼리 작성을 마친 경우 대화 상자를 닫습니다. 쿼리의 결과 텍스트 형식이 WCF 데이터 서비스 필터로 텍스트 상자에 나타납니다.
4. 쿼리를 실행하려면 녹색 삼각형 아이콘을 선택합니다.
   
    또한 필터 필드에 WCF 데이터 서비스 필터 문자열을 직접 입력한 경우 **테이블 디자이너** 에 나타나는 엔터티 데이터를 필터링할 수 있습니다. 이러한 종류의 문자열은 SQL WHERE 절과 유사하지만 HTTP 요청으로 서버에 전송됩니다. 필터 문자열을 생성하는 방법에 대한 자세한 내용은 [테이블 디자이너에 대한 필터 문자열 생성](https://msdn.microsoft.com/library/azure/ff683669.aspx)을 참조하세요.
   
    다음 그림에서는 유효한 필터 문자열의 예를 보여줍니다.
   
    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>저장소 데이터 새로 고침
서버 탐색기에 연결하거나 저장소 계정에서 데이터를 가져올 때 작업을 완료하기까지 최대 일 분이 소요될 수 있습니다. 연결할 수 없는 경우 작업 시간이 초과될 수 있습니다. 데이터를 검색하는 동안에 Visual Studio의 다른 부분에서 작업을 계속할 수 있습니다. 작업이 너무 오래 걸려서 취소하려면, 서버 탐색기 도구 모음에서 **새로 고침 중지** 단추를 선택합니다.

#### <a name="to-refresh-blob-container-data"></a>Blob 컨테이너 데이터를 새로 고치려면
* **저장소** 아래의 **Blob** 노드를 선택하고 서버 탐색기 도구 모음에서 **새로 고침** 단추를 선택합니다.
* 표시된 Blob 목록을 새로 고치려면 **실행** 단추를 선택합니다.

#### <a name="to-refresh-table-data"></a>테이블 데이터를 새로 고치려면
* **저장소** 아래의 **테이블** 노드를 선택하고 **새로 고침** 단추를 선택합니다.
* **테이블 디자이너**에 표시된 엔터티 목록을 새로 고치려면 **테이블 디자이너**에서 **실행** 단추를 선택합니다.

#### <a name="to-refresh-queue-data"></a>큐 데이터를 새로 고치려면
* **큐** 노드를 선택한 다음 **새로 고침** 단추를 선택합니다.

#### <a name="to-refresh-all-items-in-a-storage-account"></a>저장소 계정의 모든 항목을 새로 고치려면
* 계정 이름을 선택한 다음 서버 탐색기에 대한 도구 모음에서 **새로 고침** 단추를 선택합니다.

### <a name="add-storage-accounts-by-using-server-explorer"></a>서버 탐색기를 사용하여 저장소 계정 추가
서버 탐색기를 사용하여 저장소 계정을 추가하는 방법은 두 가지가 있습니다. Azure 구독에서 새 저장소 계정을 만들거나 기존 저장소 계정을 연결할 수 있습니다.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>서버 탐색기를 사용하여 새 저장소 계정을 만들려면
1. 서버 탐색기에서 저장소 노드에 대한 바로 가기 메뉴를 연 다음 저장소 계정 만들기를 선택합니다.
   
    ![새 Azure 저장소 계정 만들기](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)
2. **저장소 계정 만들기** 대화 상자에서 새 저장소 계정에 대한 다음의 정보를 선택 또는 입력합니다.
   
   * 저장소 계정을 추가할 Azure 구독입니다.
   * 새 저장소 계정에 대해 사용하려는 이름입니다.
   * 지역 또는 선호도 그룹 (예: 미국 서부 또는 동아시아)입니다.
   * 저장소 계정에 대해 사용하려는 복제의 유형입니다 – 예: 지역 중복.
3. **만들기**를 선택합니다.
   
    솔루션 탐색기의 **저장소** 목록에 새 저장소 계정이 나타납니다.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>서버 탐색기를 사용하여 기존 저장소 계정을 연결하려면
1. 서버 탐색기에서 Azure 저장소 노드에 대 한 바로 가기 메뉴를 연 다음 **외부 저장소 연결**을 선택합니다.
   
    ![기존 저장소 계정 추가](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
2. **저장소 계정 만들기** 대화 상자에서 새 저장소 계정에 대한 다음의 정보를 선택 또는 입력합니다.
   
   * 연결하려는 기존 저장소 계정의 이름입니다. 이름을 입력하거나 목록에서 선택할 수 있습니다.
   * 선택한 저장소 계정에 대한 키입니다. 저장소 계정을 선택할 때 일반적으로 이 값이 제공됩니다. Visual Studio가 저장소 계정 키를 기억하기를 원하는 경우 계정 키 상자 기억하기를 선택합니다.
   * 저장소 계정에 연결하기 위해 사용하는 프로토콜입니다 - 예: HTTP, HTTPS, 또는 사용자 지정 끝점. 사용자 지정 끝점에 대한 자세한 내용은 [연결 문자열을 구성하는 방법](https://msdn.microsoft.com/library/azure/ee758697.aspx) 을 참조하세요.

### <a name="to-view-the-secondary-endpoints"></a>보조 끝점을 확인하려면
* **읽기 액세스 지역 중복** 복제 옵션을 사용하여 저장소 계정을 만든 경우, 보조 끝점을 확인할 수 있습니다. 계정 이름에 대한 바로 가기 메뉴를 연 다음 **속성**을 선택합니다.
  
    ![저장소 보조 끝점](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>서버 탐색기에서 저장소 계정을 제거하려면
* 서버 탐색기에서 계정 이름에 대한 바로 가기 메뉴를 연 다음 **삭제**를 선택합니다. 저장소 계정을 삭제하면 해당 계정에 저장된 키 정보도 제거됩니다.
  
  > [!NOTE]
  > 서버 탐색기에서 저장소 계정을 삭제하면 저장소 계정이나 그 안에 포함된 데이터에는 영향을 주지 않으며, 단순히 서버 탐색기의 참조를 제거합니다. 저장소 계정을 영구적으로 삭제하려면 [Azure 클래식 포털](http://go.microsoft.com/fwlink/?LinkID=213885)을 사용하세요.
  > 
  > 

## <a name="next-steps"></a>다음 단계
Azure 저장소 서비스를 사용하는 방법에 대해 자세히 알아보려면 [Azure 저장소 서비스 액세스](https://msdn.microsoft.com/library/azure/ee405490.aspx)를 참조하세요.




<!--HONumber=Nov16_HO3-->


