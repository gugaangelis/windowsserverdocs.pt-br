---
title: clean
description: O tópico de comandos do Windows para o comando de limpeza do DiskPart, que remove toda e qualquer formatação de partição ou volume do disco com foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d573a0480c24a2a622618197dea7dccaeddac271
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847749"
---
# <a name="clean"></a>clean

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando de limpeza do DiskPart remove toda a formatação de partição ou volume do disco com foco.

## <a name="syntax"></a>Sintaxe
```
clean [all]
```
### <a name="parameters"></a>Parâmetros

| Parâmetro |                                                        Descrição                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    all    | Especifica que cada setor do disco está definido como zero, o que exclui completamente todos os dados contidos no disco. |

## <a name="remarks"></a>Comentários
- Em discos MBR (registro mestre de inicialização), somente as informações de particionamento MBR e as informações do setor oculto são substituídas.
- Em discos de tabela de partição GUID (GPT), as informações de particionamento de GPT, incluindo o MBR de proteção, são substituídas. Não há informações de setor ocultos.
- Um disco deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="examples"></a><a name=BKMK_examples></a>Disso
  Para remover toda a formatação do disco selecionado, digite:
  ```
  clean
  ```

## <a name="additional-references"></a>Referências adicionais
[Limpar disco](https://technet.microsoft.com/library/hh848661.aspx)
