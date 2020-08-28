---
title: offline disk
description: Artigo de referência do comando de disco offline, que leva o disco online com foco para o estado offline.
ms.topic: reference
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 186acfaf297e6b94faf11a235f36082672524e75
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037244"
---
# <a name="offline-disk"></a>offline disk

Coloca o disco online com foco no estado offline. Se um disco dinâmico em um grupo de discos for colocado offline, o status do disco será alterado para **ausente** e o grupo mostrará um disco que está offline. O disco ausente é movido para o grupo inválido. Se o disco dinâmico for o último disco do grupo, o status do disco será alterado para **offline**e o grupo vazio será removido.

> [!NOTE]
> Um disco deve ser selecionado para que o comando de **disco offline** seja executado com sucesso. Use o comando [selecionar disco](select-disk.md) para selecionar um disco e deslocar o foco para ele.
>
> Esse comando também funciona em discos no modo SAN online, alterando o modo SAN para offline.

## <a name="syntax"></a>Sintaxe

```
offline disk [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

### <a name="examples"></a>Exemplos

Para colocar o disco com foco offline, digite:

```
offline disk
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
