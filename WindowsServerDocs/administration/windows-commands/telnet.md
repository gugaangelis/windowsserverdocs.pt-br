---
title: telnet
description: Tópico de comandos do Windows para telnet, que se comunica com um computador que executa o serviço do servidor Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b59a3891dd276c6ab0b8e7a8a0a2d11a6b6b55c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833129"
---
# <a name="telnet"></a>telnet

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comunica-se com um computador que executa o serviço do servidor Telnet.
 
## <a name="syntax"></a>Sintaxe
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/a|Tente fazer logon automaticamente. O mesmo que a opção/l, exceto usa o nome de s do usuário conectado no momento.|
|/e \<EscapeChar >|Caractere de escape usado para entrar no prompt do cliente Telnet.|
|/f \<nome do arquivo >|Nome de arquivo usado para registro no lado do cliente.|
|/l \<nome de usuário >|Especifica o nome de usuário no qual fazer logon no computador remoto.|
|/t {vt100 &#124; vt52 &#124; ANSI &#124; VTNT}|Especifica o tipo de terminal. Os tipos de terminal com suporte são VT100, vt52, ANSI e VTNT.|
|> do host \<[\<porta >]|Especifica o nome do host ou endereço IP do computador remoto ao qual se conectar e, opcionalmente, a porta TCP a ser usada (o padrão é a porta TCP 23).|
|/?|Exibe a ajuda no prompt de comando. Como alternativa, você pode digitar/h.|

## <a name="remarks"></a>Comentários
-   Você deve instalar o software cliente Telnet antes de poder executar este comando. Para obter mais informações, consulte [instalando o Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   Você pode executar o Telnet sem parâmetros para inserir o contexto de Telnet, indicado pelo prompt Telnet (**Microsoft telnet >** ). No prompt de Telnet, você pode usar comandos Telnet para gerenciar o computador que executa o cliente Telnet.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso
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
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
