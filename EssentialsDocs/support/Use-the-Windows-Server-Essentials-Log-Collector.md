---
title: Use o coletor de Log do Windows Server Essentials
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
ms.openlocfilehash: d003c6a45159548f7e34d86ca242f74868659d2f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="use-the-windows-server-essentials-log-collector"></a>Use o coletor de Log do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando você estiver solucionando problemas do computador, um representante do atendimento ao cliente Microsoft e suporte solicitaremos que você coletar logs de servidores, computadores na rede, ou ambos, usando o coletor de Log do Windows Server Essentials.  
  
 O coletor de Log copia logs do programa, logs de eventos do revisor e informações sobre o ambiente relacionados em um arquivo zip único em um local especificado. Você pode executar o coletor de Log diretamente do servidor ou em qualquer computador na rede ou usando uma conexão remota para os computadores.  
  
> [!NOTE]
>  -   O coletor de Log não analisar problemas de rede ou fazer alterações em qualquer servidor ou computador na rede. Para obter informações sobre como solucionar problemas de rede, consulte a documentação de ajuda para seu produto de servidor.  
> -   Neste guia, os computadores em sua rede, que não seja o servidor, são chamados de computadores da rede.  
> -   [Baixe o pacote de instalação do Windows Server Essentials Log coletor](https://go.microsoft.com/fwlink/?LinkID=266341).  
  
 Para instalar e executar o coletor de Log, siga as etapas nos tópicos a seguir:  
  

1.  [Instalar o coletor de Log](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Execute o coletor de Log](Run-the-Windows-Server-Essentials-Log-Collector.md)  

1.  [Instalar o coletor de Log](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Execute o coletor de Log](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  

  
## <a name="environment-information-collected"></a>Informações sobre o ambiente coletado  
 Para cada computador da rede ou o servidor que você especificar, o coletor de Log reúne as seguintes informações de ambiente e o coloca no arquivo de coleção de log.  
  
-   Versão do sistema operacional  
  
-   Descrição e o fabricante da CPU  
  
-   Alocação e a quantidade de memória  
  
-   Adaptadores de rede que esteja vinculados ao TCP/IP  
  
-   Localidade  
  
-   Processos  
  
-   Configuração de armazenamento  
  
-   Informações de arquivo do host  
  
-   Logs de eventos, incluindo o aplicativo, sistema, Windows Server e Media Center  
  
-   Mensagens do Gerenciador de controle de serviço  
  
-   Reinicialize eventos e eventos do Windows Update  
  
-   Erros do sistema e erros de aplicativo  
  
## <a name="services-information-collected"></a>Serviços de informações coletadas  
  
### <a name="server-services"></a>Serviços de servidor  
  
-   Serviço de Backup do Windows Server cliente computador  
  
-   Serviço do provedor de Backup do Windows Server cliente computador  
  
-   Provedor de dispositivos do Windows Server  
  
-   Gerenciamento de nome de domínio do Windows Server  
  
-   Registro de provedor de serviço do Windows Server  
  
-   Provedor de configurações do Windows Server  
  
-   Serviço de dispositivo UPnP do Windows Server  
  
-   Provedor de administração de acesso do Windows servidor Web remoto  
  
-   Serviço de integridade do Windows Server  
  
-   Serviço de armazenamento do Windows Server  
  
-   Serviço do Windows Server SQM  
  
### <a name="network-computer-services"></a>Serviços de computador da rede  
  
-   Serviço do provedor de Backup do Windows Server cliente computador  
  
-   Serviço de integridade do Windows Server  
  
-   Registro de provedor de serviço do Windows Server  
  
-   Serviço do Windows Server SQM  
  
## <a name="logs-and-registry-information-collected"></a>Logs e informações de registro coletadas  
 Para cada computador da rede ou o servidor especificado, o coletor de Log reúne informações de log e registro do servidor e o computador da rede, da seguinte maneira.  
  
### <a name="server-logs-and-registry-information"></a>Logs de servidor e informações de registro  
  
-   Logs de produto de servidor, de < ProgramData\ > \Microsoft\Windows Server\Logs  
  
-   Tarefas agendadas  
  
-   Logs de instalação API  
  
-   Logs do Windows Update  
  
-   Arquivo de alertas de integridade  
  
-   Arquivo de informações de dispositivos  
  
-   Arquivo de Log de Backup do servidor  
  
-   Arquivo de Log Panther  
  
-   Serviços  
  
-   Chaves do registro, de  
  
    -   \\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>Logs de computador da rede e informações de registro  
  
-   Logs de produto do computador da rede em < ProgramData\ > \Microsoft\Windows Server\Logs  
  
-   Arquivo de alertas de integridade em < ProgramData\ > \Microsoft\Windows Server\Data  
  
-   Logs do Windows Update  
  
-   Logs de instalação API  
  
-   Informações de tarefas agendadas  
  
-   Chaves do registro de \\\HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server\  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>Logs para computadores que não executam uma versão do sistema operacional Windows  
 O coletor de Log não coletar os arquivos de log de computadores que não executam uma versão do sistema operacional Windows. Para computadores sem Windows, copie manualmente os seguintes arquivos de log no mesmo local onde você estiver armazenando os arquivos de Log coletor.  
  
-   System.log  
  
-   Logs/biblioteca/Windows Server  
  
-   Biblioteca/Logs/CrashReporter/barra inicial-< nnn\ > (copiar todos os arquivos na barra inicial-< nnn\ > .crash)  
  
-   Biblioteca/Logs/DiagnosticReports/barra inicial-< nnn\ > (copiar todos os arquivos na barra inicial-< nnn\ > .crash)  
  
## <a name="see-also"></a>Consulte também  
  

-   [Solucionar erros de coletor de Log](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [Solucionar erros de coletor de Log](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

