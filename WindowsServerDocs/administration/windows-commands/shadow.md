---
title: shadow
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 125b2971d5d4783ea0b974c45b988cbd1c58a2bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877107"
---
# <a name="shadow"></a>shadow

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite controlar remotamente uma sessão ativa de outro usuário em um servidor de Host de sessão de área de trabalho remota (Host de sessão rd).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|\<SessionName>|Especifica o nome da sessão que você deseja controlar remotamente.|
|\<SessionID>|Especifica a ID da sessão que você deseja controlar remotamente. Use **consultar usuário** para exibir a lista de sessões e as identificações de sessão.|
|/server:\<ServerName>|Especifica o servidor de Host de sessão de área de trabalho remota que contém a sessão que você deseja controlar remotamente. Por padrão, o servidor de área de trabalho remota Host4 da sessão atual é usado.|
|/v|Exibe informações sobre as ações que está sendo executada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Você pode exibir ou controlar ativamente a sessão. Se você optar por controlar ativamente uma sessão de usuário, você poderá inserir ações teclado e mouse à sessão.
-   Você pode sempre controlar remotamente suas próprias sessões (exceto a sessão atual), mas você deve ter a permissão Controle total ou a permissão de acesso especiais de controle remoto para controlar remotamente outra sessão.
-   Você também pode iniciar o controle remoto usando o Gerenciador de serviços de área de trabalho remota.
-   Antes do início do monitoramento, o servidor avisa o usuário que a sessão está prestes a ser controlado remotamente, a menos que esse aviso é desativado. A sessão pode parecer estar congelada por alguns segundos enquanto ele aguarda uma resposta do usuário. Para configurar o controle remoto para usuários e sessões, use a ferramenta de configuração de serviços de área de trabalho remota ou as extensões de serviços de área de trabalho remota para usuários e grupos e locais do Active Directory que os usuários e computadores.
-   Sua sessão deve ser capaz de dar suporte a resolução de vídeo usada na sessão que você está controlando remotamente ou a operação falhará.
-   A sessão de console não pode controlar remotamente outra sessão nem ser controlado remotamente por outra sessão.
-   Quando você deseja encerrar o controle remoto (sombreamento), pressione CTRL + * (usando \* do teclado numérico somente).

## <a name="BKMK_examples"></a>Exemplos
-   A sessão de sombra 93, digite:
    ```
    shadow 93
    ```
-   Para sombrear a sessão ACCTG01, digite:
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)
