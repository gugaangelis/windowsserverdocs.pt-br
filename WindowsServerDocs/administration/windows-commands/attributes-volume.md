---
title: attributes volume
description: Artigo de referência do comando de volume Attributes, que exibe, define ou limpa os atributos de um volume.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aae7cc7fe26fac5ef03e40610eb46389eb274c94
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923879"
---
# <a name="attributes-volume"></a>attributes volume

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe, define ou limpa os atributos de um volume.

## <a name="syntax"></a>Sintaxe

```
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| set | Define o atributo especificado do volume com foco. |
| clear | Limpa o atributo especificado do volume com foco. |
| readonly | Especifica que o volume é somente leitura. |
| oculto | Especifica que o volume está oculto. |
| nodefaultdriveletter | Especifica que o volume não recebe uma letra de unidade por padrão. |
| shadowcopy | Especifica que o volume é um volume de cópia de sombra. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

### <a name="remarks"></a>Comentários

- Em discos básicos de MBR (registro mestre de inicialização), os parâmetros **Hidden**, **ReadOnly**e **nodefaultdriveletter** se aplicam a todos os volumes no disco.

- Em discos básicos de GPT (tabela de partição GUID) e em discos MBR e GPT dinâmicos, os parâmetros **Hidden**, **ReadOnly**e **nodefaultdriveletter** se aplicam somente ao volume selecionado.

- Um volume deve ser selecionado para que o comando de **volume de atributos** seja bem-sucedidos. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.

## <a name="examples"></a>Exemplos

Para exibir os atributos atuais no volume selecionado, digite:

```
attributes volume
```

Para definir o volume selecionado como oculto e somente leitura, digite:

```
attributes volume set hidden readonly
```

Para remover os atributos ocultos e somente leitura no volume selecionado, digite:

```
attributes volume clear hidden readonly
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Selecionar comando de volume](select-volume.md)
