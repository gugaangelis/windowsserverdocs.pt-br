---
title: load metadata
description: Artigo de referência para o comando carregar metadados, que carrega um arquivo Metadata. cab antes de importar uma cópia de sombra transportável ou carrega os metadados do gravador no caso de uma restauração.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01e782d0214da70f831b81120aff3c5097895036
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931682"
---
# <a name="load-metadata"></a>Carregar metadados

Carrega um arquivo Metadata. cab antes de importar uma cópia de sombra transportável ou carrega os metadados do gravador no caso de uma restauração. Se usado sem parâmetros, **carregar metadados** exibe a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
load metadata [<drive>:][<path>]<metadata.cab>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `[<drive>:][<path>]` | Especifica o local do arquivo de metadados. |
| metadata.cab | Especifica o arquivo Metadata. cab a ser carregado. |

## <a name="remarks"></a>Comentários

- Você pode usar o comando de **importação** para importar uma cópia de sombra transportável com base nos metadados especificados pelos **metadados de carga**.

- Você deve executar esse comando antes do comando **Iniciar restauração** , para carregar os gravadores e componentes selecionados para a restauração.

## <a name="examples"></a>Exemplos

Para carregar um arquivo de metadados chamado metafile.cab do local padrão, digite:

```
load metadata metafile.cab
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [importar comando DiskShadow](import.md)

- [comando Iniciar restauração](begin-restore.md)
