---
title: Desenvolver para o Nano Server
description: Comunicação remota do PowerShell e sessões CIM
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 57079470-a1c1-4fdc-af15-1950d3381860
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 8d793dde9c41bc99b55eeb0da3a5ee4b025f08d6
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443643"
---
# <a name="developing-for-nano-server"></a>Desenvolver para o Nano Server

>Aplica-se a: Windows Server 2016

> [!IMPORTANT]
> A partir do Windows Server, versão 1709, o Nano Server estará disponível somente como uma [imagem do sistema operacional de contêiner base](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Confira [Mudanças no Nano Server](nano-in-semi-annual-channel.md) para saber o que isso significa. 

Estes tópicos explicam as diferenças importantes no PowerShell no Nano Server e fornecem orientações para desenvolver seus próprios cmdlets do PowerShell para uso com o Nano Server.

- [PowerShell no Nano Server](PowerShell-on-Nano-Server.md)
- [Desenvolver cmdlets do PowerShell para Nano Server](Developing-PowerShell-Cmdlets-for-Nano-Server.md)

## <a name="using-windows-powershell-remoting"></a>Usar a comunicação remota do Windows PowerShell  
Para gerenciar o Nano Server com a comunicação remota do Windows PowerShell, é necessário adicionar o endereço IP do Nano Server à lista de hosts confiáveis do seu computador de gerenciamento, adicionar a conta que você está usando para os administradores do Nano Server e ativar o CredSSP se quiser usar esse recurso.  

> [!NOTE]
> Se o Nano Server de destino e o computador de gerenciamento estiverem na mesma floresta de AD DS (ou em florestas com uma relação de confiança), você não deverá adicionar o Nano Server à lista de hosts confiáveis. É possível conectar ao Nano Server usando o nome de domínio totalmente qualificado, por exemplo: PS C:\> Enter-PSSession -ComputerName nanoserver.contoso.com -Credential (Get-Credential)
  
  
Para adicionar o Nano Server à lista de hosts confiáveis, execute este comando em um prompt com privilégios elevados do Windows PowerShell:  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
Para iniciar a sessão remota do Windows PowerShell, inicie uma sessão local do Windows PowerShell com privilégios elevados e execute estes comandos:  
  
  
```  
$ip = "\<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
Agora você pode executar comandos do Windows PowerShell no Nano Serve normalmente.  
  
> [!NOTE]  
> Nem todos os comandos do Windows PowerShell estão disponíveis nesta versão do Nano Server. Para ver quais estão disponíveis, execute `Get-Command -CommandType Cmdlet`  
  
Parar a sessão remota com o comando `Exit-PSSession`  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>Usar sessões CIM do Windows PowerShell no WinRM  
Você pode usar sessões e instâncias CIM no Windows PowerShell para executar comandos WMI no Windows Remote Management (WinRM).  
  
Inicie a sessão CIM executando estes comandos em um prompt do Windows PowerShell:  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
Com a sessão estabelecida, você pode executar vários comandos WMI, por exemplo:  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  