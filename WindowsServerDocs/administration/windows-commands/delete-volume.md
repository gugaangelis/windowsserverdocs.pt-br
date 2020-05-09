---
title: delete volume
description: Tópico de referência para o comando excluir volume, que exclui o volume selecionado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59856e89ff96d2881040365d157540dc62c1aeb0
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993104"
---
# <a name="delete-volume"></a>delete volume

Exclui o volume selecionado. Antes de começar, você deve selecionar um volume para que essa operação seja realizada com sucesso. Use o comando [selecionar volume](select-volume.md) para selecionar um volume e deslocar o foco para ele.

> [!IMPORTANT]
> Não é possível excluir o volume do sistema, o volume de inicialização ou qualquer volume que contenha o arquivo de paginação ativo ou o despejo de memória (despejo).

## <a name="syntax"></a>Sintaxe

```
delete volume [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para excluir o volume com foco, digite:

```
delete volume
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [select volume](select-volume.md)

- [Excluir comando](delete.md)