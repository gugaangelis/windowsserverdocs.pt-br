---
title: Usar o coletor de log do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c6985518-b42d-4cfb-9761-eaa75306b6d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3bc43b08df30d03f29d9f343b7d6ed4d63c85eda
ms.sourcegitcommit: 39244de670f712857a5fdd56630e95d57b7001a5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74897667"
---
# <a name="use-the-windows-server-essentials-log-collector"></a>Usar o coletor de log do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando você estiver solucionando problemas do computador, um representante do suporte e atendimento ao cliente da Microsoft poderá solicitar que você colete logs de servidores, computadores na rede ou ambos usando o coletor de logs do Windows Server Essentials.  
  
 O coletor de log copia logs de programas, logs do revisor de eventos e informações relacionadas ao ambiente em um único arquivo zip em um local especificado. Você pode executar o coletor de log diretamente do servidor ou de qualquer computador na rede ou usando uma conexão remota para os computadores.  
  
> [!NOTE]
>O coletor de log não analisa problemas de rede ou faz alterações em qualquer servidor ou computador na rede. Para obter informações sobre como solucionar problemas de rede, consulte a documentação de ajuda para o seu produto de servidor.  
>Neste guia, os computadores em sua rede, além do servidor, são chamados de computadores de rede.  
>
>[Baixe o pacote de instalação do coletor de logs do Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=34821).  
  
 Para instalar e executar o coletor de log, execute as etapas nos tópicos a seguir:  
  

1. [Instalar o coletor de logs](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2. [Executar o coletor de logs](Run-the-Windows-Server-Essentials-Log-Collector.md)  

3. [Instalar o coletor de logs](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
4. [Executar o coletor de logs](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  


## <a name="environment-information-collected"></a>Informações sobre o ambiente coletadas  
 Para cada computador da rede ou o servidor que você especificar, o coletor de log reúne as seguintes informações de ambiente e as coloca no arquivo de coleta de log.  
  
-   Versão do sistema operacional  
  
-   Descrição e fabricante da CPU  
  
-   Alocação e quantidade de memória  
  
-   Adaptadores de rede que estão vinculados ao TCP/IP  
  
-   Locale  
  
-   Processos  
  
-   Configuração de armazenamento  
  
-   Informações de arquivo do host  
  
-   Logs de eventos, inclusive o aplicativo, o sistema, o Windows Server e o Media Center  
  
-   Mensagens do Gerenciador de Controle de Serviço  
  
-   Reiniciar os eventos e os eventos do Windows Update  
  
-   Erros de sistema e erros de aplicativo  
  
## <a name="services-information-collected"></a>Informações coletadas de serviços  
  
### <a name="server-services"></a>Serviços de servidor  
  
-   Serviço de backup do computador cliente do Windows Server  
  
-   Serviço de provedor de backup do computador cliente do Windows Server  
  
-   Provedor de dispositivos do Windows Server  
  
-   Gerenciamento de nome de domínio do Windows Server  
  
-   Registro de provedor de serviço do Windows Server  
  
-   Provedor de configurações do Windows Server  
  
-   Serviço de dispositivo UPnP do Windows Server  
  
-   Provedor de administração de Acesso Remoto via Web do Windows Server  
  
-   Serviço de integridade do Windows Server  
  
-   Serviço de armazenamento do Windows Server  
  
-   Serviço SQM do Windows Server  
  
### <a name="network-computer-services"></a>Serviços de computador da rede  
  
-   Serviço de provedor de backup do computador cliente do Windows Server  
  
-   Serviço de integridade do Windows Server  
  
-   Registro de provedor de serviço do Windows Server  
  
-   Serviço SQM do Windows Server  
  
## <a name="logs-and-registry-information-collected"></a>Informações do Registro e de logs coletadas  
 Para cada computador de rede ou servidor especificado, o coletor de log coleta informações de log e do Registro do servidor e do computador de rede, da seguinte maneira.  
  
### <a name="server-logs-and-registry-information"></a>Logs do servidor e informações do Registro  
  
-   Logs de produto do servidor, de < ProgramData\>\Microsoft\Windows Server\Logs  
  
-   Tarefas Agendadas  
  
-   Instalação dos logs de API  
  
-   Logs do Windows Update  
  
-   Arquivo de alertas de integridade  
  
-   Arquivo de informações de dispositivos  
  
-   Arquivo de log de backup do servidor  
  
-   Arquivo de Log Panther  
  
-   Serviços  
  
-   Chaves do Registro, de  
  
    -   \\\ HKEY_LOCAL_MACHINE servidor \SOFTWARE\Microsoft\Windows \  
  
    -   \\\ HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\ HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>Logs de computador da rede e informações do Registro  
  
-   Logs de produto do computador de rede em < ProgramData\>\Microsoft\Windows Server\Logs  
  
-   Arquivo de alertas de integridade em < ProgramData\>\Microsoft\Windows Server\Data  
  
-   Logs do Windows Update  
  
-   Instalação dos logs de API  
  
-   Informações de tarefas agendadas  
  
-   Chaves do registro de \\\ HKEY_LOCAL_MACHINE servidor \SOFTWARE\Microsoft\Windows \  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>Logs para computadores que não executam uma versão do sistema operacional Windows  
 O coletor de Log não coleta arquivos de log de computadores que não executam uma versão do sistema operacional Windows. Para computadores que não possuem o Windows, copie manualmente os seguintes arquivos de log para o mesmo local em que você está armazenando os arquivos do coletor de log.  
  
-   System.log  
  
-   Library/Logs/Windows Server.log  
  
-   Biblioteca/logs/CrashReporter/LaunchPad-< nnn\> (Copie todo o LaunchPad-< nnn\>arquivos. Crash)  
  
-   Biblioteca/logs/DiagnosticReports/LaunchPad-< nnn\> (Copie todo o LaunchPad-< nnn\>arquivos. Crash)  
  
## <a name="see-also"></a>Consulte também  
  

-   [Solucionar erros do coletor de logs](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [Solucionar erros do coletor de logs](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

