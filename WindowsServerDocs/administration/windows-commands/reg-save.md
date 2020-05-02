---
title: Reg salvar
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dd3e932e67df7eb972bd625ecec24f986cf3f3d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722508"
---
# <a name="reg-save"></a>Reg salvar



Salva uma cópia de subchaves, entradas e valores especificados do registro em um arquivo especificado.



## <a name="syntax"></a>Sintaxe

```
reg save <KeyName> <FileName> [/y]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave. Para especificar computadores remotos, inclua o nome do computador (no \\ \\formato ComputerName\) como parte do *KeyName*. \\ \\Omitir computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU.|
|\<Nome de arquivo>|Especifica o nome e o caminho do arquivo que é criado. Se nenhum caminho for especificado, o caminho atual será usado.|
|/y|Substitui um arquivo existente pelo *nome nome do arquivo sem solicitar* confirmação.|
|/?|Exibe a ajuda para o **reg Save** no prompt de comando.|

## <a name="remarks-optional-section"></a>Comentários \<da seção opcional>

-   A tabela a seguir lista os valores de retorno para a operação **reg Save** .

|Valor|Descrição|
|-----|-----------|
|0|Sucesso|
|1|Falha|
-   Antes de editar as entradas do registro, salve a subchave pai com a operação **reg Save** . Se a edição falhar, restaure a subchave original com a operação **reg Restore** .

## <a name="examples"></a>Exemplos

Para salvar o hive MyApp na pasta atual como um arquivo chamado AppBkUp. HIV, digite:
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)