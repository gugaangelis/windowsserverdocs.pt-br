---
title: label
description: Artigo de referência para o comando de rótulo, que cria, altera ou exclui o rótulo de volume (ou seja, o nome) de um disco.
ms.topic: reference
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 486461059e90d0d1e1c6fa413e6db595f82924bb
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028194"
---
# <a name="label"></a>label

Cria, altera ou exclui o rótulo de volume (ou seja, o nome) de um disco. Se usado sem parâmetros, o comando **Label** altera o rótulo do volume atual ou exclui o rótulo existente.

## <a name="syntax"></a>Sintaxe

```
label [/mp] [<volume>] [<label>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /mp | Especifica que o volume deve ser tratado como um ponto de montagem ou nome de volume. |
| `<volume>` | Especifica uma letra da unidade (seguida por dois-pontos), ponto de montagem ou nome do volume. Se um nome de volume for especificado, o parâmetro **/MP** será desnecessário. |
| `<label>` | Especifica o rótulo para o volume. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

- O Windows exibe o rótulo de volume e o número de série (se tiver um) como parte da listagem de diretório.

- Um rótulo de volume NTFS pode ter até 32 caracteres de comprimento, incluindo espaços. Os rótulos de volume NTFS retêm e exibem o caso que foi usado quando o rótulo foi criado.

## <a name="examples"></a>Exemplos

Para rotular um disco na unidade A que contém informações de vendas de julho, digite:

```
label a:sales-july
```

Para exibir e excluir o rótulo atual da unidade C, siga estas etapas:

1. No prompt de comando, digite:

   ```
   label
   ```

   Uma saída semelhante à seguinte deve ser exibida:

   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```

2. Pressione ENTER. O prompt a seguir deve ser exibido:

   ```
   Delete current volume label (Y/N)?
   ```

3. Pressione **Y** para excluir o rótulo atual ou **N** se desejar manter o rótulo existente.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)