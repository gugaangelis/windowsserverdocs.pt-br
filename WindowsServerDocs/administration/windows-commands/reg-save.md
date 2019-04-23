---
title: reg salvar
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a46dfe081421ed727bd7ffeeab364e6c23dd801
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841077"
---
# <a name="reg-save"></a>reg salvar



Salva uma cópia de subchaves especificadas, as entradas e valores do registro em um arquivo especificado.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave. Para especificar computadores remotos, inclua o nome do computador (no formato \\ \\ComputerName\) como parte do *KeyName*. A omissão \\ \\ComputerName\ faz com que a operação padrão no computador local. O *KeyName* deve incluir uma chave de raiz válido. As chaves de raiz válido para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves válidas são: HKLM e HKU.|
|\<FileName>|Especifica o nome e caminho do arquivo que é criado. Se nenhum caminho for especificado, o caminho atual será usado.|
|/y|Substitui um arquivo existente com o nome *FileName* sem solicitar confirmação.|
|/?|Exibe a Ajuda para **reg salvar** no prompt de comando.|

## <a name="remarks-optional-section"></a>Comentários \<seção opcional >

-   A tabela a seguir lista os valores retornados para o **reg salvar** operação.

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|
-   Antes de editar as entradas de registro, salve a subchave pai com o **reg salvar** operação. Se houver falha na edição, restaure a subchave com o **restauração reg** operação.

## <a name="BKMK_examples"></a>Exemplos

Para salvar o hive MyApp para a pasta atual como um arquivo chamado AppBkUp, digite:
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)