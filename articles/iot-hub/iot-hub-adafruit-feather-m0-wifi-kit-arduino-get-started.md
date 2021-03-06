---
title: "Adafruit Feather M0 WiFi Azure IoT 시작 키트 시작 | Microsoft Docs"
description: "Adafruit Feather M0 WiFi를 시작하여 Azure IoT Hub를 만들고 IoT Hub에 Adafruit Feather M0 WiFi를 연결합니다."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "azure iot hub, 사물 인터넷 시작, 사물 인터넷 자습서, adafruit 사물 인터넷, arduino 시작"
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 9e8084fe26229ef9ed1676c0a3c34e0ee7be88b1
ms.openlocfilehash: 8689ee35e880b1aa774b09bb194b43f280b2af79


---
# <a name="get-started-with-your-arduino-board-adafruit-feather-m0-wifi"></a>Arduino 보드 시작: Adafruit Feather M0 WiFi

이 자습서에서는 Arduino 보드를 실행하는 작업의 기초부터 학습합니다. 그런 다음 [Azure IoT Hub](iot-hub-what-is-iot-hub.md)를 사용하여 장치를 클라우드에 원활하게 연결하는 방법을 알아봅니다.

## <a name="lesson-1-configure-your-device"></a>단원 1: 장치 구성
![단원 1 종단간 다이어그램][Lesson-1-end-to-end-diagram]

이 단원에서는 운영 체제를 통해 Arduino 보드를 구성하고, 개발 환경을 설정하며, 응용 프로그램을 Arduino 보드에 배포합니다.

### <a name="configure-your-device"></a>장치 구성
보드를 조합하여 전원을 켜서 Arduino 보드를 처음 사용할 수 있도록 구성합니다.

*예상 완료 시간: 5분*  

[장치 구성][configure-your-device]으로 이동

### <a name="get-the-tools"></a>도구 얻기
Arduino 보드의 첫 번째 응용 프로그램을 빌드 및 배포하기 위한 도구 및 소프트웨어를 다운로드합니다.

*예상 완료 시간: 20분* 

[도구 얻기][get-the-tools]로 이동

### <a name="create-and-deploy-the-blink-application"></a>깜박임 응용 프로그램 만들기 및 배포
Github에서 샘플 Arduino 깜박임 응용 프로그램을 복제하고, gulp를 사용하여 이 응용 프로그램을 Arduino 보드에 배포합니다. 이 샘플 응용 프로그램은 GPIO #13 온보드 LED를 2초마다 깜박이게 합니다.

*예상 완료 시간: 5분*  

[깜박임 응용 프로그램 만들기 및 배포][create-and-deploy-the-blink-application]로 이동

## <a name="lesson-2-create-your-iot-hub"></a>단원 2: IoT hub 만들기
![단원 2 종단간 다이어그램][lesson-2-end-to-end-diagram]

이 단원에서는 무료 Azure 계정을 만들고, Azure IoT Hub를 프로비전하며, IoT Hub에서 첫 번째 장치를 만듭니다.

이 단원을 시작하기 전에 1단원을 완료합니다.

### <a name="get-the-azure-tools"></a>Azure 도구 얻기
Azure 명령줄 인터페이스(Azure CLI) 설치.

*예상 완료 시간: 10분*  

[Azure 도구 얻기][get-azure-tools]로 이동

### <a name="create-your-iot-hub-and-register-your-arduino-board"></a>IoT Hub 만들기 및 Arduino 보드 등록
리소스 그룹을 만들고, 첫 번째 Azure IoT Hub를 프로비전하며, Azure CLI를 사용하여 첫 번째 장치를 IoT Hub에 추가합니다.

*예상 완료 시간: 10분*  

[IoT Hub 만들기 및 Arduino 보드 등록][create-your-iot-hub-and-register-your-arduino-board]으로 이동

## <a name="lesson-3-send-device-to-cloud-messages"></a>단원 3: 장치-클라우드 메시지 보내기
![단원 3 종단간 다이어그램][lesson-3-end-to-end-diagram]

