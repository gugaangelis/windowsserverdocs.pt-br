---
title: select disk
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d9078242264b01ee4bc24dc590df24b1e53e548
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371080"
---
# <a name="select-disk"></a>select disk

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona o disco especificado e desloca o foco para ele.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> Os parâmetros **<disk path>** , **System**e **Next** estão disponíveis somente no windows 7 e no Windows Server 2008 R2.  
  
## <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                                                                                                                                            Descrição                                                                                                                                                                                                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <n>     | Especifica o número do disco para receber o foco. Você pode exibir os números de todos os discos no computador usando o comando **listar disco** no DiskPart. **Observação:** Ao configurar sistemas com vários discos, não use **selecionar disco\=0** para especificar o disco do sistema. O computador pode reatribuir números de disco quando você reinicializa, e computadores diferentes com a mesma configuração de disco podem ter diferentes números de disco. |
| <disk path> |                                                                                                                 Especifica o local do disco para receber o foco, por exemplo, **PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . Para exibir o caminho do local de um disco, selecione-o e digite **disco de detalhes**.                                                                                                                  |
|   sistema    |                                 Em computadores BIOS, especifica que o disco 0 recebe foco. Em computadores EFI, o disco que contém a partição do sistema EFI \(\) do ESP usado para a inicialização atual recebe o foco. Em computadores EFI, o comando falhará se não houver nenhum ESP, se houver mais de um ESP ou se o computador for inicializado a partir de Ambiente de Pré-Instalação do Windows \(\)do Windows PE.                                  |
|    próximo     |                                                                                                                                     Depois que um disco é selecionado, esse comando faz a iteração em todos os discos na lista de discos. Quando você executar esse comando, o foco do próximo disco na lista será exibido.                                                                                                                                      |
  
## <a name="BKMK_examples"></a>Disso  
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
  
#### <a name="additional-references"></a>referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

