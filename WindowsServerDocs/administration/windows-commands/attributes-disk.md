---
title: attributes disk
description: Artigo de referência do comando atributos Disk, que exibe, define ou limpa os atributos de um disco.
ms.topic: reference
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72b4a99c36085062441a7a8751a065dad1dd0c51
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029194"
---
# <a name="attributes-disk"></a>attributes disk

Exibe, define ou limpa os atributos de um disco. Quando esse comando é usado para exibir os atributos atuais de um disco, o atributo de disco de inicialização denota o disco usado para iniciar o computador. Para um espelho dinâmico, ele exibe o disco que contém o Plex de inicialização do volume de inicialização.

> [!IMPORTANT]
> Um disco deve ser selecionado para que o comando **atributos de disco** tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
attributes disk [{set | clear}] [readonly] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| set | Define o atributo especificado do disco com foco. |
| clear | Limpa o atributo especificado do disco com foco. |
| readonly | Especifica que o disco é somente leitura. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para exibir os atributos do disco selecionado, digite:

```
attributes disk
```

Para definir o disco selecionado como somente leitura, digite:

```
attributes disk set readonly
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Selecionar comando de disco](select-disk.md)
