---
title: Corrige
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88293422519488405d94e32596c81dbe4a697dee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371532"
---
# <a name="repair"></a>Corrige

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Repara o volume RAID @ no__t-05 com foco, substituindo a região do disco com falha pelo disco dinâmico especificado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
| Parâmetro  |                                                                                             Descrição                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disco @ no__t-0 @ no__t-1  |                                                                 Especifica o disco dinâmico que substituirá a região do disco com falha.                                                                 |
| alinhar @ no__t-0 @ no__t-1 |          Alinha todas as extensões de volume ou partição ao limite de alinhamento mais próximo. *n* é o número de kilobytes \( KB @ no__t-2 desde o início do disco até o limite de alinhamento mais próximo.           |
|   NOERR    | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |
  
## <a name="remarks"></a>Comentários  
  
-   O disco dinâmico especificado deve ter espaço livre maior ou igual ao tamanho total da região do disco com falha no volume RAID @ no__t-05.  
  
-   Um volume em uma matriz RAID @ no__t-05 deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.  
  
## <a name="BKMK_examples"></a>Disso  
Para substituir o volume com foco, substituindo-o pelo disco dinâmico 4, digite:  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

