---
title: select disk
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6e8352b45cf3a46f14828c9c6796e4ec73499d5a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858957"
---
# <a name="select-disk"></a>select disk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleciona o disco especificado e desloca o foco para ele.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> O **<disk path>**, **sistema**, e **próximo** parâmetros só estão disponíveis no Windows 7 e Windows Server 2008 R2.  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|<n>|Especifica o número do disco para receber o foco. Você pode exibir os números para todos os discos no computador usando o **disco lista** comando DiskPart. **Observação:** Ao configurar sistemas com vários discos, não use **Selecionar disco\=0** para especificar o disco do sistema. O computador pode reatribuir a números de disco quando você reinicializa e computadores diferentes com a mesma configuração de disco podem ter números de disco diferente.|  
|<disk path>|Especifica o local do disco para receber o foco, por exemplo, **PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . Para exibir o caminho do local de um disco, selecione-o e, em seguida, digite **disco detalhes**.|  
|sistema|Em computadores com BIOS, especifica que esse disco 0 recebe o foco. Em computadores EFI, o disco que contém a partição do sistema EFI \(ESP\) que é usado para a inicialização atual recebe o foco. Em computadores EFI, o comando falhará se houver não poderes Paranormais, se houver mais de um ESP ou o computador é inicializado a partir do Windows Preinstallation Environment \(Windows PE\).|  
|próximo|Quando um disco for selecionado, esse comando itera em todos os discos na lista de discos. Quando você executar esse comando, o próximo disco na lista receberá o foco.|  
  
## <a name="BKMK_examples"></a>Exemplos  
Para deslocar o foco para o disco 1, digite:  
  
```  
select disk=1  
```  
  
Para selecionar um disco usando seu caminho de local, digite:  
  
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
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

