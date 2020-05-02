---
title: chkntfs
description: Tópico de referência para o comando chkntfs, que exibe ou modifica a verificação automática de disco quando o computador é iniciado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 69b8e21a7b43538b6296666d813f2b33daa8045f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82713655"
---
# <a name="chkntfs"></a>chkntfs

Exibe ou modifica a verificação automática de disco quando o computador é iniciado. Se for usado sem opções, o **chkntfs** exibirá o sistema de arquivos do volume especificado. Se a verificação automática de arquivos estiver agendada para execução, o **chkntfs** exibirá se o volume especificado está sujo ou agendado para ser verificado na próxima vez em que o computador for iniciado.

> [!NOTE]
> Para executar o **chkntfs**, você deve ser um membro do grupo Administradores.

## <a name="syntax"></a>Sintaxe

```
chkntfs <volume> [...]
chkntfs [/d]
chkntfs [/t[:<time>]]
chkntfs [/x <volume> [...]]
chkntfs [/c <volume> [...]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<volume>` [...] | Especifica um ou mais volumes a serem verificados quando o computador for iniciado. Os volumes válidos incluem letras de unidade (seguidas por dois-pontos), pontos de montagem ou nomes de volume. |
| /d | Restaura todas as configurações padrão do **chkntfs** , exceto o tempo de contagem regressiva da verificação automática de arquivos. Por padrão, todos os volumes são verificados quando o computador é iniciado e o **chkdsk** é executado naqueles que estão sujos. |
| /t [`:<time>`] | Altera o tempo de contagem regressiva inicial do autochk. exe para o período de tempo especificado em segundos. Se você não inserir uma hora, **/t** exibirá o tempo de contagem regressiva atual. |
| /x `<volume>` [...] | Especifica um ou mais volumes a serem excluídos da verificação quando o computador é iniciado, mesmo que o volume seja marcado como exigindo **chkdsk**. |
| /c `<volume>` [...] | Agenda um ou mais volumes a serem verificados quando o computador é iniciado e executa o **chkdsk** naqueles que estão sujos. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para exibir o tipo de sistema de arquivos para a unidade C, digite:

```
chkntfs c:
```

> [!NOTE]
> Se a verificação automática de arquivos estiver agendada para execução, a saída adicional será exibida, indicando se a unidade está suja ou se foi agendada manualmente para ser verificada na próxima vez em que o computador for iniciado.

Para exibir o tempo de contagem regressiva inicial de autochk. exe, digite:

```
chkntfs /t
```

Para alterar o tempo de contagem regressiva inicial de autochk. exe para 30 segundos, digite:

```
chkntfs /t:30
```

> [!NOTE]
> Embora seja possível definir o tempo de contagem regressiva inicial de autochk. exe como zero, isso impedirá que você cancele uma verificação de arquivo automática potencialmente demorada.

Para excluir vários volumes de serem verificados, você deve listá-los em um único comando. Por exemplo, para excluir os volumes D e E, digite:

```
chkntfs /x d: e:
```

> [!IMPORTANT]
> A opção de linha de comando **/x** não é cumulativa. Se você digitá-lo mais de uma vez, a entrada mais recente substituirá a entrada anterior.

Para agendar a verificação automática de arquivos no volume D, mas não nos volumes C ou E, digite os seguintes comandos na ordem:

```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

> [!IMPORTANT]
> A opção de linha de comando **/c** é cumulativa. Se você digitar **/c** mais de uma vez, cada entrada permanecerá. Para garantir que apenas um determinado volume esteja marcado, redefina os padrões para limpar todos os comandos anteriores, excluir todos os volumes de serem verificados e, em seguida, agendar a verificação automática de arquivos no volume desejado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
