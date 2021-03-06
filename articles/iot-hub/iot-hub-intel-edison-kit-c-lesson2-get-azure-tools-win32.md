---
title: "Azure IoT 시작 키트(Windows 7 이상)에 대한 Azure 도구 받기 | Microsoft Docs"
description: "Windows 7 이상 버전에 Python 및 Azure CLI(Azure 명령줄 인터페이스)를 설치합니다."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "azure cli, iot 클라우드 서비스, arduino 클라우드"
ms.assetid: 1035760e-cdd1-4d99-8003-067e98b29762
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/8/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: f45b3bf00d619376ac07418f0c02eca5f3241939
ms.openlocfilehash: dbb40d01aaaa94b3b579543449563dfe23390383


---
# <a name="get-azure-tools-windows-7-and-later"></a>Azure 도구 얻기(Windows 7 이상)
> [!div class="op_single_selector"]
> * [Windows 7 이상][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>수행할 사항
Python 및 Azure CLI(Azure 명령줄 인터페이스)를 설치합니다. 문제가 있으면 [문제 해결 페이지][troubleshooting]에서 솔루션을 검색하세요.

## <a name="what-you-will-learn"></a>알아볼 내용
이 문서에서는 다음에 대해 알아봅니다.
* Python을 설치하는 방법.
* Azure CLI를 설치하는 방법.

## <a name="what-you-need"></a>필요한 항목
* 인터넷에 연결된 Windows 컴퓨터.
* 활성 Azure 구독. Azure 계정이 없는 경우 몇 분 만에 [무료 Azure 평가판 계정](http://azure.microsoft.com/pricing/free-trial/)을 만들 수 있습니다.

## <a name="install-python"></a>Python 설치
Windows 컴퓨터에 [Python을 설치](https://www.python.org/downloads/)합니다. Python 2.7, 3.4 또는 3.5를 설치할 수 있습니다. 이 자습서는 Python 2.7을 기반으로 합니다. Python을 이미 설치한 경우 다음 섹션으로 이동하여 Azure CLI를 설치합니다.

python.exe 및 pip.exe가 설치되어 있는 폴더의 경로를 시스템 `PATH` 환경 변수에 추가해야 합니다. 기본적으로 python.exe는 `C:\Python27`에, pip.exe는 `C:\Python27\Scripts`에 설치되어 있습니다.

## <a name="install-the-azure-cli"></a>Azure CLI 설치
Azure CLI는 Azure에 대한 다중 플랫폼 명령줄 환경을 제공합니다. 명령줄에서 리소스를 프로비전하고 관리하는 작업을 바로 수행할 수 있습니다.

Azure CLI를 설치하려면 다음 단계를 따르세요.

1. 관리자로 명령 프롬프트 창을 엽니다.
2. 다음 명령을 실행합니다.

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. 다음 명령을 실행하여 설치를 확인합니다.

   ```cmd
   az iot -h
   ```

성공적으로 설치되면 다음과 같은 출력이 표시됩니다.

![성공을 나타내는 출력](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a>요약
Azure CLI를 설치했습니다. 다음 작업은 Azure CLI를 사용하여 Azure IoT Hub 및 장치 ID를 만드는 것입니다.

## <a name="next-steps"></a>다음 단계
[IoT Hub 만들기 및 Intel Edison 등록][create-your-iot-hub-and-register-intel-edison]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md



<!--HONumber=Dec16_HO2-->


