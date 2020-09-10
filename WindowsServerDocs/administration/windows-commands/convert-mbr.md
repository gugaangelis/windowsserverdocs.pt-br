---
title: convert mbr
description: Artigo de referência para o comando Convert MBR, que converte um disco básico vazio com o estilo de partição GPT (tabela de partição GUID) em um disco básico com o estilo de partição MBR (registro mestre de inicialização).
ms.topic: reference
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4811bbd0aff1bb0087b5275a83695623e0cb34b6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629326"
---
# <a name="convert-mbr"></a>convert mbr

Converte um disco básico vazio com o estilo de partição GPT (tabela de partição GUID) em um disco básico com o estilo de partição MBR (registro mestre de inicialização). Um disco básico deve ser selecionado para que essa operação tenha sucesso. Use o [comando selecionar disco](select-disk.md) para selecionar um disco básico e deslocar o foco para ele.

> [!IMPORTANT]
> O disco deve estar vazio para convertê-lo em um disco básico. Faça backup dos dados e exclua todas as partições ou volumes antes de converter o disco.

> [!NOTE]
> Para obter instruções sobre como usar esse comando, consulte [alterar um disco de tabela de partição GUID em um disco de registro mestre de inicialização](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc725797(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
convert mbr [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para converter um disco básico do estilo de partição GPT em estilo de partição MBR, digite>:

```
convert mbr
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Convert](convert.md)
