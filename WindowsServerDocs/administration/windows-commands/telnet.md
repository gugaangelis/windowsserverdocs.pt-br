---
title: telnet
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b1621a10b9b445e87eb6bc16a8d7f52a64c146a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385148"
---
# <a name="telnet"></a>telnet

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comunica-se com um computador que executa o serviço do servidor Telnet. 
## <a name="syntax"></a>Sintaxe
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|SRDF|Tente fazer logon automaticamente. O mesmo que a opção/l, exceto usa o nome de s do usuário conectado no momento.|
|/e \<EscapeChar >|Caractere de escape usado para entrar no prompt do cliente Telnet.|
|/f \<FileName >|Nome de arquivo usado para registro no lado do cliente.|
|/l \<UserName >|Especifica o nome de usuário no qual fazer logon no computador remoto.|
|/t {vt100 &#124; vt52 &#124; ANSI &#124; VTNT}|Especifica o tipo de terminal. Os tipos de terminal com suporte são VT100, vt52, ANSI e VTNT.|
|\<Host > [\<Port >]|Especifica o nome do host ou endereço IP do computador remoto ao qual se conectar e, opcionalmente, a porta TCP a ser usada (o padrão é a porta TCP 23).|
|/?|Exibe a ajuda no prompt de comando. Como alternativa, você pode digitar/h.|

## <a name="remarks"></a>Comentários
-   Você deve instalar o software cliente Telnet antes de poder executar este comando. Para obter mais informações, consulte [instalando o Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   Você pode executar o Telnet sem parâmetros para inserir o contexto de Telnet, indicado pelo prompt Telnet (**Microsoft telnet >** ). No prompt de Telnet, você pode usar comandos Telnet para gerenciar o computador que executa o cliente Telnet.

## <a name="BKMK_Examples"></a>Disso
Use o Telnet para se conectar ao computador que está executando o serviço do servidor Telnet em telnet.microsoft.com.
```
telnet telnet.microsoft.com
```
Use o Telnet para se conectar ao computador que executa o serviço do servidor Telnet em telnet.microsoft.com na porta TCP 44 e registrar a atividade de sessão em um arquivo local chamado telnetlog. txt
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Referências adicionais
-   [Instalando o Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [Referência técnica de Telnet](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
