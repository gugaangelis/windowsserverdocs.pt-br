---
title: shadow
description: Tópico de comandos do Windows para sombra, que permite controlar remotamente uma sessão ativa de outro usuário em um servidor de Host da Sessão da Área de Trabalho Remota.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90c3202d810257cc94c73b88c5c1627901f54af0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834329"
---
# <a name="shadow"></a>shadow

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite controlar remotamente uma sessão ativa de outro usuário em um Host da Sessão da Área de Trabalho Remota Server.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|\<SessionName >|Especifica o nome da sessão que você deseja controlar remotamente.|
|\<SessionID >|Especifica a ID da sessão que você deseja controlar remotamente. Use o **usuário de consulta** para exibir a lista de sessões e suas IDs de sessão.|
|/Server:\<ServerName >|Especifica o servidor de host da sessão da área de trabalho remota que contém a sessão que você deseja controlar remotamente. Por padrão, o servidor de Host4 da Sessão RD atual é usado.|
|/v|Exibe informações sobre as ações que estão sendo executadas.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Você pode exibir ou controlar ativamente a sessão. Se optar por controlar ativamente a sessão de um usuário, você poderá inserir ações de teclado e mouse na sessão.
-   Você sempre pode controlar remotamente suas próprias sessões (exceto a sessão atual), mas deve ter permissão de controle total ou permissão de acesso especial de controle remoto para controlar remotamente outra sessão.
-   Você também pode iniciar o controle remoto usando Gerenciador de Serviços de Área de Trabalho Remota.
-   Antes do início do monitoramento, o servidor avisa ao usuário que a sessão está prestes a ser controlada remotamente, a menos que este aviso seja desabilitado. Sua sessão pode parecer estar congelada por alguns segundos enquanto aguarda uma resposta do usuário. Para configurar o controle remoto para usuários e sessões, use a ferramenta de configuração Serviços de Área de Trabalho Remota ou as extensões de Serviços de Área de Trabalho Remota para usuários e grupos locais e usuários e computadores do Active Directory.
-   Sua sessão deve ser capaz de dar suporte à resolução de vídeo usada na sessão que você está controlando remotamente ou a operação falha.
-   A sessão de console não pode controlar remotamente outra sessão nem pode ser controlada remotamente por outra sessão.
-   Quando você quiser encerrar o controle remoto (sombreamento), pressione CTRL +\* (usando \* somente do teclado numérico).

## <a name="examples"></a><a name=BKMK_examples></a>Disso
-   Para a sessão de sombra 93, digite:
    ```
    shadow 93
    ```
-   Para sombrear a sessão ACCTG01, digite:
    ```
    shadow ACCTG01
    ```

## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência de comando serviços de área de trabalho remota (serviços de terminal)](remote-desktop-services-terminal-services-command-reference.md)
