---
title: clean
description: Artigo de referência para o comando de limpeza do DiskPart, que remove todas as partições ou a formatação de volume do disco com foco.
ms.topic: reference
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 82026520c456c76823993d34a4fb09bcd4b79808
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629649"
---
# <a name="clean"></a>clean

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove todas as partições ou a formatação de volume do disco com foco.

>[!NOTE]
> Para obter uma versão do PowerShell desse comando, consulte [comando clear-Disk](/powershell/module/storage/clear-disk).

## <a name="syntax"></a>Sintaxe

```
clean [all]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| all | Especifica que cada setor do disco está definido como zero, o que exclui completamente todos os dados contidos no disco. |

#### <a name="remarks"></a>Comentários

- Em discos MBR (registro mestre de inicialização), somente as informações de particionamento MBR e as informações do setor oculto são substituídas.

- Em discos de tabela de partição GUID (GPT), as informações de particionamento de GPT, incluindo o MBR de proteção, são substituídas. Não há informações de setor ocultos.

- Um disco deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="examples"></a>Exemplos

Para remover toda a formatação do disco selecionado, digite:

```
clean
```

## <a name="additional-references"></a>Referências adicionais

- [apagar-comando de disco](/powershell/module/storage/clear-disk)

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
