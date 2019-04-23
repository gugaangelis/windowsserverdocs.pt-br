---
title: Solução de problemas gerais
description: Este tópico faz parte do guia de implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe1fdc4fb5aff2e34555b08d3b2c4347e643085e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831077"
---
# <a name="troubleshooting-general-issues"></a>Solução de problemas gerais

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico contém informações sobre solução de problemas gerais relacionados ao acesso remoto.  
  
## <a name="gpo-retrieval-error"></a>Erro na recuperação de GPO  
**Erro recebido**. Não não possível recuperar as configurações de GPO do servidor DirectAccess. Se que você tem permissões de edição para o GPO.  
  
O console de gerenciamento de acesso remoto não está respondendo depois de receber esse erro.  
  
**Causa**  
  
O DirectAccess não pode acessar o GPO de um dos pontos de entrada na implantação e assim a configuração falhará ao carregar.  
  
**Solução**  
  
Certifique-se de que cada ponto de entrada na implantação tem um GPO correspondente em seu controlador de domínio e verifique se que o usuário conectado tem permissões leitura e gravação para todos os GPOs configurados na implantação do acesso remoto.  
  
Como alternativa, use os cmdlets de configuração em vez de usar o console de gerenciamento de acesso remoto. Por exemplo, usando `Get-RemoteAccess` e `Get-DAEntryPoint`.  
  
> [!NOTE]  
> Esse cenário não ocorre quando o GPO do servidor do ponto de entrada atual não está disponível.  
  
Você pode usar o `Get-DAEntryPointDC` cmdlet para listar todos os controladores de domínio que armazenam os GPOs de servidor e `Get-DAMultiSite` em conjunto com `Get-RemoteAccess` para recuperar uma lista completa dos GPOs de servidor na implantação. Por exemplo:   
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Windows 7 para o Windows 8 ou 10 atualização de cliente  
**Sintoma**. Depois que um cliente do Windows 7 é atualizado para Windows 10 ou Windows 8 em uma implantação multissite, a conexão do DirectAccess não estiver visível na lista de redes.  
  
**Causa**  
  
Windows 7 GPOs em uma implantação multissite não contêm a configuração do Assistente de conectividade de rede do Windows 8.  
  
 Os clientes do Windows 7 devem usar o Assistente de conectividade do DirectAccess para monitorar seu status de conectividade do DirectAccess que requer uma configuração manual separada no GPOs de cliente do Windows 7. Quando os clientes do Windows 7 são atualizados para Windows 10 ou Windows 8, o Assistente de conectividade de rede não funcionará se o GPO do cliente Windows 7 ainda está aplicado.  
  
**Solução**  
  
Se o Assistente de conectividade do DirectAccess configurações nos GPOs Windows 7, você poderá resolver esse problema antes de atualizar computadores cliente, modificando os GPOs de 7 do Windows usando os seguintes cmdlets do PowerShell:  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
Se um cliente já foi atualizado ou não está configurado o DCA, mova o computador cliente para o grupo de segurança do Windows 10 ou Windows 8.  
  
## <a name="general-cmdlet-errors"></a>Erros gerais de cmdlet  
  
-   **Problema 1**  
  
    **Erro recebido**. O controlador de domínio < controlador_de_domínio > não pode ser alcançado para < nome_do_servidor ou nome_do_ponto_de_entrada >.  
  
    **Causa**  
  
    Para manter a consistência da configuração em uma implantação multissite, é importante assegurar que cada GPO seja gerenciado por um único controlador de domínio. Quando o controlador de domínio que gerencia o GPO do servidor de um ponto de entrada não estiver disponível, as definições de configuração de acesso remoto não podem ser lidas ou modificadas.  
  
    **Solução**  
  
    Siga o procedimento "para alterar o controlador de domínio que gerencia GPOs de servidor" descrito [2.4. Configurar GPOs](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Problema 2**  
  
    **Erro recebido**. O controlador de domínio primário no domínio < nome_do_domínio > não pode ser alcançado.  
  
    **Causa**  
  
    Para manter a consistência da configuração em uma implantação multissite, é importante assegurar que cada GPO seja gerenciado por um único controlador de domínio. Os GPOs de cliente são gerenciados no controlador de domínio primário. Se esse controlador não estiver disponível, não será possível ler nem modificar as configurações do Acesso Remoto.  
  
    **Solução**  
  
    Siga o procedimento para transferir a função de emulador do PDC", descrito na [2.4. Configurar GPOs](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  


