---
title: select disk
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 441e680f0a705255f6eba8c4a16db4a623e50b6b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834809"
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
  
### <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                                                                                                                                            Descrição                                                                                                                                                                                                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <n>     | Especifica o número do disco para receber o foco. Você pode exibir os números de todos os discos no computador usando o comando **listar disco** no DiskPart. **Observação:** Ao configurar sistemas com vários discos, não use **selecionar disco\=0** para especificar o disco do sistema. O computador pode reatribuir números de disco quando você reinicializa, e computadores diferentes com a mesma configuração de disco podem ter diferentes números de disco. |
| <disk path> |                                                                                                                 Especifica o local do disco para receber o foco, por exemplo, **PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . Para exibir o caminho do local de um disco, selecione-o e digite **disco de detalhes**.                                                                                                                  |
|   sistema    |                                 Em computadores BIOS, especifica que o disco 0 recebe foco. Em computadores EFI, o disco que contém a partição do sistema EFI \(\) do ESP usado para a inicialização atual recebe o foco. Em computadores EFI, o comando falhará se não houver nenhum ESP, se houver mais de um ESP ou se o computador for inicializado a partir de Ambiente de Pré-Instalação do Windows \(\)do Windows PE.                                  |
|    next     |                                                                                                                                     Depois que um disco é selecionado, esse comando faz a iteração em todos os discos na lista de discos. Quando você executar esse comando, o foco do próximo disco na lista será exibido.                                                                                                                                      |
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
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
  

  

