---
title: select vdisk
description: Artigo de referência para o comando SELECT VDISK, que seleciona o VHD (disco rígido virtual) especificado e desloca o foco para ele.
ms.topic: reference
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 19e842115f914711cc59cfb770ee19f732b3a388
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389202"
---
# <a name="select-vdisk"></a>select vdisk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleciona o VHD do disco rígido virtual especificado \( \) e desloca o foco para ele.

## <a name="syntax"></a>Sintaxe

```
select vdisk file=<full path> [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| arquivo =`<full path>` | Especifica o caminho completo e o nome de arquivo de um arquivo VHD existente. |
| NOERR | Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para deslocar o foco para o VHD denominado *c:\test\test.vhd*, digite:

```
select vdisk file=c:\test\test.vhd
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [attach vdisk](attach-vdisk.md)

- [compact vdisk](compact-vdisk.md)

- [detach vdisk](detach-vdisk.md)

- [detail vdisk](detail-vdisk.md)

- [expand vdisk](expand-vdisk.md)

- [merge vdisk](merge-vdisk.md)

- [list](list.md)

- [Selecionar comando de disco](select-disk.md)

- [Selecionar comando de partição](select-partition.md)

- [Selecionar comando de volume](select-volume.md)