이 단원에서는 Arduino 보드의 메시지를 IoT Hub로 보냅니다. 또한 IoT Hub에서 들어오는 메시지를 가져와서 Azure Table Storage에 쓰는 Azure 함수 앱을 만듭니다.

이 단원을 시작하기 전에 단원 1과 단원 2를 완료합니다.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Azure 함수 앱 및 Azure Storage 계정 만들기
Azure Resource Manager 템플릿을 사용하여 Azure 함수 앱 및 Azure Storage 계정을 만듭니다.

*예상 완료 시간: 10분*  

[Azure 함수 앱 및 Azure Storage 계정 만들기][create-an-azure-function-app-and-azure-storage-account]로 이동

### <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>샘플 응용 프로그램을 실행하여 장치-클라우드 메시지 보내기
IoT Hub에 메시지를 보내는 샘플 응용 프로그램을 Arduino 보드에 배포 및 실행합니다.

*예상 완료 시간: 10분*  

[샘플 응용 프로그램을 실행하여 장치-클라우드 메시지 보내기][send-device-to-cloud-messages]로 이동

### <a name="read-messages-persisted-in-azure-storage"></a>Azure Storage에 유지되는 메시지 읽기
장치-클라우드 메시지가 Azure Storage에 기록될 때 해당 메시지를 모니터링합니다.

*예상 완료 시간: 5분*  

[Azure Storage에 유지되는 메시지 읽기][read-messages-persisted-in-azure-storage]로 이동

## <a name="lesson-4-send-cloud-to-device-messages"></a>단원 4: 클라우드-장치 메시지 보내기
![단원 4 종단간 다이어그램][lesson-4-end-to-end-diagram]

이 단원은 Azure IoT Hub에서 Arduino 보드로 메시지를 보내는 방법을 보여 줍니다. 이 메시지는 GPIO #13 온보드 LED의 켜기 및 끄기 동작을 제어합니다. 이 작업을 수행하기 위한 샘플 응용 프로그램이 준비되었습니다.

이 단원을 시작하기 전에 단원 1, 단원 2, 단원 3을 완료합니다.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>샘플 응용 프로그램을 실행하여 클라우드-장치 메시지 받기
단원 4의 샘플 응용 프로그램은 Arduino 보드에서 실행되며 IoT Hub에서 들어오는 메시지를 모니터링합니다. 새로운 gulp 작업은 IoT Hub에서 Arduino 보드로 메시지를 보내고 LED를 깜빡입니다.

*예상 완료 시간: 10분*  

[샘플 응용 프로그램을 실행하여 클라우드-장치 메시지 받기][receive-cloud-to-device-messages]로 이동

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>선택적인 섹션: LED 켜기 및 끄기 동작 변경
LED의 켜기 및 끄기 동작을 변경하도록 메시지를 사용자 지정합니다.

*예상 완료 시간: 10분*  

[선택적인 섹션: LED 켜기 및 끄기 동작 변경][change-the-on-and-off-behavior-of-the-led]으로 이동

## <a name="troubleshooting"></a>문제 해결
단원을 진행하는 동안 문제가 발생하면 [문제 해결][troubleshooting] 문서에서 솔루션을 찾습니다.

<!-- Images and links -->

[Lesson-1-end-to-end-diagram]: media/iot-hub-adafruit-feather-m0-wifi-lessons/e2e-lesson1.png
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[create-and-deploy-the-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[lesson-2-end-to-end-diagram]: media/iot-hub-adafruit-feather-m0-wifi-lessons/e2e-lesson2.png
[get-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[lesson-3-end-to-end-diagram]: media/iot-hub-adafruit-feather-m0-wifi-lessons/e2e-lesson3.png
[create-an-azure-function-app-and-azure-storage-account]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md
[lesson-4-end-to-end-diagram]: media/iot-hub-adafruit-feather-m0-wifi-lessons/e2e-lesson4.png
[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[change-the-on-and-off-behavior-of-the-led]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-change-led-behavior.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md


<!--HONumber=Dec16_HO2-->


