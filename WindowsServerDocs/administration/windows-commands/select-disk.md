---
title: select disk
description: Artigo de referência do comando selecionar disco, que seleciona o disco especificado e, em seguida, desloca o foco para ele.
ms.topic: reference
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 194bd36305060c8adfec27435ef05bf87605b45c
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389239"
---
# <a name="select-disk"></a>select disk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleciona o disco especificado e desloca o foco para ele.

## <a name="syntax"></a>Sintaxe

```
select disk={<n>|<disk path>|system|next}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<n>` | Especifica o número do disco para receber o foco. Você pode exibir os números de todos os discos no computador usando o comando **listar disco** no DiskPart.<p>**OBSERVAÇÃO**<br>Ao configurar sistemas com vários discos, não use **selecionar disco = 0** para especificar o disco do sistema. O computador pode reatribuir números de disco quando você reinicializa, e computadores diferentes com a mesma configuração de disco podem ter diferentes números de disco. |
| `<disk path>` | Especifica o local do disco para receber o foco, por exemplo, `PCIROOT(0)#PCI(0F02)#atA(C00T00L00)` . Para exibir o caminho do local de um disco, selecione-o e digite **disco de detalhes**. |
| sistema | Em computadores BIOS, essa opção especifica que o disco 0 recebe foco. Em computadores EFI, o disco que contém a partição do sistema EFI (ESP), usado para a inicialização atual, recebe o foco. Em computadores EFI, o comando falhará se não houver nenhum ESP, se houver mais de um ESP ou se o computador for inicializado a partir de Ambiente de Pré-Instalação do Windows (Windows PE). |
| Próximo | Depois que um disco é selecionado, essa opção itera em todos os discos na lista de discos. Quando você executa essa opção, o próximo disco da lista recebe o foco. |

## <a name="examples"></a>Exemplos

Para deslocar o foco para o disco 1, digite:

```
select disk=1
```

Para selecionar um disco usando seu caminho de localização, digite:

```
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)
```

Para deslocar o foco para o disco do sistema, digite:

```
select disk=system
```

Para deslocar o foco para o próximo disco no computador, digite:

```
select disk=next
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Selecionar comando de partição](select-partition.md)

- [Selecionar comando vdisk](select-vdisk.md)

- [Selecionar comando de volume](select-volume.md)
