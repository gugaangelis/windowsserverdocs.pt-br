---
title: Solução de problemas do DirectAccess
description: Este tópico fornece informações sobre como solucionar problemas de implantações do DirectAccess no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61040e19-5960-4eb0-b612-d710627988f7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ec725eea286c359461b0f4a7b8763b97464e7067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867087"
---
# <a name="troubleshooting-directaccess"></a>Solução de problemas do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Siga estas etapas para solucionar problemas de acesso remoto (DirectAccess).  
  
|||  
|-|-|  
|**Problema**|**Resolução**|  
|Console de gerenciamento de acesso remoto é não é possível mostrar a configuração do DirectAccess|**Para restaurar as informações de configuração ausente**<br />– Se você estiver solucionando problemas de uma implantação multissite, certifique-se de que o controlador de domínio mais próximo ao ponto de entrada está disponível.<br />– Use o **Get-DAEntrypointDC** cmdlet para recuperar o nome do controlador de domínio mais próximo ao ponto de entrada. Se o controlador de domínio não está em execução, use o **Set-DAEntryPointDC** cmdlet para apontar para outro controlador de domínio.<br />-Execute **gpresult** em um prompt de comando elevado no servidor para garantir que o servidor está obtendo os objetos de diretiva de grupo do DirectAccess.<br />-Habilite o log de (UI) de interface do usuário.<br />-Use o seguinte comando para iniciar o registro em log do Windows PowerShell:<pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets <br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre><repro>-Feche e reabra a interface do usuário.<br />-Desabilite o registro em log do Windows Powershell. Colete os arquivos de Log de rastreamento de eventos. Além disso, coletar todos os logs do **%windir%/tracing** pasta.|  
|Aplicando a configuração do DirectAccess falhar|**Para atualizar a configuração do DirectAccess**<br />– Se você estiver solucionando problemas de uma implantação multissite, certifique-se de que o controlador de domínio mais próximo ao ponto de entrada está disponível.<br />– Use o **Get-DAEntrypointDC** cmdlet para recuperar o nome do controlador de domínio mais próximo ao ponto de entrada. Se o controlador de domínio não está em execução, use o **Set-DAEntryPointDC** cmdlet para apontar para outro controlador de domínio.<br />-Use o seguinte comando para iniciar o registro em log do Windows Powershell:<br /><pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets<br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre>    <repro><br />-Clique em **aplicar**.<br />-Após a falha ocorrer, desabilite o log do Windows Powershell e coletar Log de rastreamento de eventos.|  
|O DirectAccess é configurado, mas os clientes não são capazes de se conectar aos recursos internos|**Solucionar problemas de conexão do cliente**<br />-Clique a **Status de operações** guia no console de gerenciamento de acesso remoto e certifique-se de que todos os componentes mostram um ícone verde. Caso contrário, verifique os detalhes do erro e siga as etapas de resolução.<br />-Execute o acesso remoto Server Best Practices Analyzer (BPA). Se não houver nenhum aviso ou erro, siga as etapas de resolução para resolver o problema.|  
|Encontrando problemas relacionados a uma configuração multissite (por exemplo, Habilitar pontos de entrada multissite, adição ou configuração do controlador de domínio para um ponto de entrada)|Siga as etapas em [solucionar problemas de uma implantação multissite](https://technet.microsoft.com/library/jj554657(v=ws.11).aspx).|  
|Bloco de status de configuração no painel mostra um aviso ou erro|Siga as etapas em [monitorar o status de distribuição de configuração de servidor de acesso remoto](https://technet.microsoft.com/library/jj574221(v=ws.11).aspx).|  
|Encontrando problemas relacionados à configuração (por exemplo, a configuração falha quando você habilitar o balanceamento de carga ou há problemas ao adicionar ou remover servidores de um cluster) de balanceamento de carga|Se você foram habilitando o balanceamento de carga ou adicionar um nó e a configuração atualizada quando você clicou **aplicar**, mas o cluster não forma corretamente no servidor, execute o seguinte comando: **cmd.exe /c "reg add HKLM\ SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v DebugFlag /t REG_DWORD /d "" 0xffffffff"" "** para coletar o usuário interface logs no novo servidor.|  
|Status de operações mostra um erro ou aviso depois de seguir as etapas para corrigir a situação|Se o status de operações está mostrando informações incorretas (como erros, mesmo após você corrigi-los):<br /><br />-   Enable the registry key **cmd.exe /c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v EnableTracing /t REG_DWORD /d ""5"" "**.<br />-Atualizar o status de operações e colete os logs em **%windir%/tracing**.|  
|Windows 8 e os computadores cliente DirectAccess posteriores relatam "Sem Internet" como o status para a conexão do DirectAccess e rede conectividade Status NCSI (indicador) relata conectividade limitada.|Isso pode ocorrer quando criar túneis à força é habilitada na configuração do DirectAccess e, por isso, apenas IPHTTPS está sendo usado. Para resolver esse problema, você pode criar e configurar um servidor proxy. NCSI, em seguida, usa o servidor proxy para executar verificações de conectividade de Internet. É recomendável adicionar um proxy estático para o nome de resolução de política NRPT (tabela), usando o procedimento a seguir.<br /><br />Antes de executar os comandos neste procedimento, certifique-se de substituir todos os nomes de domínio, nomes de computador e outras variáveis de comando do Windows PowerShell com os valores que são apropriadas para sua implantação.<br /><br />**Configurar um proxy estático para uma regra da NRPT**<br />1.  Exibição de "." Regra da NRPT: `Get-DnsClientNrptRule -GpoName "corp.example.com\DirectAccess Client Settings" -Server <DomainControllerNetBIOSName>`<br />2.  Anote o nome (GUID) do "." Regra da NRPT. O nome (GUID) deve começar com **DA-{.}**<br />3.  Definir o proxy para o "." Regra da NRPT **proxy.corp.example.com:8080**:  `Set-DnsClientNrptRule -Name "DA-{..}" -Server <DomainControllerNetBIOSName> -GPOName "corp.example.com\DirectAccess Client Settings" -DAProxyServerName "proxy.corp.example.com:8080" -DAProxyType "UseProxyName"`<br />4.  Exibição de "." Regra da NRPT novamente executando `Get-DnsClientNrptRule`, verifique se **ProxyFQDN:port** agora está configurado corretamente.<br />5.  Atualizar a diretiva de grupo, executando `gpupdate /force` em um cliente DirectAccess quando o cliente é conectado internamente, em seguida, exibir usando a NRPT `Get-DnsClientNrptPolicy` e verifique se que o "." regra mostra **ProxyFQDN:port**.|  
  


