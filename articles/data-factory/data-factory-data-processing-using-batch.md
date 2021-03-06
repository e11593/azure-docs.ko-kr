---
title: "데이터 팩터리 및 배치를 사용하여 대규모 데이터 집합 처리 | Microsoft Docs"
description: "Azure 배치의 병렬 처리 기능을 사용하여 Azure Data Factory 파이프라인에서 대용량 데이터를 처리하는 방법을 설명합니다."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 1b2514e1e6f39bb3ce9d8a46f4af01835284cdcc
ms.openlocfilehash: ec9be7c94c953155808ce8543b8297a361552c6e


---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>데이터 팩터리 및 배치를 사용하여 대규모 데이터 집합 처리
이 문서에서는 예약된 자동 방식으로 대규모 데이터 집합을 이동 및 처리하는 샘플 솔루션의 아키텍처에 대해 설명합니다. 또한 Azure Data Factory 및 Azure 배치를 사용하여 솔루션을 구현하는 종합적인 연습 과정을 제공합니다.

이 문서는 전체 샘플 솔루션의 연습을 포함하기 때문에 일반적인 문서보다 깁니다. 배치 및 Data Factory를 처음 사용하는 경우 이러한 서비스 및 작동 방식에 대해 알아볼 수 있습니다. 서비스에 대한 정보가 있으며 솔루션을 디자인/설계하는 경우 문서의 [아키텍처 섹션](#architecture-of-sample-solution)을 집중적으로 살펴보고, 프로토타입 또는 솔루션을 개발하는 경우는 [연습](#implementation-of-sample-solution)의 단계별 지침을 진행해볼 수 있습니다. 이 콘텐츠 및 사용 방법에 대한 사용자의 의견을 환영합니다.

첫째, 클라우드에서 대용량 데이터 집합을 처리할 때 Data Factory 및 배치 서비스가 어떻게 도움을 줄 수 있는지 살펴보겠습니다.     

## <a name="why-azure-batch"></a>Azure 배치를 사용해야 하는 이유
Azure 배치를 통해 클라우드에서 효율적으로 대규모 병렬 및 HPC(고성능 컴퓨팅) 응용 프로그램을 실행할 수 있습니다. 가상 컴퓨터의 관리된 컬렉션에서 실행되는 계산 집약적인 작업을 예약하는 플랫폼 서비스이며 작업의 요구를 충족하도록 계산 리소스의 규모를 자동으로 조정할 수 있습니다.

배치 서비스를 통해 응용 프로그램을 병렬로 규모에 따라 실행하도록 Azure 계산 리소스를 정의합니다. 요청 시 또는 예약된 작업을 실행할 수 있으며 HPC 클러스터, 개별 가상 컴퓨터, 가상 네트워크, 복잡한 작업 및 작업 스케줄러 인프라를 수동으로 만들거나 구성하거나 관리할 필요가 없습니다.

Azure 배치에 대해 잘 모를 경우 이 문서에 설명된 솔루션의 아키텍처/구현을 이해하는 데 도움이 되므로 다음 문서를 참조하세요.   

* [Azure 배치의 기본 사항](../batch/batch-technical-overview.md)
* [배치 기능 개요](../batch/batch-api-basics.md)

(선택 사항) Azure 배치에 대해 자세히 알아보려면 [Azure 배치의 학습 경로](https://azure.microsoft.com/documentation/learning-paths/batch/)를 참조하세요.

## <a name="why-azure-data-factory"></a>Azure Data Factory를 사용해야 하는 이유
데이터 팩터리는 데이터의 이동과 변환을 조율하고 자동화하는 클라우드 기반의 데이터 통합 서비스입니다. Data Factory 서비스를 사용하여 온-프레미스 및 클라우드 데이터 저장소에서 중앙 집중식 데이터 저장소(예: Azure Blob 저장소)로 데이터를 이동하고 Azure HDInsight 및 Azure 기계 학습과 같은 서비스를 사용하여 데이터를 처리/변환하는 관리되는 데이터 파이프라인을 만들 수 있습니다. 또한 예약된 방식(시간별, 일별, 주별 등)으로 실행되도록 데이터 파이프라인을 예약하고 간편하게 관리하여 문제를 파악한 후 조치를 취할 수도 있습니다.

Azure Data Factory에 대해 잘 모를 경우 이 문서에 설명된 솔루션의 아키텍처/구현을 이해하는 데 도움이 되므로 다음 문서를 참조하세요.  

* [Azure Data Factory 소개](data-factory-introduction.md)
* [첫 번째 데이터 파이프라인 작성](data-factory-build-your-first-pipeline.md)   

(선택 사항) Azure Data Factory에 대해 자세히 알아보려면 [Azure Data Factory의 학습 경로](https://azure.microsoft.com/documentation/learning-paths/data-factory/)를 참조하세요.

## <a name="data-factory-and-batch-together"></a>Data Factory 및 배치
Data Factory에는 원본 데이터 저장소의 데이터를 대상 데이터 저장소로 복사/이동하기 위한 복사 활동 및 Azure에서 Hadoop(HDInsight) 클러스터를 사용하여 데이터를 처리하는 하이브 활동과 같은 기본 제공 활동이 포함되어 있습니다. 지원되는 변환 활동 목록에 대해서는 [데이터 변환 활동](data-factory-data-transformation-activities.md)을 참조하세요.

또한 자체 논리에 따라 데이터를 이동 또는 처리하는 사용자 지정 .NET 활동을 만든 후 이러한 활동을 Azure HDInsight 클러스터 또는 Azure VM 배치 풀에 대해 실행할 수 있습니다. Azure 배치를 사용할 경우 제공하는 수식에 따라 자동으로 크기가 조정되도록(워크로드에 따라 VM 추가 또는 제거) 풀을 구성할 수 있습니다.     

## <a name="architecture-of-sample-solution"></a>샘플 솔루션 아키텍처
이 문서에 설명된 아키텍처는 단일 솔루션에 대한 것이지만 금융 서비스별 위험 모델링, 이미지 처리 및 렌더링, 유전자 분석 등과 같은 다양한 시나리오와 관련되어 있습니다.

다이어그램은 1) 데이터 팩터리가 데이터 이동 및 처리를 오케스트레이션하는 방법 및 2) Azure 배치가 데이터를 병렬 방식으로 처리하는 방법을 보여 줍니다. 쉽게 참조할 수 있도록 다이어그램을 다운로드하고 인쇄합니다(11 x 17인치 또는 A3 크기). [Azure 배치 및 Data Factory를 사용하여 HPC 및 데이터 오케스트레이션](http://go.microsoft.com/fwlink/?LinkId=717686)

[![대규모 데이터 처리 다이어그램](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

다음 목록은 프로세스의 기본 단계를 제공합니다. 솔루션에는 종단 간 솔루션을 구축하는 코드와 설명이 포함되어 있습니다.

1. **계산 노드(VM)의 풀과 함께 Azure 배치를 구성합니다**. 노드 수와 각 노드의 크기를 지정할 수 있습니다.
2. **Azure Data Factory 인스턴스를 만듭니다**.
3. **Data Factory 파이프라인에서 사용자 지정 .NET 작업을 만듭니다**. 작업은 Azure 배치 풀에서 실행되는 사용자 코드입니다.
4. **Azure storage에 Blob으로 대량의 입력 데이터를 저장합니다**. 데이터는 논리 조각(일반적으로 시간으로)으로 나뉩니다.
5. **Data Factory는 보조 위치에 병렬로 처리되는 데이터를 복사합니다** .
6. **Data Factory는 배치에서 할당한 풀을 사용하여 사용자 지정 작업을 실행합니다**. 데이터 팩터리는 작업을 동시에 실행할 수 있습니다. 각 작업은 데이터 조각을 처리합니다. 결과는 Azure 저장소에 저장됩니다.
7. **Data Factory에서 응용 프로그램을 통해 배포하거나 다른 도구에서 추가 처리하기 위한 목적으로 최종 결과를 세 번째 위치로 이동합니다**.

## <a name="implementation-of-sample-solution"></a>샘플 솔루션의 구현
샘플 솔루션은 의도적으로 간단하며, Data Factory 및 배치를 함께 사용하여 데이터 집합을 처리하는 방법을 보여 주기 위한 것입니다. 시계열에 구성된 입력 파일에서 일치하는 검색 단어("Microsoft")의 수를 계산하는 솔루션입니다. 출력 파일에 개수를 출력합니다.

**시간**: Azure, Data Factory 및 배치의 기본 사항에 익숙하고 아래 나열된 필수 구성 요소를 완료했다면 이 솔루션이 완료되는 데 1~2시간이 소요됩니다.

### <a name="prerequisites"></a>필수 조건
#### <a name="azure-subscription"></a>Azure 구독
Azure 구독이 없는 경우 몇 분 만에 무료 평가판 계정을 만들 수 있습니다. [무료 평가판](https://azure.microsoft.com/pricing/free-trial/)을 참조하세요.

#### <a name="azure-storage-account"></a>Azure 저장소 계정
이 자습서에서는 데이터 저장을 위해 Azure 저장소 계정을 사용합니다. Azure 저장소 계정이 없는 경우 [저장소 계정 만들기](../storage/storage-create-storage-account.md#create-a-storage-account)를 참조하세요. 샘플 솔루션은 Blob 저장소를 사용합니다.

#### <a name="azure-batch-account"></a>Azure 배치 계정
[Azure 포털](http://manage.windowsazure.com/)을 사용하여 Azure 배치 계정을 만듭니다. [Azure 배치 계정 만들기 및 관리](../batch/batch-account-create-portal.md)를 참조하세요. Azure 배치 계정 이름 및 계정 키를 적어둡니다. [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet을 사용하여 Azure 배치 계정을 만들 수도 있습니다. 이 cmdlet 사용에 대한 자세한 지침은 [Azure 배치 PowerShell cmdlet 시작](../batch/batch-powershell-cmdlets-get-started.md)을 참조하세요.

샘플 솔루션은 Azure 배치를 사용하여(간접적으로 Azure Data Factory 파이프라인을 통해) 가상 컴퓨터의 관리되는 컬렉션인 계산 노드의 풀에서 병렬 방식으로 데이터를 처리합니다.

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>VM(가상 컴퓨터)의 Azure 배치 풀
적어도 2개의 계산 노드로 **Azure 배치 풀**을 만듭니다.

1. [Azure 포털](https://portal.azure.com)에서 왼쪽 메뉴의 **찾아보기**, **배치 계정**을 차례로 클릭합니다.
2. Azure 배치 계정을 선택하여 **배치 계정** 블레이드를 엽니다.
3. **풀** 타일을 클릭합니다.
4. **풀** 블레이드에서 도구 모음의 추가 단추를 클릭하여 풀을 추가합니다.
   1. 풀에 대한 ID(**풀 ID**)를 입력합니다. Data Factory 솔루션을 만들 때 필요하므로 **풀의 ID**를 메모해둡니다.
   2. 운영 체제 제품군 설정에 **Windows Server 2012 R2** 를 지정합니다.
   3. **노드 가격 책정 계층**을 선택합니다.
   4. **대상 전용** 설정 값으로 **2**를 입력합니다.
   5. **노드당 최대 작업** 설정 값으로 **2**를 입력합니다.
   6. **확인**을 클릭하여 풀을 만듭니다.

#### <a name="azure-storage-explorer"></a>Azure 저장소 탐색기
[Azure Storage 탐색기 6(도구)](https://azurestorageexplorer.codeplex.com/) 또는 [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer)(ClumsyLeaf 소프트웨어에서). 클라우드 호스티드 응용 프로그램의 로그를 포함한 Azure Storage 프로젝트의 데이터 검사 및 변경에 대해 이러한 도구를 사용합니다.

1. 개인 액세스로 **mycontainer**라는 컨테이너 만들기(익명 액세스 없음)
2. **CloudXplorer**를 사용 중인 경우 다음과 같은 구조의 폴더와 하위 폴더를 만듭니다.

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   **Inputfolder** 및 **outputfolder**는 **mycontainer**에서 최상위 폴더이며 **inputfolder**에는 날짜-시간 스탬프(YYYY-MM-DD-HH)와 함께 하위 폴더가 있습니다.

   **Azure Storage 탐색기**를 사용 중인 경우 다음 단계에서 다음과 같은 이름이 지정된 파일을 업로드 해야 합니다: inputfolder/2015-11-16-00/file.txt, inputfolder/2015-11-16-01/file.txt 등 이 단계는 자동으로 폴더를 만듭니다.
3. 키워드 **Microsoft**가 있는 콘텐츠를 사용하여 컴퓨터에 텍스트 파일 **file.txt**를 만듭니다. 예: "테스트 사용자 지정 작업 Microsoft 테스트 사용자 지정 작업 Microsoft”
4. Azure Blob 저장소의 다음 입력 폴더에 파일을 업로드합니다.

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   **Azure 저장소 탐색기**를 사용 중인 경우 파일 **file.txt**를 **mycontainer**에 업로드합니다. 도구 모음의 **복사**를 클릭하여 Blob의 복사본을 만듭니다. **Blob 복사** 대화 상자에서 **대상 Blob 이름**을 **inputfolder/2015-11-16-00/file.txt**로 변경합니다. 이 단계를 반복하여 inputfolder/2015-11-16-01/file.txt, inputfolder/2015-11-16-02/file.txt, inputfolder/2015-11-16-03/file.txt, inputfolder/2015-11-16-04/file.txt 등을 만듭니다. 이 작업은 자동으로 폴더를 만듭니다.
5. **customactivitycontainer**라는 다른 컨테이너를 만듭니다. 이 컨테이너에 사용자 지정 작업 zip 파일을 업로드합니다.

#### <a name="visual-studio"></a>Visual Studio
Data Factory 솔루션에서 사용될 사용자 지정 배치 활동을 만들려면 Microsoft Visual Studio 2012 이상을 설치합니다.

### <a name="high-level-steps-to-create-the-solution"></a>솔루션을 만들기 위한 대략적인 단계
1. 데이터 처리 논리를 포함하는 사용자 지정 활동을 만듭니다.
2. 사용자 지정 작업을 사용하는 Azure 데이터 팩터리 만들기:

### <a name="create-the-custom-activity"></a>사용자 지정 작업 만들기
데이터 팩터리 사용자 지정 작업은 이 샘플 솔루션의 핵심입니다. 샘플 솔루션은 Azure 배치를 사용하여 사용자 지정 작업을 실행합니다. 사용자 지정 작업을 개발하고 Azure 데이터 팩터리 파이프라인에서 사용하는 기본 정보는 [Azure 데이터 팩터리 파이프라인에서 사용자 지정 작업 사용](data-factory-use-custom-activities.md) (영문)을 참조하세요.

Azure Data Factory 파이프라인에서 사용할 .NET 사용자 지정 작업을 만들려면 **IDotNetActivity** 인터페이스를 구현하는 클래스와 함께 **.NET 클래스 라이브러리** 프로젝트를 만들어야 합니다. 이 인터페이스는 **Execute**라는 하나의 메서드만 포함합니다. 해당 메서드의 서명은 다음과 같습니다.

    public IDictionary<string, string> Execute(
                IEnumerable<LinkedService> linkedServices,
                IEnumerable<Dataset> datasets,
                Activity activity,
                IActivityLogger logger)

이 메서드에는 이해해야 하는 몇 가지 주요 구성 요소가 포함됩니다.

* 이 메서드는 다음과 같은 네 개의 매개 변수를 사용합니다.

  1. **linkedServices**. 입/출력 데이터 원본(예: Azure Blob Storage)을 데이터 팩터리에 연결하는 연결된 서비스의 열거형 목록입니다. 이 샘플에서는 입력 및 출력 모두에 사용되는 Azure 저장소 형식의 연결된 서비스가 하나만 있습니다.
  2. **datasets**. 데이터 집합의 열거형 목록입니다. 이 매개 변수를 사용하여 입력 및 출력 데이터 집합에 정의된 위치 및 스키마를 가져올 수 있습니다.
  3. **activity**. 이 매개 변수는 현재 계산 엔터티를 나타냅니다(이 경우, Azure 배치 서비스).
  4. **logger**. 로거를 사용하면 파이프라인에 대한 "User" 로그로 노출할 디버그 주석을 기록할 수 있습니다.
* 이 메서드는 나중에 사용자 지정 작업을 함께 연결하는 데 사용할 수 있는 사전을 반환합니다. 이 기능은 아직 구현되지 않았기 때문에, 메서드로부터 빈 사전이 반환됩니다.

#### <a name="procedure-create-the-custom-activity"></a>절차: 사용자 지정 작업 만들기
1. Visual Studio에서 .NET 클래스 라이브러리 프로젝트를 만듭니다.

   1. **Visual Studio 2012**/**2013/2015**를 실행합니다.
   2. **파일**을 클릭하고 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.
   3. **템플릿**을 확장하고 **Visual C\#**를 선택합니다. 이 연습에서는 C\#를 사용하지만 다른 .NET 언어를 사용하여 사용자 지정 작업을 개발할 수도 있습니다.
   4. 오른쪽의 프로젝트 형식 목록에서 **클래스 라이브러리**를 선택합니다.
   5. **이름**에 **MyDotNetActivity**를 입력합니다.
   6. **C:\\ADF**를 **위치**로 선택합니다. 존재하지 않는 경우 폴더 **ADF**를 만듭니다.
   7. **확인**을 클릭하여 프로젝트를 만듭니다.
2. **도구**를 클릭하고 **NuGet 패키지 관리자**를 가리킨 다음 **패키지 관리자 콘솔**을 클릭합니다.
3. **패키지 관리자 콘솔**에서 다음 명령을 실행하여 **Microsoft.Azure.Management.DataFactories**를 가져옵니다.

           Install-Package Microsoft.Azure.Management.DataFactories
4. **Azure 저장소** NuGet 패키지를 프로젝트로 가져옵니다. 이 샘플에서 Blob 저장소 API를 사용하므로 이 패키지가 필요합니다.

       Install-Package Azure.Storage
5. 다음 **using** 지시문을 프로젝트의 원본 파일에 추가합니다.

       using System.IO;
       using System.Globalization;
       using System.Diagnostics;
       using System.Linq;

       using Microsoft.Azure.Management.DataFactories.Models;
       using Microsoft.Azure.Management.DataFactories.Runtime;

       using Microsoft.WindowsAzure.Storage;
       using Microsoft.WindowsAzure.Storage.Blob;
6. **namespace**의 이름을 **MyDotNetActivityNS**로 변경합니다.

       namespace MyDotNetActivityNS
7. 클래스 이름을 **MyDotNetActivity**로 변경하고 아래와 같이 **IDotNetActivity** 인터페이스에서 클래스를 파생합니다.

       public class MyDotNetActivity : IDotNetActivity
8. **IDotNetActivity** 인터페이스의 **Execute** 메서드를 **MyDotNetActivity** 클래스에 구현(추가)하고 다음 샘플 코드를 메서드에 복사합니다. 이 메서드에 사용되는 논리에 대한 설명은 [메서드 실행](#execute-method) 섹션을 참조하세요.

       /// <summary>
       /// Execute method is the only method of IDotNetActivity interface you must implement.
       /// In this sample, the method invokes the Calculate method to perform the core logic.  
       /// </summary>
       public IDictionary<string, string> Execute(
           IEnumerable<LinkedService> linkedServices,
           IEnumerable<Dataset> datasets,
           Activity activity,
           IActivityLogger logger)
       {

           // declare types for input and output data stores
           AzureStorageLinkedService inputLinkedService;

           Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);

           foreach (LinkedService ls in linkedServices)
               logger.Write("linkedService.Name {0}", ls.Name);

           // using First method instead of Single since we are using the same
           // Azure Storage linked service for input and output.
           inputLinkedService = linkedServices.First(
               linkedService =>
               linkedService.Name ==
               inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
               as AzureStorageLinkedService;

           string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
           string folderPath = GetFolderPath(inputDataset);
           string output = string.Empty; // for use later.

           // create storage client for input. Pass the connection string.
           CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
           CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();

           // initialize the continuation token before using it in the do-while loop.
           BlobContinuationToken continuationToken = null;
           do
           {   // get the list of input blobs from the input storage client object.
               BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                        true,
                                        BlobListingDetails.Metadata,
                                        null,
                                        continuationToken,
                                        null,
                                        null);

               // Calculate method returns the number of occurrences of
               // the search term (“Microsoft”) in each blob associated
               // with the data slice.
               //
               // definition of the method is shown in the next step.
               output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

           } while (continuationToken != null);

           // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
           Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

           folderPath = GetFolderPath(outputDataset);

           logger.Write("Writing blob to the folder: {0}", folderPath);

           // create a storage object for the output blob.
           CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
           // write the name of the file.
           Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

           logger.Write("output blob URI: {0}", outputBlobUri.ToString());
           // create a blob and upload the output text.
           CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
           logger.Write("Writing {0} to the output blob", output);
           outputBlob.UploadText(output);

           // The dictionary can be used to chain custom activities together in the future.
           // This feature is not implemented yet, so just return an empty dictionary.
           return new Dictionary<string, string>();
       }
9. 클래스에 다음과 같은 도우미 메서드를 추가합니다. 이 메서드는 **Execute** 메서드로 호출됩니다. 가장 중요한 점은 **Calculate** 메서드가 각 Blob을 반복하는 코드를 격리한다는 것입니다.

       /// <summary>
       /// Gets the folderPath value from the input/output dataset.
       /// </summary>
       private static string GetFolderPath(Dataset dataArtifact)
       {
           if (dataArtifact == null || dataArtifact.Properties == null)
           {
               return null;
           }

           AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
           if (blobDataset == null)
           {
               return null;
           }

           return blobDataset.FolderPath;
       }

       /// <summary>
       /// Gets the fileName value from the input/output dataset.
       /// </summary>

       private static string GetFileName(Dataset dataArtifact)
       {
           if (dataArtifact == null || dataArtifact.Properties == null)
           {
               return null;
           }

           AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
           if (blobDataset == null)
           {
               return null;
           }

           return blobDataset.FileName;
       }

       /// <summary>
       /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
       /// and prepares the output text that is written to the output blob.
       /// </summary>

       public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
       {
           string output = string.Empty;
           logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
           foreach (IListBlobItem listBlobItem in Bresult.Results)
           {
               CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
               if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
               {
                   string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                   logger.Write("input blob text: {0}", blobText);
                   string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                   var matchQuery = from word in source
                                    where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                    select word;
                   int wordCount = matchQuery.Count();
                   output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
               }
           }
           return output;
       }

    **GetFolderPath** 메서드는 데이터 집합이 가리키는 폴더로 경로를 반환하고 **GetFileName** 메서드는 데이터 집합이 가리키는 Blob/파일의 이름을 반환합니다.

        "name": "InputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "file.txt",
                "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",


    **Calculate** 메서드는 입력 파일(폴더에서 Blob)에서 **Microsoft** 키워드의 인스턴스 수를 계산합니다. 검색 용어("Microsoft")는 코드에 하드 코딩됩니다.

1. 프로젝트를 컴파일합니다. 메뉴에서 **빌드**, **솔루션 빌드**를 차례로 클릭합니다.
2. **Windows 탐색기**를 시작하고 빌드 유형에 따라 **bin\\debug** 또는 **bin\\release** 폴더로 이동합니다.
3. **\\bin\\Debug** 폴더의 이진을 모두 포함하는 **MyDotNetActivity.zip** Zip 파일을 만듭니다. 오류가 발생할 경우 문제를 발생시킨 소스 코드의 줄 번호 같은 추가 정보를 받을 수 있도록 MyDotNetActivity.**pdb** 파일을 포함할 수 있습니다.

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. **ADFTutorialDataFactory**의 연결된 서비스 **StorageLinkedService**가 사용하는 Azure Blob 저장소의 Blob 컨테이너 **customactivitycontainer**에 Blob으로 **MyDotNetActivity.zip**을 업로드합니다. Blob 컨테이너 **customactivitycontainer**가 아직 없는 경우 새로 만듭니다.

#### <a name="execute-method"></a>메서드 실행
이 섹션에서는 Execute 메서드에서 코드에 대한 자세한 내용과 참고 사항을 제공합니다.

1. 입력 컬렉션을 반복하는 멤버는 [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) 네임스페이스에 있습니다. Blob 컬렉션을 반복하려면 **BlobContinuationToken** 클래스를 사용해야 합니다. 본질적으로 루프를 종료하기 위한 메커니즘으로 토큰과 함께 do-while 루프를 사용해야 합니다. 자세한 내용은 [.NET에서 Blob 저장소를 사용하는 방법](../storage/storage-dotnet-how-to-use-blobs.md)(영문)을 참조하세요. 기본 루프는 다음과 같습니다.

     // Initialize the continuation token.
     BlobContinuationToken continuationToken = null; do { // Get the list of input blobs from the input storage client object.
     BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,

                             true,
                                       BlobListingDetails.Metadata,
                                       null,
                                       continuationToken,
                                       null,
                                       null);
     // Return a string derived from parsing each blob.

         output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

     } while (continuationToken != null);

   자세한 내용은 [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) 메서드에 대한 설명서를 참조하세요.
2. Blob 집합을 진행하는 코드는 논리적으로 do-while 루프 안으로 들어갑니다. **Execute** 메서드에서 do-while 루프는 **Calculate**라는 메서드로 Blob 목록을 전달합니다. 이 메서드는 **output** 이라는 문자열 변수를 반환하는데, 이는 세그먼트에서 모든 Blob을 반복한 결과입니다.

   **Calculate** 메서드에 전달된 Blob에서 검색 용어(**Microsoft**) 발생 수를 반환합니다.

     output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
3. **Calculate** 메서드가 작업을 완료한 후에는 새 Blob에 작성되어야 합니다. 따라서 처리된 모든 BLOB 집합에 대해 결과를 새 BLOB에 작성할 수 있습니다. 새 BLOB를 작성하려면 먼저 출력 데이터 집합을 찾습니다.

     // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
     Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
4. 이 코드는 **GetFolderPath** 도우미 메서드도 호출하여 폴더 경로(저장소 컨테이너 이름)를 검색합니다.

     folderPath = GetFolderPath(outputDataset);

   **GetFolderPath** 는 DataSet 개체를 AzureBlobDataSet로 캐스팅하며 여기에는 FolderPath라는 속성이 포함됩니다.

     AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

     return blobDataset.FolderPath;
5. 이 코드는 파일 이름(BLOB 이름)을 검색할 **GetFileName** 메서드를 호출합니다. 이 코드는 폴더 경로를 가져오는 위의 코드와 유사합니다.

     AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

     return blobDataset.FileName;
6. 파일 이름은 URI 개체를 만들어 기록합니다. URI 생성자는 컨테이너 이름을 반환하는 **BlobEndpoint** 속성을 사용합니다. 출력 BLOB URI를 생성하기 위해 폴더 경로 및 파일 이름이 추가됩니다.  

     // Write the name of the file.
     Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
7. 파일 이름이 작성되었고 이제 **Calculate** 메서드에서 새 Blob으로 출력 문자열을 작성할 수 있습니다.

     // Create a blob and upload the output text.
     CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials); logger.Write("Writing {0} to the output blob", output); outputBlob.UploadText(output);

### <a name="create-the-data-factory"></a>데이터 팩터리 만들기
[사용자 지정 작업 만들기](#create-the-custom-activity) 섹션에서 사용자 지정 작업을 만들고 이진과 함께 zip 파일을 업로드하고 PDB 파일을 Azure Blob 컨테이너에 업로드했습니다. 이 섹션에서는 **사용자 지정 작업**을 사용하는 **파이프라인**으로 Azure **데이터 팩터리**를 만듭니다.

사용자 지정 작업에 대한 입력 데이터 집합은 Blob 저장소에서 입력 폴더(mycontainer\\inputfolder)의 Blob(파일)을 나타냅니다. 이 작업에 대한 출력 데이터 집합은 Blob 저장소에서 출력 폴더(mycontainer\\outputfolder)의 출력 Blob을 나타냅니다.

입력 폴더에 있는 하나 이상의 파일을 삭제합니다.

    mycontainer -\> inputfolder
        2015-11-16-00
        2015-11-16-01
        2015-11-16-02
        2015-11-16-03
        2015-11-16-04

예를 들어 각 폴더에 다음과 같은 콘텐츠로 하나의 파일(file.txt)을 삭제합니다.

    test custom activity Microsoft test custom activity Microsoft

각 입력 폴더는 폴더에 2개 이상의 파일이 포함된 경우에도 Azure Data Factory의 조각에 해당합니다. 각 조각이 파이프라인으로 처리될 때 사용자 지정 작업은 해당 조각에 대한 입력 폴더에서 모든 BLOB를 반복합니다.

동일한 콘텐츠를 가진 5개의 출력 파일이 표시됩니다. 예를 들어 2015-11-16-00 폴더의 파일 처리에서 출력 파일은 다음 내용을 포함합니다.

    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.

동일한 콘텐츠를 가진 여러 개의 파일(file.txt, file2.txt, file3.txt)을 입력 폴더로 삭제하면 출력 파일에 다음 내용이 표시됩니다. 각 폴더(2015-11-16-00 등)는 폴더에 여러 개의 입력 파일이 있더라도 이 샘플에 있는 분할 영역에 해당합니다.

    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.


출력 파일은 이제 조각(2015-11-16-00)과 연결된 폴더의 각 입력 파일(Blob)에 하나씩 세 개의 줄을 가집니다.

각 작업 실행에 대한 작업(task)이 만들어집니다. 이 샘플에서는 파이프라인에 하나의 작업만 있습니다. 조각이 파이프라인에 의해 처리될 때 사용자 지정 작업은 조각을 처리하도록 Azure 배치에서 실행됩니다. 5개 조각(각 조각은 여러 Blob 또는 파일을 가질 수 있음)이 있으므로 Azure 배치에서 생성된 5개의 작업이 있게 됩니다. 작업이 일괄 처리에서 실행될 때 실제로 사용자 지정 작업이 실행됩니다.

다음 연습에서는 추가 정보를 제공합니다.

#### <a name="step-1-create-the-data-factory"></a>1단계: 데이터 팩터리 만들기
1. [Azure 포털](https://portal.azure.com/)에 로그인한 후에 다음 단계를 수행합니다.

   1. 왼쪽 메뉴에서 **새로 만들기**를 클릭합니다.
   2. **새** 블레이드에서 **데이터 + 분석**을 클릭합니다.
   3. **데이터 분석** 블레이드에서 **Data Factory**를 클릭합니다.
2. **새 Data Factory** 블레이드에서 이름으로 **CustomActivityFactory**를 입력합니다. Azure Data Factory 이름은 전역적으로 고유해야 합니다. **"CustomActivityFactory" Data Factory 이름은 사용할 수 없습니다.**라는 오류 메시지가 표시되는 경우 Data Factory 이름을 변경하고(예: **yournameCustomActivityFactory**) 해당 Data Factory를 다시 만듭니다.
3. **리소스 그룹 이름**을 클릭하여 기존 리소스 그룹을 선택하거나 리소스 그룹을 만듭니다.
4. 데이터 팩터리를 만들려는 구독 및 지역을 사용하고 있는지 확인합니다.
5. **새 Data Factory** 블레이드에서 **만들기**를 클릭합니다.
6. Azure 포털의 **대시보드**에 생성된 데이터 팩터리가 표시됩니다.
7. 데이터 팩터리 만들기를 완료한 후에는 데이터 팩터리 페이지가 표시되며 여기에 데이터 팩터리의 내용이 표시됩니다.

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>2단계: 연결된 서비스 만들기
연결된 서비스는 데이터 저장소 또는 계산 서비스를 Azure Data Factory에 연결합니다. 이 단계에서는 **Azure Storage** 계정 및 **Azure 배치** 계정을 데이터 팩터리에 연결합니다.

#### <a name="create-azure-storage-linked-service"></a>Azure 저장소 연결된 서비스 만들기
1. **CustomActivityFactory**에 대한 **Data Factory** 블레이드에서 **작성 및 배포 타일**을 클릭합니다. 데이터 팩터리 편집기가 표시됩니다.
2. 명령 모음에서 **새 데이터 저장소**를 클릭하고 **Azure 저장소**를 선택합니다. 편집기에 Azure 저장소 연결된 서비스를 만들기 위한 JSON 스크립트가 표시됩니다.

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. **계정 이름**을 Azure 저장소 계정 이름으로 변경하고 **계정 키**를 Azure 저장소 계정의 액세스 키로 변경합니다. 저장소 액세스 키를 확보하는 방법을 알아보려면 [저장소 액세스 키 보기, 복사 및 다시 생성](../storage/storage-create-storage-account.md#manage-your-storage-account)을 참조하세요.

4. 명령 모음에서 **배포** 를 클릭하여 연결된 서비스를 배포합니다.

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Azure 배치 연결된 서비스 만들기
이 단계에서는 데이터 팩터리 사용자 지정 작업을 실행하는 데 사용될 **Azure 배치** 계정에 대한 연결된 서비스를 만듭니다.

1. 명령 모음에서 **새 계산**을 클릭하고 **Azure Batch**를 선택합니다. 편집기에 Azure 배치 연결된 서비스를 만들기 위한 JSON 스크립트가 표시됩니다.
2. JSON 스크립트에서:

   1. **계정 이름**을 Azure 배치 계정의 이름으로 대체합니다.
   2. **액세스 키**를 Azure 배치 계정의 액세스 키로 대체합니다.
   3. **poolName** 속성에 대한 풀 ID를 입력합니다**.**  이 속성의 경우 풀 이름 또는 풀 ID 중 하나를 지정할 수 있습니다.
   4. **batchUri** JSON 속성에 대한 배치 URI를 입력합니다.

      > [!IMPORTANT]
      > **Azure 배치 계정 블레이드**의 **URL**은 \<accountname\>.\<region\>.batch.azure.com 형식을 사용합니다. **batchUri** JSON 속성의 경우 URL에서 **"accountname."을 제거**한 다음 해야 합니다. 예: `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      **poolName** 속성의 경우 풀 이름 대신 풀 ID를 지정할 수도 있습니다.

      > [!NOTE]
      > 데이터 팩터리 서비스는 HDInsight에 대해서와 마찬가지로 Azure Batch에 대한 주문형 옵션을 지원하지 않습니다. Azure Data Factory에서는 사용자 고유의 Azure Batch 풀만 사용할 수 있습니다.
      >
      >
   5. **linkedServiceName** 속성에 대해 **StorageLinkedService**를 지정합니다. 이 연결된 서비스는 이전 단계에서 만들었습니다. 이 저장소는 파일 및 로그에 대한 준비 영역으로 사용됩니다.
3. 명령 모음에서 **배포**를 클릭하여 연결된 서비스를 배포합니다.

#### <a name="step-3-create-datasets"></a>3단계: 데이터 집합 만들기
이 단계에서는 입력 및 출력 데이터를 나타낼 데이터 집합을 만듭니다.

#### <a name="create-input-dataset"></a>입력 데이터 집합 만들기
1. Data Factory의 **편집기**에서 도구 모음의 **새 데이터 집합** 단추를 클릭하고 드롭다운 메뉴에서 **Azure Blob 저장소**를 클릭합니다.
2. 오른쪽 창의 JSON을 다음 JSON 코드 조각으로 바꿉니다.

       {
           "name": "InputDataset",
           "properties": {
               "type": "AzureBlob",
               "linkedServiceName": "AzureStorageLinkedService",
               "typeProperties": {
                   "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
                   "format": {
                       "type": "TextFormat"
                   },
                   "partitionedBy": [
                       {
                           "name": "Year",
                           "value": {
                               "type": "DateTime",
                               "date": "SliceStart",
                               "format": "yyyy"
                           }
                       },
                       {
                           "name": "Month",
                           "value": {
                               "type": "DateTime",
                               "date": "SliceStart",
                               "format": "MM"
                           }
                       },
                       {
                           "name": "Day",
                           "value": {
                               "type": "DateTime",
                               "date": "SliceStart",
                               "format": "dd"
                           }
                       },
                       {
                           "name": "Hour",
                           "value": {
                               "type": "DateTime",
                               "date": "SliceStart",
                               "format": "HH"
                           }
                       }
                   ]
               },
               "availability": {
                   "frequency": "Hour",
                   "interval": 1
               },
               "external": true,
               "policy": {}
           }
       }

     이 연습에서는 시작 시간: 2015-11-16T00:00:00Z 및 종료 시간: 2015-11-16T05:00:00Z로 나중에 파이프라인을 만듭니다. **매시간** 데이터를 생성하도록 예약되어 있어 5개의 입/출력 조각이 있습니다(**00**:00:00 -\> **05**:00:00 범위).

     입력 데이터 집합의 **빈도** 및 **간격**은 **Hour** 및 **1**로 설정되며 이는 입력 조각이 매시간 제공됨을 의미합니다.

     다음은 각 조각에 대한 시작 시간이며 위의 JSON 코드 조각에서 **SliceStart** 시스템 변수로 표현됩니다.

    | **조각** | **시작 시간**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**:00:00 |
    | 2         | 2015-11-16T**01**:00:00 |
    | 3         | 2015-11-16T**02**:00:00 |
    | 4         | 2015-11-16T**03**:00:00 |
    | 5         | 2015-11-16T**04**:00:00 |

     **folderPath**는 조각 시작 시간(**SliceStart**)의 연도, 월, 일 및 시간 부분을 사용하여 계산됩니다. 따라서 입력 폴더가 조각에 매핑되는 방식은 다음과 같습니다.

    | **조각** | **시작 시간**          | **입력 폴더**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04** |

1. 도구 모음에서 **배포**를 클릭하여 **InputDataset** 테이블을 만들고 배포합니다.

#### <a name="create-output-dataset"></a>출력 데이터 집합 만들기
이 단계에서는 출력 데이터를 나타내는 다른 AzureBlob 형식의 데이터 집합을 만듭니다.

1. Data Factory의 **편집기**에서 도구 모음의 **새 데이터 집합** 단추를 클릭하고 드롭다운 메뉴에서 **Azure Blob 저장소**를 클릭합니다.
2. 오른쪽 창의 JSON을 다음 JSON 코드 조각으로 바꿉니다.

       {
           "name": "OutputDataset",
           "properties": {
               "type": "AzureBlob",
               "linkedServiceName": "AzureStorageLinkedService",
               "typeProperties": {
                   "fileName": "{slice}.txt",
                   "folderPath": "mycontainer/outputfolder",
                   "partitionedBy": [
                       {
                           "name": "slice",
                           "value": {
                               "type": "DateTime",
                               "date": "SliceStart",
                               "format": "yyyy-MM-dd-HH"
                           }
                       }
                   ]
               },
               "availability": {
                   "frequency": "Hour",
                   "interval": 1
               }
           }
       }

     각 입력 조각에 대해 출력 BLOB/파일이 생성됩니다. 각 조각에 대해 출력 파일의 이름을 지정하는 방법은 다음과 같습니다. **mycontainer\\outputfolder**라는 하나의 출력 폴더에 모든 출력 파일이 생성됩니다.


    | **조각** | **시작 시간**          | **출력 파일**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00.txt** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01.txt** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02.txt** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03.txt** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04.txt** |

     입력 폴더(예: 2015-11-16-00)에 있는 모든 파일은 시작 시간(2015-11-16-00)의 조각 중 일부입니다. 이 조각을 처리할 때 사용자 지정 작업은 각 파일을 검색하고 검색 용어("Microsoft") 항목 수와 함께 출력 파일에 줄을 생성합니다. 폴더 2015-11-16-00에 세 개의 파일이 있는 경우 출력 파일(2015-11-16-00.txt)에 세 줄이 있게 됩니다.

1. 도구 모음에서 **배포**를 클릭하여 **OutputDataset**을 만들고 배포합니다.

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a>4단계: 사용자 지정 활동을 사용하여 파이프라인 만들기 및 실행
이 단계에서는 이전에 만든 하나의 작업, 사용자 지정 작업을 사용하여 파이프라인을 만듭니다.

> [!IMPORTANT]
> **file.txt**를 Blob 컨테이너의 입력 폴더에 업로드하지 않은 경우 파이프라인을 만들기 전에 업로드합니다. **isPaused** 속성은 파이프라인 JSON에서 false로 설정되었으므로 파이프라인은 즉시 과거의 **시작** 날짜로 실행합니다.
>
>

1. 데이터 팩터리 편집기의 명령 모음에서 **새 파이프라인**을 클릭합니다. 명령이 표시되지 않으면 **... (줄임표)**을 클릭하여 표시합니다.
2. 오른쪽 창의 JSON을 다음 JSON 스크립트로 바꿉니다.

       {
           "name": "PipelineCustom",
           "properties": {
               "description": "Use custom activity",
               "activities": [
                   {
                       "type": "DotNetActivity",
                       "typeProperties": {
                           "assemblyName": "MyDotNetActivity.dll",
                           "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                           "packageLinkedService": "AzureStorageLinkedService",
                           "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                       },
                       "inputs": [
                           {
                               "name": "InputDataset"
                           }
                       ],
                       "outputs": [
                           {
                               "name": "OutputDataset"
                           }
                       ],
                       "policy": {
                           "timeout": "00:30:00",
                           "concurrency": 5,
                           "retry": 3
                       },
                       "scheduler": {
                           "frequency": "Hour",
                           "interval": 1
                       },
                       "name": "MyDotNetActivity",
                       "linkedServiceName": "AzureBatchLinkedService"
                   }
               ],
               "start": "2015-11-16T00:00:00Z",
               "end": "2015-11-16T05:00:00Z",
               "isPaused": false
          }
       }

   다음 사항에 유의하세요.

   * 파이프라인에는 **DotNetActivity** 유형의 작업 하나만 있습니다.
   * **AssemblyName**은 **MyDotNetActivity.dll** DLL 이름으로 설정합니다.
   * **EntryPoint**는 **MyDotNetActivityNS.MyDotNetActivity**로 설정합니다. 기본적으로 코드에 있는 \<namespace\>.\<classname\>입니다.
   * **PackageLinkedService**가 사용자 지정 작업 zip 파일을 포함하는 Blob 저장소를 가리키는 **StorageLinkedService**로 설정됩니다. 입/출력 파일 및 사용자 지정 작업 zip 파일에 대해 서로 다른 Azure Storage 계정을 사용하는 경우 다른 Azure Storage 연결된 서비스를 만들어야 합니다. 이 문서에서는 동일한 Azure 저장소 계정을 사용 중이라고 가정합니다.
   * **PackageFile**은 **customactivitycontainer/MyDotNetActivity.zip**으로 설정합니다. \<containerforthezip\>/\<nameofthezip.zip\> 형식입니다.
   * 사용자 지정 작업은 입력으로 **InputDataset**, 출력으로 **OutputDataset**을 사용합니다.
   * 사용자 지정 활동의 **linkedServiceName** 속성은 **AzureBatchLinkedService**를 가리키며 Azure Data Factory에 사용자 지정 작업을 Azure 배치에서 실행해야 함을 알려줍니다.
   * **동시성** 설정은 중요합니다. Azure 배치 풀에서 2개 이상의 계산 노드를 가지더라도 1인 기본값을 사용하는 경우 조각은 차례로 처리됩니다. 따라서 Azure 배치의 병렬 처리 기능을 활용하지 못합니다. **동시성**을 더 높은 값, 예를 들어 2로 설정한 경우 2조각(Azure 배치에서 2개의 작업에 해당)은 동시에 처리될 수 있으며 이 경우 Azure 배치 풀의 두 VM이 활용됩니다. 따라서 동시성 속성을 적절하게 설정합니다.
   * 기본적으로 VM에서 언제든지 하나의 작업(조각)이 실행됩니다. 기본적으로 **VM당 최대 작업**이 Azure 배치 풀에 대해 1로 설정되었기 때문입니다. 필수 구성의 일부로 2로 설정된 이 속성으로 풀을 만들었으므로 두 개의 데이터 팩터리 조각이 VM에서 동시에 실행될 수 있습니다.

    -   **isPaused** 속성은 기본적으로 false로 설정됩니다. 이 예제에서는 조각이 이전에 시작되므로 파이프라인이 즉시 실행됩니다. 파이프라인을 일시 중지하려면 이 속성을 true로 설정하고 다시 시작하려면 false로 다시 설정할 수 있습니다.

    -   **start** 시간과 **end** 시간은 5시간 차이이며 파이프라인이 작동하면서 매시간 5개 조각이 생성됩니다.

1. 명령 모음에서 **배포**를 클릭하여 파이프라인을 배포합니다.

#### <a name="step-5-test-the-pipeline"></a>5단계: 파이프라인 테스트
이 단계에서는 입력 폴더에 파일을 삭제하여 파이프라인을 테스트합니다. 하나의 입력 폴더당 하나의 파일을 사용하여 파이프라인 테스트를 시작합니다.

1. Azure 포털의 Data Factory 블레이드에서 **다이어그램**을 클릭합니다.

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. 다이어그램 뷰에서 **InputDataset** 입력 데이터 집합을 두 번 클릭합니다.

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. 모든 5조각이 준비된 상태로 **InputDataset** 블레이드가 표시됩니다. 각 조각에 대해 **SLICE START TIME** 및 **SLICE END TIME**입니다.

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. **다이어그램 뷰**에서 **OutputDataset**을 클릭합니다.
5. 5개의 출력 조각이 이미 생성된 경우 준비 상태로 표시됩니다.

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. Azure 포털을 사용하여 **조각**과 연결된 **태스크**를 보고 각 조각이 실행된 VM을 봅니다. 자세한 내용은 [Data Factory 및 배치 통합](#data-factory-and-batch-integration) 섹션을 참조하세요.
7. Azure Blob 저장소에서 **mycontainer**의 **outputfolder**에 출력 파일이 표시됩니다.

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   각 입력 조각에 대해 하나씩 5개의 출력 파일이 표시됩니다. 각 출력 파일은 다음 출력과 유사한 콘텐츠를 가져야 합니다.

       2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.

   다음 다이어그램에서는 데이터 팩터리 조각이 Azure 배치의 작업에 매핑하는 방법을 보여 줍니다. 이 예제에서는 하나의 조각은 하나의 실행만 가집니다.

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. 이제 폴더의 여러 파일을 시도해 보겠습니다. 파일 만들기: **2015-11-06-01** 폴더의 file.txt와 동일한 콘텐츠를 가진 **file2.txt**, **file3.txt**, **file4.txt** 및 **file5.txt**
9. 출력 폴더에서 출력 파일 **2015-11-16-01.txt**를 **삭제**합니다.
10. 이제 **OutputDataset** 블레이드에서 **11/16/2015 01:00:00 AM**으로 설정된 **SLICE START TIME**으로 조각을 오른쪽 단추로 클릭하고 **실행**을 클릭하여 조각을 다시 실행/다시 처리합니다. 이제 조각에 하나의 파일 대신 5개의 파일이 있습니다.

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. 조각이 실행되고 해당 상태가 **Ready** 상태가 된 후 Blob 저장소에 있는 **mycontainer**의 **outputfolder**에 있는 이 조각(**2015-11-16-01.txt**)에 대한 출력 파일의 콘텐츠를 확인합니다. 조각의 각 파일에 대한 줄이 있어야 합니다.

        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.

> [!NOTE]
> 5개의 입력 파일을 시도하기 전에 출력 파일 2015-11-16-01.txt를 삭제하지 않은 경우 이전 조각 실행에서 한 줄이 표시되고 현재 조각 실행에서 5줄이 표시됩니다. 기본적으로 콘텐츠는 이미 있는 경우 출력 파일에 추가됩니다.
>
>

#### <a name="data-factory-and-batch-integration"></a>Data Factory 및 배치 통합
Data Factory 서비스가 Azure 배치에 **adf-poolname:job-xxx**라는 이름으로 작업을 만듭니다.

![Azure Data Factory - 배치 작업](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

조각의 각 작업 실행에 대한 작업(task)이 작업(job)에 만들어집니다. 처리를 위해 준비된 10개 조각이 있는 경우 이 작업에 10개 작업(task)이 만들어집니다. 풀에 여러 계산 노드가 있는 경우 병렬로 실행 중인 두 개 이상의 조각을 포함할 수 있습니다. 계산 노드당 최대 작업이 1보다 크게 설정된 경우에도 동일한 계산에 실행 중인 두 개 이상의 조각을 포함할 수 있습니다.

이 예제에서는 5개 조각이 있으므로 Azure 배치에 5개의 작업이 있게 됩니다. Azure Data Factory의 파이프라인 JSON에서 **동시성**을 **5**로, **2**개의 VM이 있는 Azure 배치 풀에서 **VM당 최대 태스크**를 **2**로 설정하여 태스크가 매우 빠르게 실행됩니다(태스크에 대한 시작 및 종료 시간 확인).

포털을 사용하여 배치 작업 및 **조각**과 연결된 작업을 보고 각 조각이 실행된 VM을 봅니다.

![Azure Data Factory - 배치 작업 태스크](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>파이프라인 디버깅
디버깅은 몇 가지 기본적인 방법으로 구성됩니다.

1. 입력 조각이 **Ready**로 설정되지 않은 경우 입력 폴더 구조가 올바르고 file.txt가 입력 폴더에 있는지 확인합니다.

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. 사용자 지정 작업의 **Execute** 메서드에서 **IActivityLogger** 개체를 사용하여 문제 해결에 도움이 되는 정보를 기록합니다. 기록된 메시지는 user\_0.log 파일에 표시됩니다.

   **OutputDataset** 블레이드에서 조각에 대한 **데이터 조각** 블레이드를 보려면 해당 조각을 클릭합니다. 해당 조각에 대한 **작업 실행** 이 표시됩니다. 해당 조각에 대한 하나의 작업 실행이 표시됩니다. 명령 모음에서 **실행**을 클릭하면 동일한 조각에 대해 다른 작업 실행을 시작할 수 있습니다.

   작업 실행을 클릭하면 로그 파일 목록과 함께 **작업 실행 세부 정보** 블레이드가 표시됩니다. 기록된 메시지는 **user\_0.log** 파일에 표시됩니다. 오류가 발생하면 파이프라인/작업 JSON에서 재시도 횟수가 3으로 설정되므로 세 개의 작업 실행이 표시됩니다. 작업 실행을 클릭하면 문제 해결을 위해 검토할 수 있는 로그 파일이 표시됩니다.

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   로그 파일 목록에서 **user-0.log**를 클릭합니다. 오른쪽 패널은 **IActivityLogger.Write** 메서드를 사용한 결과입니다.

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   또한 system-0.log에서 시스템 오류 메시지 및 예외를 확인합니다.

       Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...

       Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...

       Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module

       Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
3. Zip 파일에 **PDB** 파일을 포함하여 오류 발생 시 오류 세부 정보에 **호출 스택**과 같은 정보가 포함되도록 합니다.
4. 사용자 지정 작업에 대한 zip 파일의 모든 파일은 하위 폴더가 없는 **최상위**여야 합니다.

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. **assemblyName**(MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile**(customactivitycontainer/MyDotNetActivity.zip) 및 **packageLinkedService**(zip 파일을 포함하는 Azure Blob 저장소를 가리켜야 함)가 올바른 값으로 설정되었는지 확인합니다.
6. 오류를 해결했고 조각을 다시 처리하려면 **OutputDataset** 블레이드에서 조각을 마우스 오른쪽 단추로 클릭하고 **실행**을 클릭합니다.

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > **dfjobs**라는 **컨테이너**가 Azure Blob 저장소에 표시됩니다. 이 컨테이너는 자동으로 삭제되지 않지만 솔루션 테스트 후 안전하게 삭제할 수 있습니다. 마찬가지로 데이터 팩터리 솔루션은 **adf-\<pool ID/name\>:job-0000000001**이라는 Azure 배치 **작업**을 만듭니다. 원하는 경우 솔루션을 테스트한 후 이 작업을 삭제할 수 있습니다.
   >
   >
7. 사용자 지정 작업은 패키지에서 **app.config** 파일을 사용하지 않습니다. 따라서 코드가 구성 파일에서 연결 문자열을 읽는 경우 런타임 시 작동하지 않습니다. Azure 배치를 사용할 경우 **Azure KeyVault**에 모든 암호를 저장하고, 인증서 기반 서비스 주체를 사용하여 KeyVault를 보호하고, 인증서를 Azure 배치 풀에 배포하는 것이 좋습니다. 그러면 .NET 사용자 지정 활동은 런타임에 주요 자격 증명 모음의 암호에 액세스할 수 있습니다. 이 솔루션은 일반 솔루션이며 연결 문자열뿐 아니라 모든 유형의 암호로 확장될 수 있습니다.

    최상은 아니지만 좀 더 쉬운 해결 방법이 있습니다. 즉 연결 문자열 설정을 사용하여 **Azure SQL 연결 서비스**를 만들고, 이 서비스를 사용하는 데이터 집합을 만든 다음 사용자 지정 .NET 작업에 이 데이터 집합을 더미 입력 데이터 집합으로 연결하면 됩니다. 그런 후 사용자 지정 활동 코드에서 연결된 서비스의 연결 문자열에 액세스할 수 있습니다. 그러면 런타임에 문제 없이 작동합니다.  

#### <a name="extend-the-sample"></a>샘플 확장
Azure Data Factory 및 Azure 배치 기능에 대한 자세한 내용을 보려면 이 샘플을 확장할 수 있습니다. 예를 들어 서로 다른 시간 범위에서 조각을 처리하려면 다음 단계를 수행합니다.

1. **inputfolder**2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08, 2015-11-16-09에 다음 하위 폴더를 추가하고 해당 폴더에 입력 파일을 배치합니다. `2015-11-16T05:00:00Z`에서 `2015-11-16T10:00:00Z`(으)로 파이프라인에 대한 종료 시간을 변경합니다. **다이어그램 보기**에서 **InputDataset**을 두 번 클릭하고 입력 조각이 준비되었는지 확인합니다. **OuptutDataset**을 두 번 클릭하여 출력 조각의 상태를 봅니다. 준비 상태에 있는 경우 출력 파일에 대한 outputfolder를 확인합니다.
2. 솔루션, 특히 Azure 배치에서 발생하는 처리의 성능에 미치는 영향을 이해하려면 **동시성** 설정을 증가 또는 감소시킵니다. (4단계: **동시성** 설정에 대한 파이프라인 만들기 및 실행을 참조하세요.)
3. 상위/하위 **VM당 최대 작업**을 가진 풀을 만듭니다. 만든 새 풀을 사용하려면 데이터 팩터리 솔루션에서 Azure 배치 연결된 서비스를 업데이트합니다. (4단계: **VM당 최대 작업** 설정에 대한 파이프라인 만들기 및 실행을 참조하세요.)
4. **자동 크기 조정** 기능이 있는 Azure 배치 풀을 만듭니다. Azure Batch 풀에서 자동으로 계산 노드 크기를 조정하는 것은 응용 프로그램에서 사용하는 처리 능력을 동적으로 조정하는 것입니다. 예를 들어 보류 중인 작업의 수에 따라 0 전용 VM 및 자동 크기 조정 수식을 사용하여 Azure 배치 풀을 만들 수 있습니다.

   한 번에 보류 중인 작업당 하나의 VM(예: 보류 중인 5개의 작업 -> 5개의 VM)

       pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
       $TargetDedicated = max(pendingTaskSampleVector);

   보류 중인 작업의 수에 관계 없이 한 번에 최대 하나의 VM

       pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
       $TargetDedicated = (max(pendingTaskSampleVector)>0)?1:0;

   자세한 내용은 [Azure 배치 풀에서 자동으로 계산 노드 크기 조정](../batch/batch-automatic-scaling.md) 을 참조하세요.

   풀에 기본 [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx)이 사용되는 경우, 배치 서비스가 사용자 지정 작업을 실행하기 전에 VM을 준비하는 데 15~30분이 소요될 수 있습니다.  풀에 다른 autoScaleEvaluationInterval이 사용되는 경우, 배치 서비스는 autoScaleEvaluationInterval +10분이 소요될 수 있습니다.
5. 샘플 솔루션에서 **Execute** 메서드는 출력 데이터 조각을 생성하도록 입력 데이터 조각을 처리하는 **Calculate** 메서드를 호출합니다. 사용자 고유 메서드를 작성하여 입력 데이터를 처리하고 Execute 메서드의 Calculate 메서드 호출을 사용자 메서드에 호출로 대체할 수 있습니다.

### <a name="next-steps-consume-the-data"></a>다음 단계: 데이터 사용
데이터를 처리한 후 **Microsoft Power BI**와 같은 온라인 도구와 함께 사용할 수 있습니다. 다음은 Power BI 및 Azure에서 사용하는 방법을 이해하는 데 도움을 주는 링크입니다.

* [Power BI에서 데이터 집합 탐색](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-data/)
* [Power BI Desktop 시작](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/)
* [Power BI에서 데이터 새로 고침](https://powerbi.microsoft.com/en-us/documentation/powerbi-refresh-data/)
* [Azure 및 Power BI - 기본 개요](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>참조
* [Azure 데이터 팩터리](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Azure Data Factory 서비스 소개](data-factory-introduction.md)
  * [Azure Data Factory 시작](data-factory-build-your-first-pipeline.md)
  * [Azure Data Factory 파이프라인에서 사용자 지정 작업 사용](data-factory-use-custom-activities.md)
* [Azure 배치](https://azure.microsoft.com/documentation/services/batch/)

  * [Azure 배치의 기본 사항](../batch/batch-technical-overview.md)
  * [Azure 배치 기능 개요](../batch/batch-api-basics.md)
  * [Azure 포털에서 Azure 배치 계정 만들기 및 관리](../batch/batch-account-create-portal.md)
  * [Azure 배치 라이브러리 .NET 시작](../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx



<!--HONumber=Nov16_HO3-->


