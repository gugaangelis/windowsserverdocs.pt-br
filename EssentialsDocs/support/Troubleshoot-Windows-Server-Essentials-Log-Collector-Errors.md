---
title: Solucionar problemas de erros de coletores de log do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa2e1685-31c0-4d4f-a10a-6c8885dfc493
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e92236c8e5d956b50f657ebcbe1a942b5841fded
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836657"
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Solucionar problemas de erros de coletores de log do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando executar o coletor de Log, você pode encontrar um dos seguintes erros. Para resolver um problema, siga as orientações fornecidas para o erro associado.  
  
> [!NOTE]
>  Neste documento, os computadores em sua rede, diferentes do servidor, são chamados de computadores da rede.?  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a> A pasta de destino não é válida  
 **Causa:** A pasta na qual você está tentando copiar os arquivos de log pode não existir ou pode não ter espaço suficiente para conter os arquivos.  
  
 **Solução:** Verifique se a pasta selecionada existe e se há espaço livre suficiente na unidade para os arquivos. Você também deve garantir que há espaço livre suficiente restante na pasta temporária na unidade.  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a> Ocorreu um erro de rede  
 **Causa:** Pode haver um problema de rede no computador da rede ou no servidor.  
  
 **Solução:** Certifique-se de que todos os computadores e dispositivos de rede estejam ligados e conectados à rede. Se não for possível resolver o problema, entre em contato com a pessoa que mantém a sua rede para obter assistência.  
  
###  <a name="BKMK_CannotCollectLogFiles"></a> Não é possível coletar arquivos de log para o computador  
 **Causa:** O coletor de Log não pode ser instalado no computador porque o computador não se pode se conectar com êxito ao servidor usando o assistente Conectar Computador ao Servidor.  
  
 **Solução:** Para obter informações sobre como resolver problemas sobre as conexões ao servidor, consulte [solucione o problema conectar computadores ao servidor](https://go.microsoft.com/fwlink/p/?LinkID=241492)? (https://go.microsoft.com/fwlink/p/?LinkID=241492) no site da Microsoft.  
  
 Se você ainda não conseguir conectar o computador ao servidor, você pode copiar os arquivos de log manualmente para uma unidade flash USB da seguinte maneira:  
  
-   Para computadores cliente que executam o Windows 7, Windows 8 ou Windows MultiPoint Server, você pode copiar a pasta **Logs** localizada em **%sysdir%\programdata\Microsoft\Windows Server**.  
  
###  <a name="BKMK_YouDoNotHavePermission"></a> Você não tem permissão para salvar os arquivos de log na pasta selecionada  
 **Causa:** Você não terá permissão de gravação para a pasta selecionada para salvar os arquivos de log.  
  
 **Solução:** Se você estiver usando o caminho padrão para salvar arquivos de log, certifique-se de que você tenha permissão de gravação para a pasta compartilhada  **\\ \\< ServerName\>\Logs**. Se estiver armazenando logs em um computador da rede, certifique-se de que você tenha permissão de gravação para a pasta que selecionada para salvar os arquivos de log.  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a> O computador não está configurado corretamente para coletar os arquivos de log  
 **Causa:** O computador não foi configurado corretamente para o coletor de log.  
  
 **Solução:** Reinstalar o coletor de log. Consulte [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall).  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a> Ocorreu um erro desconhecido  
 **Causa:** Desconhecida.  
  
 **Solução 1:** Execute novamente o coletor de log. Se o erro ocorrer novamente, verifique se não há problemas de conectividade. Você também pode tentar reinstalar o coletor de log. Consulte [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Se não for possível resolver o problema, entre em contato com a pessoa que mantém a sua rede para obter assistência.  
  
 **Solução 2:** Primeiro, tente abrir a pasta na qual os arquivos de log estão salvos. Se o arquivo zip com o nome de computador já tiver sido gerado, ignore este erro e use os arquivos de log. Se não houver nenhum arquivo de log gerado, execute novamente o coletor de log. Se o erro ocorrer novamente, verifique se não há problemas de conectividade. Você também pode tentar reinstalar o coletor de log. Consulte [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Se não for possível resolver o problema, entre em contato com a pessoa que mantém a sua rede para obter assistência.