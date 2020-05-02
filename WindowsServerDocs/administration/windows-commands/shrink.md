---
title: shrink
description: Tópico de referência para o DiskPart Shrink, que reduz o tamanho do volume selecionado pelo valor especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 357a2320bf8b26130c9aa148d513edff6f1e85db
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721797"
---
# <a name="shrink"></a>shrink

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando de redução do DiskPart reduz o tamanho do volume selecionado pelo valor especificado. Esse comando disponibiliza o espaço livre em disco do espaço não utilizado no final do volume.

## <a name="syntax"></a>Sintaxe
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
### <a name="parameters"></a>Parâmetros

|  Parâmetro  |                                                                                             Descrição                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desejado =<n> |                                                     Especifica a quantidade desejada de espaço em megabytes (MB) para reduzir o tamanho do volume pelo.                                                     |
| mínimo =<n> |                                                           Especifica a quantidade mínima de espaço em MB para reduzir o tamanho do volume.                                                           |
|  querymax   |                       Retorna a quantidade máxima de espaço em MB pela qual o volume pode ser reduzido. Esse valor poderá ser alterado se os aplicativos estiverem acessando o volume no momento.                        |
|   nowait    |                                                       força o comando a retornar imediatamente enquanto o processo de redução ainda está em andamento.                                                        |
|    NOERR    | somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="remarks"></a>Comentários
- Você pode reduzir o tamanho de um volume somente se ele estiver formatado usando o sistema de arquivos NTFS ou se ele não tiver um sistema de arquivos.
- Esse comando funciona em volumes básicos e em volumes dinâmicos simples ou estendidos.
- se um valor desejado não for especificado, o volume será reduzido pelo valor mínimo (se especificado).
- se uma quantidade mínima não for especificada, o volume será reduzido pelo valor desejado (se especificado).
- Se nenhum valor mínimo nem um valor desejado for especificado, o volume será reduzido o máximo possível.
- se uma quantidade mínima for especificada, mas não houver espaço livre suficiente disponível, o comando falhará.
- Um volume deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.
- Esse comando não funciona em partições de OEM (fabricante de equipamento original), partições de sistema de EFI (Extensible Firmware Interface) ou partições de recuperação.
  ## <a name="examples"></a>Exemplos
  Para reduzir o tamanho do volume selecionado pelo maior valor possível entre 250 e 500 megabytes, digite:
  ```
  shrink desired=500 minimum=250
  ```
  Para exibir o número máximo de MB pelo qual o volume pode ser reduzido, digite:
  ```
  shrink querymax
  ```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
