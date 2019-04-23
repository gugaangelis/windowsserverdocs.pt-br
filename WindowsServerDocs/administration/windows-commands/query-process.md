---
title: processo de consulta
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e15a41fc931d2cf04d60759a63e3b80392265175
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840717"
---
# <a name="query-process"></a>processo de consulta

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre processos em execução em um servidor de Host de sessão de área de trabalho remota (Host de sessão rd).
Você pode usar esse comando para descobrir quais programas de um usuário específico está em execução, e também que os usuários estão executando um programa específico.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
## <a name="syntax"></a>Sintaxe
```
query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|*|lista os processos para todas as sessões.|
|<ProcessID>|Especifica a ID numérica de identificar o processo que você deseja consultar.|
|<UserName>|Especifica o nome do usuário cujos processos você deseja listar.|
|<SessionName>|Especifica o nome da sessão cujos processos você deseja listar.|
|/id:<nn>|Especifica a ID da sessão cujos processos você deseja listar.|
|<ProgramName>|Especifica o nome do programa cujos processos você deseja consultar. A extensão .exe é necessária.|
|/server:<ServerName>|Especifica o servidor de Host de sessão de área de trabalho remota cujos processos você deseja listar. Se não for especificado, o servidor no qual você está conectado no momento será usado.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
-   Os administradores têm acesso completo a todos **processo de consulta** funções.
-   Se você não especificar o <*nome de usuário*>, <*SessionName*>, **/id:**<*nn*>, <*ProgramName*>, ou **\*** parâmetros **processo de consulta** exibe apenas os processos que pertencem ao usuário atual.
-   Se uma sessão for especificada, ele deve identificar uma sessão ativa.
-   **processo de consulta** retorna as seguintes informações:
    -   O usuário que possui o processo
    -   A sessão que possui o processo
    -   A ID da sessão
    -   O nome do processo
    -   A ID do processo
-   Quando **processo de consulta** retorna informações, um símbolo de maior que (>) são exibidas antes de cada processo que pertence à sessão atual.
## <a name="BKMK_examples"></a>Exemplos
-   Para exibir informações sobre os processos que estão sendo usados por todas as sessões, digite:
    ```
    query process *
    ```
-   Para exibir informações sobre os processos que estão sendo usados pelo 2 de ID de sessão, digite:
    ```
    query process /ID:2
    ```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[consulta](query.md)
[Remote Desktop Services &#40;serviços de Terminal&#41; referência de comandos](remote-desktop-services-terminal-services-command-reference.md)
