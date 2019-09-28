---
title: adicionar
description: O tópico de comandos do Windows para **add_1** -adiciona volumes ao conjunto de volumes que devem ser copiados em sombra ou adiciona aliases ao ambiente de alias.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d1aaa211938d14a0019d29e64867f4df2475a877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382776"
---
# <a name="add"></a>adicionar


Adiciona volumes ao conjunto de volumes que devem ser copiados em sombra ou adiciona aliases ao ambiente de alias. Se usado sem subcomandos, **Adicionar** lista os volumes e aliases atuais.

> [!NOTE]
> Os aliases não são adicionados ao ambiente de alias até que a cópia de sombra seja criada. Os aliases que você precisa imediatamente devem ser adicionados usando **Add alias**.

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
|volume|Adiciona um volume ao conjunto de cópias de sombra, que é o conjunto de volumes a serem copiados em sombra. Consulte [Adicionar volume](add-volume.md) para sintaxe e parâmetros.|
|alias|Adiciona o nome e o valor fornecidos ao ambiente do alias. Consulte [Adicionar alias](add-alias.md) para sintaxe e parâmetros.|
|/?|Exibe a ajuda na linha de comando.|

## <a name="BKMK_examples"></a>Disso

Para exibir os volumes adicionados e os aliases que estão atualmente no ambiente, digite:
```
add
```
A saída a seguir mostra que a unidade C foi adicionada ao conjunto de cópias de sombra:
```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)