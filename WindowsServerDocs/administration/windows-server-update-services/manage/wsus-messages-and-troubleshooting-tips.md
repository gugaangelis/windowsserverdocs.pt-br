---
title: Mensagens do WSUS e dicas de solução de problemas
description: Tópico do Windows Server Update Service (WSUS)-solucionar problemas usando mensagens do WSUS
ms.topic: article
ms.assetid: 9f6317f7-bfe0-42d9-87ce-d8f038c728ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf3d0337dfa7360bdf8304c587c4ea31b7607e27
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990957"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>Mensagens do WSUS e dicas de solução de problemas

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico contém informações sobre as seguintes mensagens do WSUS:

-   O computador não relatou o status

-   ID da mensagem 6703-falha na sincronização do WSUS

-   Erro 0x80070643: erro fatal durante a instalação

-   Alguns serviços não estão em execução. Verifique os seguintes serviços [...]

## <a name="computer-has-not-reported-status"></a>O computador não relatou o status
Essa mensagem é gerada no console do WSUS quando um computador cliente do WSUS não envia informações ao servidor do WSUS para indicar seu estado de atualização atual. Esse problema é normalmente causado pelo computador cliente do WSUS, não pelo servidor do WSUS.

Os motivos mais comuns são:

-   O computador perdeu a conectividade com a rede:
    -   O cabo de rede está desconectado.
    -   Um cabo de rede intermediário está com defeito.
    -   O computador tem um adaptador de rede com falha.
    -   A porta de rede à qual o computador se conecta foi desabilitada.
    -   O adaptador sem fio não pode ser associado e se conectar ao ponto de acesso sem fio corporativo.
-   O computador está desligado. (Ele foi desligado ou está no modo de suspensão ou hibernação.)

## <a name="message-id-6703---wsus-synchronization-failed"></a>ID da mensagem 6703-falha na sincronização do WSUS
> Mensagem: a solicitação falhou com o status HTTP 503: serviço não disponível.
>
> Origem: Microsoft. updateservices. Administration. AdminProxy. createUpdateServer.

Ao tentar abrir os serviços de atualização no servidor do WSUS, você recebe o seguinte erro:

> Erro: erro de conexão
>
> Ocorreu um erro ao tentar se conectar ao servidor do WSUS. Esse erro pode ocorrer por vários motivos. Entre em contato com seu administrador de rede se o problema persistir. Clique no nó redefinir servidor para se conectar ao servidor novamente.

Além do que está acima, as tentativas de acessar a URL para o site de administração do WSUS (ou seja, `http://CM12CAS:8530` ) falham com o erro:

> Erro HTTP 503. O serviço está indisponível

Nessa situação, a causa mais provável é que o pool de aplicativos WsusPool no IIS está em um estado parado.

Além disso, o limite de memória privada (KB) para o pool de aplicativos é provavelmente definido como o valor padrão de 1843200 KB. Se você encontrar esse problema, aumente o limite de memória privada para 4 GB (4 milhões KB) e reinicie o pool de aplicativos. Para aumentar o limite de memória privada, selecione o pool de aplicativos WsusPool e clique em configurações avançadas em Editar pool de aplicativos. Em seguida, defina o limite de memória privada para 4GB (4 milhões KB). Depois que o pool de aplicativos for reiniciado, monitore o status do componente SMS_WSUS_SYNC_MANAGER, WCM. log e wsyncmgr. log para obter falhas. Observe que pode ser necessário aumentar o limite de memória privada para 8 GB (8 milhões KB) ou superior, dependendo do ambiente.

Para obter detalhes adicionais, consulte: [a sincronização do WSUS no ConfigMgr 2012 falha com erros HTTP 503](https://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>Erro 0x80070643: erro fatal durante a instalação
A instalação do WSUS usa Microsoft SQL Server para executar a instalação. Esse problema ocorre porque o usuário que está executando a instalação do WSUS não tem permissões de administrador do sistema no SQL Server.

Para resolver esse problema, conceda permissões de administrador do sistema a uma conta de usuário ou a uma conta de grupo no SQL Server e execute a instalação do WSUS novamente.

## <a name="some-services-are-not-running-check-the-following-services"></a>Alguns serviços não estão em execução. Verifique os seguintes serviços:

- **Selfupdate:** Consulte [atualizações automáticas deve ser atualizado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708554(v=ws.10)) para obter informações sobre como solucionar problemas do serviço selfupdate.

- **WSSUService.exe:** Esse serviço facilita a sincronização. Se você tiver problemas com a sincronização, acesse WSUSService.exe clicando em **Iniciar**, apontando para **Ferramentas administrativas**, clicando em **Serviços**e localizando **serviço de atualização do Windows Server** na lista de serviços. Faça o seguinte:

    -   Verifique se esse serviço está em execução. Clique em **Iniciar** se ele for interrompido ou **reiniciado** para atualizar o serviço.

    -   Use Visualizador de Eventos para verificar os logs de eventos do **aplicativo**, do **securit**s e do **sistema** para ver se há algum evento que possa indicar um problema.

    -   Você também pode verificar o SoftwareDistribution. log para ver se há eventos que podem indicar um problema.

- **Serviço Web servicesSQL:** Os serviços Web são hospedados no IIS. Se eles não estiverem em execução, verifique se o IIS está em execução (ou iniciado). Você também pode tentar redefinir o serviço Web digitando **iisreset** em um prompt de comando.

- **Serviço SQL:** Cada serviço, exceto para o serviço selfupdate, requer que o serviço SQL esteja em execução. Se qualquer um dos arquivos de log indicar problemas de conexão do SQL, verifique o serviço SQL primeiro. Para acessar o serviço SQL, clique em **Iniciar**, aponte para **Ferramentas administrativas**, clique em **Serviços**e procure um dos seguintes:

  - **MSSQLSERver** (se você estiver usando o WMSDE ou o MSDE, ou se estiver usando SQL Server e estiver usando o nome de instância padrão para o nome da instância)

  - **MSSQL $ WSUS** (se você estiver usando um banco de dados SQL Server e tiver nomeado sua instância de banco de dados WSUS)

    Clique com o botão direito do mouse no serviço e clique em **Iniciar** se o serviço não estiver em execução ou **reinicie** para atualizar o serviço se ele estiver em execução.