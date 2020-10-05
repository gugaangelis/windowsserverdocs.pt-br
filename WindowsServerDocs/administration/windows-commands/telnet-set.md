---
title: telnet set
description: Artigo de referência para o comando set do telnet, que define opções.
ms.topic: reference
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 05678eaa3c4308f72ef4a754cc3e352826fa473a
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717973"
---
# <a name="telnet-set"></a>Telnet: definir

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define opções. Você pode usar o [comando de desdefinição de Telnet](telnet-unset.md) para desativar uma opção que foi definida anteriormente.

## <a name="syntax"></a>Sintaxe

```
set [bsasdel] [crlf] [delasbs] [escape <char>] [localecho] [logfile <filename>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| bsasdel | Envia **Backspace** como uma **exclusão**. |
| CRLF | Envia CR & LF (0x0D, 0x 0A) quando a tecla **Enter** é pressionada. Conhecido como **modo de nova linha**. |
| delasbs | Envia **delete** como um **Backspace**. |
| fuga `<character>` | Define o caractere de escape usado para entrar no prompt do cliente Telnet. O caractere de escape pode ser um único caractere ou pode ser uma combinação da tecla **Ctrl** mais um caractere. Para definir uma combinação de chave de controle, mantenha pressionada a tecla **Ctrl** enquanto digita o caractere que você deseja atribuir. |
| localecho | Ativa o eco local. |
| restaura `<filename>` | Registra a sessão Telnet atual no arquivo local. O registro em log inicia automaticamente quando você define essa opção. |
| registro em log | Ativa o registro em log. Se nenhum arquivo de log estiver definido, uma mensagem de erro será exibida. |
| moda `{console | stream}` | Define o modo de operação. |
| NTLM | Ativa a autenticação NTLM. |
| prazo `{ansi | vt100 | vt52 | vtnt}` | Define o tipo de terminal. |
| ? | Exibe a ajuda para este comando. |

#### <a name="remarks"></a>Comentários

- Em versões diferentes do inglês do telnet, o **codeset** `<option>` está disponível. **Código de** `<option>` define o conjunto de códigos atual como uma opção, que pode ser qualquer um dos seguintes: **Shift JIS**, **EUC japonês**, **JIS kanji**, **JIS kanji (78)**, **Dec kanji**, **NEC kanji**. Você deve definir o mesmo conjunto de código no computador remoto.

## <a name="example"></a>Exemplo

Para definir o arquivo de log e iniciar o registro no arquivo local *tnlog.txt*, digite:

```
set logfile tnlog.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
