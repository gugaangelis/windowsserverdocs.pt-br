---
title: disco de atributos
description: Tópico de referência do comando atributos de disco, que exibe, define ou limpa os atributos de um disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3d378439b30328e4df48020fa4b3288f7af31c6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718892"
---
# <a name="attributes-disk"></a>disco de atributos

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
