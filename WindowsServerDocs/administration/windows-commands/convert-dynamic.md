---
title: converter dinâmico
description: O tópico de comandos do Windows para converter dinâmico, que converte um disco básico em um disco dinâmico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ad84cf2ecb6808b68110187b52f3fc13590b491
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847319"
---
# <a name="convert-dynamic"></a>converter dinâmico

Converte um disco básico em um disco dinâmico.

Para obter instruções sobre como usar esse comando, consulte [alterar um disco básico em um disco dinâmico](https://go.microsoft.com/fwlink/?LinkId=207047) (https://go.microsoft.com/fwlink/?LinkId=207047).

## <a name="syntax"></a>Sintaxe

```
convert dynamic [noerr]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

-   Todas as partições existentes no disco básico se tornam volumes simples.
-   Um disco básico deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco básico e deslocar o foco para ele.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para converter um disco básico em um disco dinâmico, digite:
```
convert dynamic
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

