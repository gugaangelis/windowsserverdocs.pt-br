---
title: delete volume
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d25ed68077f594c765cf5630648ad52528d8fe3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872507"
---
# <a name="delete-volume"></a>delete volume



Exclui o volume selecionado.

## <a name="syntax"></a>Sintaxe

```
delete volume [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

-   Você não pode excluir o volume do sistema, o volume de inicialização ou qualquer volume contend o arquivo de paginação ativo ou o despejo de pane (despejo de memória).
-   Um volume deve ser selecionado para essa operação seja bem-sucedida. Use o **selecionar volume** comando para selecionar um volume e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para excluir o volume com foco, digite:
```
delete volume
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

