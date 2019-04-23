---
title: chkntfs
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 876c0e0c254216ac217aea7d165d5f4e3a7da9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861987"
---
# <a name="chkntfs"></a>chkntfs



Exibe ou modifica automática quando o computador é iniciado de verificação de disco. Se usado sem opções, **chkntfs** exibe o sistema de arquivos do volume especificado. Se a verificação automática de arquivos está agendada para ser executado, **chkntfs** exibe se o volume especificado está sujo ou está programado para ser verificado na próxima vez que o computador é iniciado.

> [!NOTE]
> Para executar **chkntfs**, você deve ser um membro do grupo Administradores.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
chkntfs <Volume> [...]
chkntfs [/d]
chkntfs [/t[:<Time>]]
chkntfs [/x <Volume> [...]]
chkntfs [/c <Volume> [...]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Volume > [...]|Especifica um ou mais volumes para verificar quando o computador é iniciado. Volumes válidos incluem letras de unidade (seguidas por dois-pontos), pontos ou nomes de volume de montagem.|
|/d|Restaura todos os **chkntfs** configurações, exceto o tempo de contagem regressiva para a verificação automática de arquivos padrão. Por padrão, todos os volumes são verificados quando o computador é iniciado, e **chkdsk** é executado nas informações que estão sujos.|
|/t [:\<Time>]|Altera o tempo de contagem regressiva Autochk.exe inicial para o período de tempo especificado em segundos. Se você não inserir uma hora, **/t** exibe a hora atual da contagem regressiva.|
|/x \<Volume> [...]|Especifica um ou mais volumes a serem excluídos da verificação quando o computador é iniciado, mesmo se o volume é marcado como exigindo **chkdsk**.|
|/c \<volume > [...]|Agenda a um ou mais volumes a ser verificado quando o computador é iniciado e executa **chkdsk** naqueles que esteja sujo.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Exemplos

Para exibir o tipo de sistema de arquivos para a unidade C, digite:
```
chkntfs c:
```
A saída a seguir indica um sistema de arquivos NTFS:
```
The type of the file system is NTFS.
```

> [!NOTE]
> Se a verificação automática de arquivos está agendado para execução, exibirá saída adicionais, que indica se a unidade está suja ou tiver sido manualmente agendado ser verificado na próxima vez que o computador é iniciado.

Para exibir o tempo de contagem regressiva Autochk.exe inicial, digite:
```
chkntfs /t
```
Por exemplo, se o tempo de contagem regressiva é definido como 10 segundos, exibe a saída a seguir:
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
Para alterar o tempo de contagem regressiva Autochk.exe inicial para 30 segundos, digite:
```
chkntfs /t:30
```

> [!NOTE]
> Embora você possa definir o tempo de contagem regressiva de iniciação Autochk.exe como zero, isso impedirá você de cancelamento de uma verificação automática de arquivos potencialmente demorada.

O **/x** opção de linha de comando não é cumulativa. Se você digitá-lo mais de uma vez, a entrada mais recente substituirá a entrada anterior. Para excluir vários volumes do que está sendo verificado, você deve listar cada um deles em um único comando. Por exemplo, para excluir o D e o E volumes, digite:
```
chkntfs /x d: e:
```
O **/c** opção de linha de comando é cumulativa. Se você digitar **/c** mais de uma vez, cada entrada permanece. Para garantir que apenas um volume específico é verificado, redefina os padrões para limpar todos os comandos anteriores, exclua todos os volumes do que está sendo verificado e, em seguida, agendar a verificação no volume desejado automática de arquivos.

Por exemplo, para agendar a verificação no volume D, mas não os volumes de C ou E automática de arquivos, digite os comandos a seguir na ordem:
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)