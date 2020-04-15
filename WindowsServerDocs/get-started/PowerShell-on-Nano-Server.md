---
title: PowerShell do Nano Server
description: Diferenças no conjunto reduzido de recursos do PowerShell no Nano Server
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.topic: article
ms.assetid: 9b25b939-1e2c-4bed-a8d3-2a8e8e46b53d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 4879ae58c24596d64d24b6bece54d4c35837f00f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826759"
---
# <a name="powershell-on-nano-server"></a>PowerShell do Nano Server

> Aplica-se a: Windows Server 2016

> [!IMPORTANT]
> A partir do Windows Server, versão 1709, o Nano Server estará disponível somente como uma [imagem do sistema operacional de base de contêiner](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Confira [Mudanças no Nano Server](nano-in-semi-annual-channel.md) para saber o que isso significa.

## <a name="powershell-editions"></a>Edições do PowerShell

A partir da versão 5.1, o PowerShell está disponível em diferentes edições que denotam os vários conjuntos de recursos e compatibilidades de plataforma.

- **Desktop Edition:** Criado no .NET Framework; fornece compatibilidade com scripts e módulos destinados a versões do PowerShell em execução em edições de volume completo do Windows, como Server Core e Windows Desktop.
- **Core Edition:** Criado no .NET Core; fornece compatibilidade com scripts e módulos destinados a versões do PowerShell em execução em edições de volume reduzido do Windows, como Nano Server e Windows IoT.

A edição do PowerShell em execução é exibida na propriedade PSEdition de $PSVersionTable.
```powershell
$PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.14300.1000
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
CLRVersion                     4.0.30319.42000
BuildVersion                   10.0.14300.1000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Os autores de módulo podem declarar seus módulos como compatíveis com uma ou mais edições do PowerShell usando a chave de manifesto do módulo CompatiblePSEditions. Essa chave só é compatível com o PowerShell 5.1 ou posterior.
```powershell
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1
$moduleInfo = Test-ModuleManifest -Path \TestModuleWithEdition.psd1
$moduleInfo.CompatiblePSEditions
Desktop
Core

$moduleInfo | Get-Member CompatiblePSEditions

   TypeName: System.Management.Automation.PSModuleInfo

Name                 MemberType Definition
----                 ---------- ----------
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}

```
Ao obter uma lista de módulos disponíveis, é possível filtrá-la por edição do PowerShell.
```powershell
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Desktop

    Directory: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   1.0        ModuleWithPSEditions

Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Core | % CompatiblePSEditions
Desktop
Core

```
Os autores de script podem impedir que um script seja executado, a menos que seja executado em uma edição compatível do PowerShell, usando o parâmetro PSEdition em uma instrução #requires.
```powershell
Set-Content C:\script.ps1 -Value #requires -PSEdition Core
Get-Process -Name PowerShell
Get-Content C:\script.ps1
#requires -PSEdition Core
Get-Process -Name PowerShell

C:\script.ps1
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a #requires statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.
At line:1 char:1
+ C:\script.ps1
+ ~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition
```

## <a name="differences-in-powershell-on-nano-server"></a>Diferenças do PowerShell no Nano Server
O Nano Server inclui o PowerShell Core por padrão em todas as instalações do Nano Server. O PowerShell Core é uma edição de volume reduzido do PowerShell baseada no .NET Core e executada em edições de volume reduzido do Windows, como Nano Server e Windows IoT Core. O PowerShell Core funciona da mesma maneira que outras edições do PowerShell, como o Windows PowerShell em execução no Windows Server 2016. No entanto, o volume reduzido do Nano Server significa que nem todos os recursos do PowerShell do Windows Server 2016 estão disponíveis no PowerShell Core no Nano Server.


**Recursos do Windows PowerShell não disponíveis no Nano Server**
* Adaptadores de tipo ADSI, ADO e o WMI
* Enable-PSRemoting, Disable-PSRemoting (a comunicação remota do PowerShell é habilitada por padrão; confira a seção Usar a comunicação remota do Windows PowerShell de [Instalar o Nano Server](Getting-Started-with-Nano-Server.md)).
* Trabalhos agendados e módulo PSScheduledJob
* Cmdlets de computador para ingressar em um domínio { Add | Remove } (para ver métodos diferentes de ingressar em um domínio do Nano Server, confira a seção Ingressar o Nano Server em um domínio em [Instalar o Nano Server](Getting-Started-with-Nano-Server.md)).
* Reset-ComputerMachinePassword, Test-ComputerSecureChannel
* Perfis (é possível adicionar um script de inicialização para conexões de entrada remotas com o `Set-PSSessionConfiguration`)
* Área de transferência de Cmdlets
* Cmdlets de EventLog { Clear | Get | Limit | New | Remove | Show | Write } (use os cmdlets New-WinEvent e Get-WinEvent ao invés disso).
* Cmdlet Get-PfxCertificate
* Cmdlets TraceSource { Get | Set }
* Cmdlets de contador { Get | Export | Import }
* Alguns cmdlets relacionados à Web { New-WebServiceProxy, Send-MailMessage, ConvertTo-Html }
* Registro em log e rastreamento usando o módulo PSDiagnostics
* Get-HotFix (para obter e gerenciar atualizações no Nano Server, confira [Gerenciar o Nano Server](Manage-Nano-Server.md)).
* Cmdlets de comunicação remota implícitos { Export-PSSession | Import-PSSession }
* New-PSTransportOption
* Cmdlets de transação e transações do PowerShell { Complete | Get | Start | Undo | Use }
* Cmdlets, módulos e infraestrutura do fluxo de trabalho do PowerShell
* Out-Printer
* Update-List
* Cmdlets do WMI v1: Get-WmiObject, Invoke-WmiMethod, Register-WmiEvent, Remove-WmiObject, Set-WmiInstance (use o módulo CimCmdlets ao invés disso)

## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>Usar a configuração de estado desejado do Windows PowerShell com o Nano Server

É possível gerenciar o Nano Server como nós de destino com a Configuração de Estado Desejado (DSC) do Windows PowerShell. No momento, só é possível gerenciar nós que estejam executando o Nano Server com DSC no modo push. Nem todos os recursos de DSC funcionam com o Nano Server.

Para obter mais detalhes, confira [Usar DSC no Nano Server](https://docs.microsoft.com/powershell/scripting/dsc/getting-started/nanodsc).

