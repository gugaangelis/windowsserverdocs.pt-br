---
title: clean
description: Artigo de referência para o comando de limpeza do DiskPart, que remove todas as partições ou a formatação de volume do disco com foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a30e1f765959ed60efa662301f95defc21d6587
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929902"
---
# <a name="clean"></a>clean

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove todas as partições ou a formatação de volume do disco com foco.

>[!NOTE]
> Para obter uma versão do PowerShell desse comando, consulte [comando clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk).

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

- [apagar-comando de disco](https://docs.microsoft.com/powershell/module/storage/clear-disk)

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
