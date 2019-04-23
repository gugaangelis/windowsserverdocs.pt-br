---
title: adicionar
description: Tópico de comandos do Windows para **add_1** - adiciona volumes para o conjunto de volumes que devem ser copiados em sombra ou aliases para o ambiente de alias.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 549c8560774f004a60926ce568c850fd1b71c7f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889947"
---
# <a name="add"></a>adicionar


Adiciona os volumes para o conjunto de volumes que devem ser copiados em sombra ou aliases para o ambiente de alias. Se usado sem subcomandos, **adicionar** lista os aliases e os volumes atuais.

> [!NOTE]
> Aliases não são adicionados ao ambiente de alias, até que a cópia de sombra é criada. Aliases que você precisa imediatamente devem ser adicionados por meio **Adicionar alias**.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
add 
add volume <Volume> [provider <ProviderID>] 
add alias <AliasName> <AliasValue>
```

## <a name="add-subcommands"></a>Adicionar subcomandos

|Subcomando|Descrição|
|----------|-----------|
|volume|Adiciona um volume para o conjunto de cópias de sombra, que é o conjunto de volumes a serem copiados em sombra. Ver [Adicionar volume](add-volume.md) para sintaxe e parâmetros.|
|alias|Adiciona o nome fornecido e o valor para o ambiente de alias. Ver [Adicionar alias](add-alias.md) para sintaxe e parâmetros.|
|/?|Exibe a Ajuda na linha de comando.|

## <a name="BKMK_examples"></a>Exemplos

Para exibir os volumes adicionados e os aliases que estão atualmente no ambiente, digite:
```
add
```
A saída a seguir mostra que a unidade C foi adicionada ao conjunto de cópia de sombra:
```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)