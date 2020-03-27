---
title: Solução de problemas gerais
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 513bcae13d4a8f3ab935d2bda77745baa1788fa9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313790"
---
# <a name="troubleshooting-general-issues"></a>Solução de problemas gerais

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico contém informações de solução de problemas gerais relacionadas ao acesso remoto.  
  
## <a name="gpo-retrieval-error"></a>Erro de recuperação de GPO  
**Erro recebido**. As configurações de GPO do servidor DirectAccess não podem ser recuperadas. Verifique se você tem permissões de edição para o GPO.  
  
O console de gerenciamento de acesso remoto não está respondendo depois de receber esse erro.  
  
**Causa**  
  
O DirectAccess não pode acessar o GPO de um dos pontos de entrada na implantação e, como resultado, a configuração não é carregada.  
  
**Solução**  
  
Certifique-se de que cada ponto de entrada na implantação tenha um GPO correspondente em seu controlador de domínio e verifique se o usuário conectado tem permissões de leitura e gravação para todos os GPOs configurados na implantação de acesso remoto.  
  
Como alternativa, use os cmdlets de configuração em vez de usar o console de gerenciamento de acesso remoto; por exemplo, usando `Get-RemoteAccess` e `Get-DAEntryPoint`.  
  
> [!NOTE]  
> Esse cenário não ocorre quando o GPO do servidor do ponto de entrada atual não está disponível.  
  
Você pode usar o cmdlet `Get-DAEntryPointDC` para listar todos os controladores de domínio que armazenam GPOs do servidor e `Get-DAMultiSite` em conjunto com `Get-RemoteAccess` para recuperar uma lista completa dos GPOs do servidor na implantação. Por exemplo:  
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Atualização de cliente do Windows 7 para o Windows 8 ou 10  
**Sintoma**. Depois que um cliente do Windows 7 é atualizado para o Windows 10 ou o Windows 8 em uma implantação multissite, a conexão do DirectAccess não fica visível na lista redes.  
  
**Causa**  
  
Os GPOs do Windows 7 em uma implantação multissite não contêm a configuração do assistente de conectividade de rede do Windows 8.  
  
 Os clientes do Windows 7 devem usar o assistente de conectividade do DirectAccess para monitorar o status de conectividade do DirectAccess, que requer uma configuração manual separada nos GPOs do cliente do Windows 7. Quando os clientes do Windows 7 forem atualizados para o Windows 10 ou o Windows 8, o assistente de conectividade de rede não funcionará se o GPO do cliente do Windows 7 ainda estiver aplicado.  
  
**Solução**  
  
Se as configurações do assistente de conectividade do DirectAccess estiverem definidas nos GPOs do Windows 7, você poderá resolver esse problema antes de atualizar os computadores cliente, modificando os GPOs do Windows 7 usando os seguintes cmdlets do PowerShell:  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
Se um cliente já tiver sido atualizado ou o DCA não estiver configurado, mova o computador cliente para o grupo de segurança Windows 10 ou Windows 8.  
  
## <a name="general-cmdlet-errors"></a>Erros gerais de cmdlet  
  
-   **Problema 1**  
  
    **Erro recebido**. O controlador de domínio < domain_controller > não pode ser acessado para < server_name ou entry_point_name >.  
  
    **Causa**  
  
    Para manter a consistência da configuração em uma implantação multissite, é importante assegurar que cada GPO seja gerenciado por um único controlador de domínio. Quando o controlador de domínio que gerencia o GPO do servidor de um ponto de entrada não está disponível, as definições de configuração de acesso remoto não podem ser lidas ou modificadas.  
  
    **Solução**  
  
    Siga o procedimento "para alterar o controlador de domínio que gerencia GPOs de servidor", descrito em [2,4. Configurar GPOs](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Problema 2**  
  
    **Erro recebido**. O controlador de domínio primário no domínio < domain_name > não pode ser acessado.  
  
    **Causa**  
  
    Para manter a consistência da configuração em uma implantação multissite, é importante assegurar que cada GPO seja gerenciado por um único controlador de domínio. Os GPOs de cliente são gerenciados no controlador de domínio primário. Se esse controlador não estiver disponível, não será possível ler nem modificar as configurações do Acesso Remoto.  
  
    **Solução**  
  
    Siga o procedimento "para transferir a função de emulador de PDC", descrito em [2,4. Configurar GPOs](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  


