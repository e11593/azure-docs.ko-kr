---
title: "Windows 가상 컴퓨터 다시 배포 | Microsoft Docs"
description: "RDP 연결 문제를 완화하도록 Windows 가상 컴퓨터를 다시 배포하는 방법을 설명합니다."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/19/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 61dbf8fb96233d2e0f592deacf139a83cee51954


---
# <a name="redeploy-virtual-machine-to-new-azure-node"></a>새 Azure 노드로 가상 컴퓨터 다시 배포
RDP(원격 데스크톱) 연결 문제 또는 Windows 기반 Azure VM(가상 컴퓨터)에 대한 응용 프로그램 액세스 문제를 해결하는 데 어려움이 있는 경우 VM을 다시 배포하는 것이 도움이 될 수 있습니다. VM을 다시 배포하면 VM이 Azure 인프라 내의 새 노드로 이동된 다음 전원이 켜지고, 모든 구성 옵션 및 관련 리소스는 그대로 유지됩니다. 이 문서에서는 Azure PowerShell 또는 Azure 포털을 사용하여 VM을 다시 배포하는 방법을 보여 줍니다.

> [!NOTE]
> VM을 다시 배포한 후에 임시 디스크가 손실되고 가상 네트워크 인터페이스와 연결된 동적 IP 주소가 업데이트됩니다. 
> 
> 

## <a name="using-azure-powershell"></a>Azure PowerShell 사용
최신 Azure PowerShell 1.x를 컴퓨터에 설치했는지 확인합니다. 자세한 내용은 [Azure PowerShell 설치 및 구성하는 방법](../powershell-install-configure.md)을 참조하세요.

가상 컴퓨터를 다시 배포하려면 이 Azure PowerShell 명령을 사용합니다.

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>다음 단계
VM에 연결하는 데 문제가 있는 경우 [RDP 연결 문제 해결](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 또는 [자세한 RDP 문제 해결 단계](virtual-machines-windows-detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)에서 특정 도움말을 찾을 수 있습니다. VM에서 실행 중인 응용 프로그램에 액세스할 수 없는 경우 [응용 프로그램 문제 해결](virtual-machines-windows-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)을 읽어볼 수도 있습니다.




<!--HONumber=Nov16_HO3-->


