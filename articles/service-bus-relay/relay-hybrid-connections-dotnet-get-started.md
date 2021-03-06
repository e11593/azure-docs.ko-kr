---
title: ".NET에서 Azure 릴레이 하이브리드 연결 시작 | Microsoft Docs"
description: "하이브리드 연결에 대한 C# 콘솔 응용 프로그램을 작성하는 방법"
services: service-bus-relay
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/28/2016
ms.author: jotaub,sethm
translationtype: Human Translation
ms.sourcegitcommit: ca66a344ea855f561ead082091c6941540b1839d
ms.openlocfilehash: a645d742c1e6d80e86ac29d78d78422b5b47a286


---
# <a name="get-started-with-relay-hybrid-connections"></a>릴레이 하이브리드 연결 시작
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>수행될 작업
하이브리드 연결에는 클라이언트와 서버 구성 요소가 모두 필요하므로 이 자습서에서는 두 개의 콘솔 응용 프로그램을 만듭니다. 단계는 다음과 같습니다.

1. Azure 포털을 사용하여 릴레이 네임스페이스를 만듭니다.
2. Azure 포털을 사용하여 하이브리드 연결을 만듭니다.
3. 메시지를 수신하는 서버 콘솔 응용 프로그램을 작성합니다.
4. 메시지를 보내는 클라이언트 콘솔 응용 프로그램을 작성합니다.

## <a name="prerequisites"></a>필수 조건
1. [Visual Studio 2013 또는 Visual Studio 2015](http://www.visualstudio.com). 이 자습서의 예제에서는 Visual Studio 2015를 사용합니다.
2. Azure 구독.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Azure 포털을 사용하여 네임스페이스 만들기
릴레이 네임스페이스를 이미 만든 경우 [Azure 포털을 사용하여 하이브리드 연결 만들기](#2-create-a-hybrid-connection-using-the-azure-portal) 섹션으로 이동합니다.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Azure 포털을 사용하여 하이브리드 연결 만들기
하이브리드 연결을 이미 만든 경우 [서버 응용 프로그램 만들기](#3-create-a-server-application-listener) 섹션으로 이동합니다.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. 서버 응용 프로그램(수신기) 만들기
릴레이에서 메시지를 수신하고 받으려면 Visual Studio를 사용하여 C# 콘솔 응용 프로그램을 작성합니다.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. 클라이언트 응용 프로그램(보낸 사람) 만들기
릴레이에 메시지를 보내려면 Visual Studio를 사용하여 C# 콘솔 응용 프로그램을 작성합니다.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. 응용 프로그램 실행
1. 서버 응용 프로그램을 실행합니다.
2. 클라이언트 응용 프로그램을 실행하고 일부 텍스트를 입력합니다.
3. 서버 응용 프로그램 콘솔이 클라이언트 응용 프로그램에 입력된 텍스트를 출력하는지 확인합니다.

![응용 프로그램 실행](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

축하합니다. 종단 간 하이브리드 연결 응용 프로그램을 만들었습니다.

## <a name="next-steps"></a>다음 단계:
* [릴레이 FAQ](relay-faq.md)
* [네임스페이스 만들기](relay-create-namespace-portal.md)
* [Node 시작](relay-hybrid-connections-node-get-started.md)




<!--HONumber=Jan17_HO4-->


