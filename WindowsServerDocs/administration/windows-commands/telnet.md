---
title: telnet
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6439a91d82d6d199666629e333d8130bf65a384b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858557"
---
# <a name="telnet"></a>telnet

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se comunica com um computador que executa o serviço do servidor telnet. 
## <a name="syntax"></a>Sintaxe
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/a|tentativa de logon automático. Mesmo que /l opção exceto usa atualmente conectado no nome de usuário s.|
|/e \<EscapeChar>|Usado para inserir o prompt de cliente telnet de caractere de escape.|
|/f \<FileName>|Nome do arquivo usado para registro em log do lado do cliente.|
|/l \<UserName >|Especifica o nome de usuário para fazer logon com no computador remoto.|
|/t {vt100 &#124; vt52 &#124; ansi &#124; vtnt}|Especifica o tipo de terminal. Tipos de terminal com suporte são vt100, vt52, ansi e vtnt.|
|\<Host> [\<Port>]|Especifica o nome do host ou endereço IP do computador remoto para se conectar a e, opcionalmente, a porta TCP a ser usada (o padrão é a porta TCP 23).|
|/?|Exibe a ajuda no prompt de comando. Como alternativa, você pode digitar /h.|

## <a name="remarks"></a>Comentários
-   Você deve instalar o software de cliente telnet antes de executar esse comando. Para obter mais informações, consulte [instalando o telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   Você pode executar o telnet sem parâmetros para inserir o contexto do telnet, indicado pelo prompt do telnet (**Microsoft telnet >**). No prompt de telnet, você pode usar comandos telnet para gerenciar o computador que executa o cliente telnet.

## <a name="BKMK_Examples"></a>Exemplos
Use o telnet para se conectar ao computador que executa o serviço do servidor telnet no telnet.microsoft.com.
```
telnet telnet.microsoft.com
```
Use o telnet para se conectar ao computador que executa o serviço do servidor telnet no telnet.microsoft.com na porta TCP 44 e de log a atividade de sessão em um arquivo local chamado telnetlog.txt
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Referências adicionais
-   [Instalando o telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [Referência técnica de Telnet](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
