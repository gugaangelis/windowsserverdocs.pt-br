---
title: select disk
description: Artigo de referência para * * * *-
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1aa57e2ad077c1f109ecb68ecde3c0433664dae
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882869"
---
# <a name="select-disk"></a>select disk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona o disco especificado e desloca o foco para ele.



## <a name="syntax"></a>Sintaxe

```
select disk={ <n> | <disk path> | system | next }
```

> [!NOTE]
> Os **<disk path>** parâmetros, **System**e **Next** estão disponíveis somente no Windows 7 e no Windows Server 2008 R2.

### <a name="parameters"></a>Parâmetros

|  Parâmetro  |                                                                                                                                                                                                            Descrição                                                                                                                                                                                                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <n>     | Especifica o número do disco para receber o foco. Você pode exibir os números de todos os discos no computador usando o comando **listar disco** no DiskPart. **Observação:** Ao configurar sistemas com vários discos, não use **selecionar disco \= 0** para especificar o disco do sistema. O computador pode reatribuir números de disco quando você reinicializa, e computadores diferentes com a mesma configuração de disco podem ter diferentes números de disco. |
| <disk path> |                                                                                                                 Especifica o local do disco para receber o foco, por exemplo, **PCIROOT \( 0 \) \# PCI \( 0F02 \) \# atA \( C00T00L00 \) **. Para exibir o caminho do local de um disco, selecione-o e digite **disco de detalhes**.                                                                                                                  |
|   sistema    |                                 Em computadores BIOS, especifica que o disco 0 recebe foco. Em computadores EFI, o disco que contém a partição do sistema EFI \( ESP \) que é usado para a inicialização atual recebe o foco. Em computadores EFI, o comando falhará se não houver nenhum ESP, se houver mais de um ESP ou se o computador for inicializado a partir de Ambiente de Pré-Instalação do Windows o \( Windows PE \) .                                  |
|    Próximo     |                                                                                                                                     Depois que um disco é selecionado, esse comando faz a iteração em todos os discos na lista de discos. Quando você executar esse comando, o foco do próximo disco na lista será exibido.                                                                                                                                      |

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




