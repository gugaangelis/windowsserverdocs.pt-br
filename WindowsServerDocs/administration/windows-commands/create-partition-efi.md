---
title: criar partição EFI
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 76d97129fd67345f23eee2fc7b300493a1632cc6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379012"
---
# <a name="create-partition-efi"></a>criar partição EFI

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em computadores com Itanium @ no__t-0based, cria uma interface de firmware extensível \(EFI @ no__t-2 partição do sistema em uma tabela de partição GUID \(gpt @ no__t-4 disco.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                             Descrição                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamanho @ no__t-0 @ no__t-1  |                         O tamanho da partição em megabytes \(MB @ no__t-1. Se nenhum tamanho for fornecido, a partição continuará até que não haja mais espaço livre na região atual.                         |
| deslocamento @ no__t-0 @ no__t-1 |             O deslocamento em kilobytes \(KB @ no__t-1, no qual a partição é criada. Se nenhum deslocamento for fornecido, a partição será colocada na primeira extensão de disco grande o suficiente para contê-la.              |
|    NOERR    | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |
  
## <a name="remarks"></a>Comentários  
  
-   Depois que a partição tiver sido criada, o foco será fornecido para a nova partição.  
  
-   Um disco GPT deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.  
  
## <a name="BKMK_examples"></a>Disso  
Para criar uma partição EFI de 1000 megabytes no disco selecionado, digite:  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

