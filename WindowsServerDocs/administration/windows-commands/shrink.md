---
title: shrink
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09cc9ce7beebc04a0a45cd93c2b0da25c5a1a49e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441259"
---
# <a name="shrink"></a>shrink

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando do Diskpart redução reduz o tamanho do volume selecionado, o valor especificado. Esse comando faz o espaço livre em disco disponível do espaço não utilizado no final do volume.

## <a name="syntax"></a>Sintaxe
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
## <a name="parameters"></a>Parâmetros

|  Parâmetro  |                                                                                             Descrição                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desired=<n> |                                                     Especifica a quantidade de espaço em megabytes (MB) para reduzir o tamanho do volume desejada pelo.                                                     |
| minimum=<n> |                                                           Especifica a quantidade mínima de espaço em MB para reduzir o tamanho do volume por.                                                           |
|  querymax   |                       Retorna a quantidade máxima de espaço em MB, pelo qual o volume pode ser reduzido. Esse valor poderá ser alterado se a aplicativos que atualmente estão acessando o volume.                        |
|   nowait    |                                                       força o comando para retornar imediatamente, enquanto o processo de redução ainda está em andamento.                                                        |
|    noerr    | Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro. |

## <a name="remarks"></a>Comentários
- Você pode reduzir o tamanho de um volume apenas se ele é formatado usando o sistema de arquivos NTFS ou se não tiver um sistema de arquivos.
- Esse comando funciona em volumes básicos e em volumes dinâmicos simples ou estendidos.
- Se a quantidade desejada não for especificada, o volume será reduzido a quantidade mínima (se especificado).
- Se uma quantidade mínima não for especificada, o volume será reduzido a quantidade desejada (se especificado).
- Se nem uma quantidade mínima nem a quantidade desejada for especificada, o volume será reduzido tanto quanto possível.
- Se uma quantidade mínima é especificada, mas não há espaço livre está disponível, o comando falhará.
- Um volume deve ser selecionado para essa operação seja bem-sucedida. Use o **selecionar volume** comando para selecionar um volume e mudar o foco a ele.
- Esse comando não funciona em partições do fabricante original do equipamento (OEM), as partições de sistema Extensible Firmware Interface (EFI) ou partições de recuperação.
  ## <a name="BKMK_examples"></a>Exemplos
  Para reduzir o tamanho do volume selecionado, a maior quantidade possível entre 500 e 250 megabytes, digite:
  ```
  shrink desired=500 minimum=250
  ```
  Para exibir o número máximo de MB de volume pode ser reduzido em, digite:
  ```
  shrink querymax
  ```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
