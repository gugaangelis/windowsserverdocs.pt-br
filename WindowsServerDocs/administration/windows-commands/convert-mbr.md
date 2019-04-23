---
title: convert mbr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da8d62567863bc38a5aa0b35a8f3fe4ee24cc888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834607"
---
# <a name="convert-mbr"></a>convert mbr



Converte um disco básico vazio com o estilo de partição da tabela de partição GUID (GPT) em um disco básico com o estilo de partição (MBR).

> [!IMPORTANT]
> O disco deve estar vazio para convertê-lo em um disco MBR. Fazer backup dos dados e, em seguida, exclua todas as partições ou volumes antes de converter o disco.

Para obter instruções sobre como usar esse comando, consulte [alterar um disco de tabela de partição GUID em um disco de registro mestre de inicialização](https://go.microsoft.com/fwlink/?LinkId=207050) (https://go.microsoft.com/fwlink/?LinkId=207050).

## <a name="syntax"></a>Sintaxe

```
convert mbr [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

-   Um disco básico deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco básico e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para converter um disco básico do estilo de partição GPT em estilo de partição MBR, digite >:
```
convert mbr
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

