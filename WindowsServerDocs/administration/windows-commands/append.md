---
title: append
description: 'Tópico de comandos do Windows para '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abf7713b3fd5bbb6172969ca1cc39cbbbbafafc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881977"
---
# <a name="append"></a>append



Permite que os programas abrir arquivos de dados nos diretórios especificados como se estivessem no diretório atual. Se usado sem parâmetros, **acrescentar** exibe a lista de pastas acrescentadas.

> [!NOTE]
> Esse comando não tem suportado no Windows 10.
>

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive>:]<Path>|Especifica uma unidade e diretório para acrescentar.|
|/x:on|Aplica-se pastas acrescentadas a pesquisas de arquivo e inicialização de aplicativos.|
|/x:off|Aplica-se pastas acrescentadas apenas às solicitações para abrir arquivos.</br>**/x: off** é a configuração padrão.|
|/path:on|Aplica-se pastas acrescentadas às solicitações de arquivo que já especificam um caminho. **/path: em** é a configuração padrão.|
|/path:off|Desativa o efeito de **/path: em**.|
|/e|Armazena uma cópia da lista de pastas acrescentadas em uma variável de ambiente denominada APPEND. **/e** pode ser usado apenas na primeira vez em que você usar **acrescente** depois de iniciar seu sistema.|
|;|Limpa a lista de pastas acrescentadas.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Exemplos

Para limpar a lista de pastas acrescentadas, digite:
```
append ;
```
Para armazenar uma cópia do diretório acrescentada a uma variável de ambiente denominada APPEND, digite:
```
append /e
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
