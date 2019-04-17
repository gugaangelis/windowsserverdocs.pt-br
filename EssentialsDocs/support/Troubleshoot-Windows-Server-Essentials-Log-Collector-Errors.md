---
title: Solucionar erros de coletor de Log do Windows Server Essentials
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Solucionar erros de coletor de Log do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando você executa o coletor de Log, você pode encontrar um dos erros a seguir. Para resolver um problema, siga as orientações fornecidas para o erro associadas.  
  
> [!NOTE]
>  Neste documento, os computadores em sua rede, que não seja o servidor, são chamados de computadores de rede.?  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a>A pasta de destino não é válida  
 **Causa:** a pasta onde você está tentando copiar os arquivos de log pode não existir ou talvez não tenha espaço suficiente para conter os arquivos.  
  
 **Solução:** Verifique se a pasta selecionada existe e que há espaço livre suficiente no disco para os arquivos. Você também deve garantir que haja espaço livre suficiente restante na pasta temporária na unidade.  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a>Ocorreu um erro de rede  
 **Causa:** pode haver um problema relacionado à rede no computador de rede ou no servidor.  
  
 **Solução:** Certifique-se de que todos os computadores e dispositivos em rede estão ligados e que ele está conectado à rede. Se você não puder resolver o problema, contate a pessoa que mantém sua rede para obter assistência.  
  
###  <a name="BKMK_CannotCollectLogFiles"></a>Não é possível coletar arquivos de log para o computador  
 **Causa:** o coletor de Log não pode ser instalado no computador porque o computador não se conectar com êxito ao servidor usando o computador se conectar ao Assistente de servidor.  
  
 **Solução:** para obter informações sobre como resolver problemas referentes a conexões ao seu servidor, consulte [solucionar conectar computadores para o servidor](https://go.microsoft.com/fwlink/p/?LinkID=241492)? (https://go.microsoft.com/fwlink/p/?LinkID=241492) no site da Microsoft.  
  
 Se você ainda não conseguir conectar o computador para o servidor, em seguida, você pode copiar os arquivos de log manualmente para uma unidade flash USB da seguinte maneira:  
  
-   Para computadores cliente que executam o Windows 7, Windows 8 ou Windows Multipoint Server, você pode copiar o **Logs** pasta localizado em **%sysdir%\programdata\Microsoft\Windows servidor**.  
  
###  <a name="BKMK_YouDoNotHavePermission"></a>Você não tem permissão para salvar os arquivos de log para a pasta selecionada  
 **Causa:** talvez você não tenha permissão de gravação para a pasta que você selecionou para salvar os arquivos de log.  
  
 **Solução:** se você estiver usando o caminho padrão para salvar arquivos de log, certifique-se de que você tenha permissões de gravação para a pasta compartilhada **\ \ \ < ServerName\ > \Logs**. Se você estiver armazenando logs em um computador da rede, certifique-se de que você tenha permissões de gravação para a pasta que você selecionou para salvar os arquivos de log.  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a>O computador não estiver configurado corretamente para coletar os arquivos de log  
 **Causa:** o computador não tiver sido configurado corretamente para o coletor de Log.  
  
 **Solução:** instalar novamente o coletor de Log. Consulte [reinstalando o coletor de Log](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall).  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a>Ocorreu um erro desconhecido  
 **Causa:** desconhecido.  
  
 **Solução 1:** execute novamente o coletor de Log. Se o erro ocorrer novamente, certifique-se de que há nenhum problema de conectividade. Você também pode tentar reinstalar o coletor de Log. Consulte [reinstalando o coletor de Log](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Se você não puder resolver o problema, contate a pessoa que mantém sua rede para obter assistência.  
  
 **Solução 2:** tentar primeiro, abra a pasta onde os arquivos de log são salvos. Se o arquivo zip com o nome do computador já foi gerado, ignorar esse erro e use os arquivos de log em vez disso. Se não houver nenhum arquivo de log gerado, execute novamente o coletor de Log. Se o erro ocorrer novamente, certifique-se de que há nenhum problema de conectividade. Você também pode tentar reinstalar o coletor de Log. Consulte [reinstalando o coletor de Log](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Se você não puder resolver o problema, contate a pessoa que mantém sua rede para obter assistência.