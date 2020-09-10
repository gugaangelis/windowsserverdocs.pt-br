---
title: detach vdisk
description: Artigo de referência para o comando Detach VDISK, que impede que o VHD (disco rígido virtual) selecionado apareça como uma unidade de disco rígido local no computador host.
ms.topic: reference
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: af3608cfa3f92ecbda3c62401f5c5a85fa5a2ffe
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628765"
---
# <a name="detach-vdisk"></a>detach vdisk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompe a exibição do VHD (disco rígido virtual) selecionado como uma unidade de disco rígido local no computador host. Quando um VHD é desanexado, você pode copiá-lo a outros locais. Antes de começar, você deve selecionar um VHD para que essa operação seja realizada com sucesso. Use o comando [Select VDISK](select-vdisk.md) para selecionar um VHD e deslocar o foco para ele.


## <a name="syntax"></a>Sintaxe

```
detach vdisk [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para desanexar o VHD selecionado, digite:

```
detach vdisk
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando attach vdisk](attach-vdisk.md)

- [comando Compact vdisk](compact-vdisk.md)

- [comando VDISK do detalhe](detail-vdisk.md)

- [comando expandir vdisk](expand-vdisk.md)

- [Comando Merge vdisk](merge-vdisk.md)

- [Selecionar comando vdisk](select-vdisk.md)

- [comando de lista](list.md)
