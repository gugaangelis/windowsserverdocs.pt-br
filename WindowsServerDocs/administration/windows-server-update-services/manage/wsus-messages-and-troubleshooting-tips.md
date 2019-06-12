---
title: Mensagens do WSUS e dicas de solução de problemas
description: Tópico do Windows Server Update Service (WSUS) - solução de problemas usando as mensagens do WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f6317f7-bfe0-42d9-87ce-d8f038c728ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4acc5e284d5ca7a62335a1c52f341cda3dfb547e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439735"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>Mensagens do WSUS e dicas de solução de problemas

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico contém informações sobre as seguintes mensagens WSUS:

-   "O computador não tenha comunicado o status"

-   "ID de mensagem 6703 - Falha na sincronização do WSUS"

-   "Erro 0x80070643: Erro fatal durante a instalação"

-   "Alguns serviços não estão executando. Verifique os seguintes serviços [...]"

## <a name="computer-has-not-reported-status"></a>Computador não tenha comunicado seu status
Essa mensagem é gerada no console do WSUS quando um computador de cliente do WSUS não envia informações ao servidor do WSUS para indicar o estado de atualização atual. Normalmente, esse problema é causado pelo computador cliente WSUS, não o servidor do WSUS.

Os motivos mais comuns são:

-   O computador perdeu a conectividade à rede:
    -   O cabo de rede está desconectado.
    -   Um cabo de rede intermediária está com defeito.
    -   O computador tem um adaptador de rede com defeito.
    -   A porta de rede que conecta o computador foi desabilitada.
    -   O adaptador sem fio não consegue associar e conectar-se ao ponto de acesso sem fio corporativa.
-   O computador é desligado. (Ele foi desligado ou está no modo de suspensão ou hibernação.)

## <a name="message-id-6703---wsus-synchronization-failed"></a>ID da mensagem 6703 - sincronização do WSUS falhou
> Mensagem: A solicitação falhou com status HTTP 503: Serviço indisponível.
> 
> Fonte: Microsoft.UpdateServices.Administration.AdminProxy.createUpdateServer.

Quando você tenta abrir os serviços de atualização no servidor do WSUS, você recebe o seguinte erro:

> Erro: Erro de Conexão
> 
> Erro ao tentar se conectar ao servidor do WSUS. Esse erro pode ocorrer por vários motivos. Entre em contato com seu administrador de rede se o problema persistir. Clique a reinicialização do nó do servidor para se conectar ao servidor novamente.

Além disso, as tentativas de acesso a URL para o site de administração do WSUS (ou seja, `http://CM12CAS:8530`) falha com o erro:

> Erro HTTP 503. O serviço está indisponível

Nessa situação, a causa mais provável é que o Pool de aplicativos de WsusPool no IIS está em um estado parado.

Além disso, o limite de memória privada (KB) para o Pool de aplicativos provavelmente é definido como o valor padrão de 1843200 KB. Se você encontrar esse problema, aumente o limite de memória privada até 4GB (4000000 KB) e reinicie o Pool de aplicativos. Para aumentar o limite de memória privada, selecione o Pool de aplicativos de WsusPool e clique em configurações avançadas em Editar Pool de aplicativos. Em seguida, defina o limite de memória privada até 4GB (4000000 KB). Depois que o Pool de aplicativos foi reiniciado, monitore o status do componente SMS_WSUS_SYNC_MANAGER, WCM e WSyncMgr. log para falhas. Observe que talvez seja necessário aumentar o limite de memória privada (8000000 KB) de 8 GB ou superior, dependendo do ambiente.

Para obter mais detalhes, consulte: [Falha na sincronização do WSUS no ConfigMgr 2012 com erros HTTP 503](http://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>Erro 0x80070643: Erro fatal durante a instalação
Instalação do WSUS usa o Microsoft SQL Server para executar a instalação. Esse problema ocorre porque o usuário que está executando a instalação do WSUS não tem permissões de administrador do sistema no SQL Server.

Para resolver esse problema, conceda permissões de administrador do sistema para uma conta de usuário ou para uma conta de grupo no SQL Server e, em seguida, execute novamente a instalação do WSUS.

## <a name="some-services-are-not-running-check-the-following-services"></a>Alguns serviços não estão em execução. Verifique os seguintes serviços:

- **SelfUpdate:** Ver [automática as atualizações devem ser atualizados](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx) para obter informações sobre como solucionar problemas do serviço Selfupdate.

- **WSSUService.exe:** Esse serviço facilita a sincronização. Se você tiver problemas com a sincronização, acessar WSUSService.exe clicando **inicie**, apontando para **ferramentas administrativas**, clicando em **serviços**e, em seguida, localizando **Windows Server Update Services** na lista de serviços. Faça o seguinte:
    
    -   Verifique se que esse serviço está em execução. Clique em **inicie** se ela estiver interrompida ou **reiniciar** para atualizar o serviço.
    
    -   Use o Visualizador de eventos para verificar a **Application**, **Securit**y, e **sistema** logs de evento para ver se há quaisquer eventos que podem indicar um problema.
    
    -   Você também pode verificar o log para ver se há eventos que podem indicar um problema.

- **ServicesSQL do Web Service:** Serviços Web são hospedados no IIS. Se não estiver executando, certifique-se de que o IIS está em execução (ou iniciado). Você também pode tentar redefinir o serviço Web, digitando **iisreset** em um prompt de comando.

- **SQL Service:** Cada serviço, exceto para o serviço selfupdate requer que o serviço do SQL está em execução. Se qualquer um dos arquivos de log indicarem problemas de conexão do SQL, verifique o serviço SQL pela primeira vez. Para acessar o serviço do SQL, clique em **inicie**, aponte para **ferramentas administrativas**, clique em **serviços**e, em seguida, procure por um dos seguintes:
    
  - **MSSQLSERver** (se você estiver usando o WMSDE ou MSDE, ou se você estiver usando o SQL Server e estiver usando o nome da instância padrão para o nome da instância)
    
  - **MSSQL$ WSUS** (se você estiver usando um banco de dados do SQL Server e ter chamado "WSUS" de sua instância de banco de dados)
    
    O serviço com o botão direito e, em seguida, clique em **inicie** se o serviço não está em execução, ou **reiniciar** para atualizar o serviço se ele está em execução.
