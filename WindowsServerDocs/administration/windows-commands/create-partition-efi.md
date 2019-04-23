---
title: Criar partição efi
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3714cfe52aafd4a602346139552b6712dbbc98c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878217"
---
# <a name="create-partition-efi"></a>Criar partição efi

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em Itanium\-computadores com, cria uma Interface de Firmware extensível \(EFI\) partição do sistema em uma tabela de partição GUID \(gpt\) disco.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|size\=<n>|O tamanho da partição em megabytes \(MB\). Se nenhum tamanho for especificado, a partição continuará até que haja espaço livre não mais na região atual.|  
|offset\=<n>|O deslocamento, em quilobytes \(KB\), no qual a partição é criada. Se o deslocamento não for especificado, a partição será colocada na primeira extensão de disco que seja grande o suficiente para contê-la.|  
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   Depois que a partição tiver sido criada, recebe o foco para a nova partição.  
  
-   Um disco gpt deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para criar uma partição EFI de 1000 megabytes no disco selecionado, digite:  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

