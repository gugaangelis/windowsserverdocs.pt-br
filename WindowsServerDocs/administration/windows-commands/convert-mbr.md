---
title: convert mbr
description: O tópico de comandos do Windows para converter MBR, que converte um disco básico vazio com o estilo de partição GPT (tabela de partição GUID) em um disco básico com o estilo de partição MBR (registro mestre de inicialização).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eeaf79a380fb5f1074d2bbef004537804caa0d8d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847199"
---
# <a name="convert-mbr"></a>convert mbr

Converte um disco básico vazio com o estilo de partição GPT (tabela de partição GUID) em um disco básico com o estilo de partição MBR (registro mestre de inicialização).

> [!IMPORTANT]
> O disco deve estar vazio para convertê-lo em um disco MBR. Faça backup dos dados e exclua todas as partições ou volumes antes de converter o disco.

Para obter instruções sobre como usar esse comando, consulte [alterar um disco de tabela de partição GUID em um disco de registro mestre de inicialização](https://go.microsoft.com/fwlink/?LinkId=207050) (https://go.microsoft.com/fwlink/?LinkId=207050).

## <a name="syntax"></a>Sintaxe

```
convert mbr [noerr]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

-   Um disco básico deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco básico e deslocar o foco para ele.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para converter um disco básico do estilo de partição GPT em estilo de partição MBR, digite >:
```
convert mbr
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

