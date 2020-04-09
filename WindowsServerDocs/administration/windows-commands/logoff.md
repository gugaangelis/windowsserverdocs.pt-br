---
title: fazer logoff
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1661a9dd6cc89ea05980fd9085aa8fa67b8fe2c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840409"
---
# <a name="logoff"></a>fazer logoff

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz logoff de um usuário de uma sessão em um servidor Host da Sessão da Área de Trabalho Remota (host de Sessão RD) e exclui a sessão do servidor.
para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
### <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                             Descrição                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  Especifica o nome da sessão.                                                                  |
|     <SessionID>      |                                                 Especifica a ID numérica que identifica a sessão para o servidor.                                                 |
| /server:<ServerName> | Especifica o servidor de host da sessão da área de trabalho remota que contém a sessão cujo usuário você deseja fazer logoff. Se não for especificado, o servidor no qual você está ativo no momento será usado. |
|          /v          |                                                       Exibe informações sobre as ações que estão sendo executadas.                                                        |
|          /?          |                                                                 Exibe a ajuda no prompt de comando.                                                                 |

## <a name="remarks"></a>Comentários
- Você sempre pode fazer logoff da sessão à qual você está conectado no momento. No entanto, você deve ter permissão controle total para fazer logoff de usuários de outras sessões.
- Fazer logoff de um usuário de uma sessão sem aviso pode resultar em perda de dados na sessão do usuário. Você deve enviar uma mensagem para o usuário usando o comando **msg** para avisar o usuário antes de executar esta ação.
- se <*SessionID*> ou <*SessionName*> não for especificado, **logoff fará logoff** do usuário da sessão atual. Se você especificar <*sessionname*>, ele deverá ser um ativo.
- Quando você faz logoff de um usuário, todos os processos são encerrados e a sessão é excluída do servidor.
- Não é possível fazer logoff de um usuário da sessão de console.
  ## <a name="examples"></a><a name=BKMK_examples></a>Disso
- Para fazer logoff de um usuário da sessão atual, digite:
  ```
  logoff
  ```
- Para fazer logoff de um usuário de uma sessão usando a ID da sessão, por exemplo, sessão 12, digite:
  ```
  logoff 12
  ```
- Para fazer logoff de um usuário de uma sessão usando o nome da sessão e do servidor, por exemplo, Session TERM04 no Server1, digite:
  ```
  logoff TERM04 /server:Server1
  ```

## <a name="additional-references"></a>Referências adicionais
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
