---
title: clean
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f871ad1d13e06bf0cbb886ba64a52e7a55a9a797
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379317"
---
# <a name="clean"></a>clean

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando de limpeza do DiskPart remove toda a formatação de partição ou volume do disco com foco.
## <a name="syntax"></a>Sintaxe
```
clean [all]
```
## <a name="parameters"></a>Parâmetros

| Parâmetro |                                                        Descrição                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    all    | Especifica que cada setor do disco está definido como zero, o que exclui completamente todos os dados contidos no disco. |

## <a name="remarks"></a>Comentários
- Em discos MBR (registro mestre de inicialização), somente as informações de particionamento MBR e as informações do setor oculto são substituídas.
- Em discos de tabela de partição GUID (GPT), as informações de particionamento de GPT, incluindo o MBR de proteção, são substituídas. Não há informações de setor ocultos.
- Um disco deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.
  ## <a name="BKMK_examples"></a>Disso
  Para remover toda a formatação do disco selecionado, digite:
  ```
  clean
  ```

[Limpar disco](https://technet.microsoft.com/library/hh848661.aspx)
