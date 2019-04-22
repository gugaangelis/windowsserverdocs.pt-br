---
title: Selecione a partição
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7d5675aa6c33ddbe1e5e873e1a7cf7a2e8f8017
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824957"
---
# <a name="select-partition"></a>Selecione a partição

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleciona a partição especificada e desloca o foco para ele. Este comando também pode ser usado para exibir a partição que atualmente tem o foco no disco selecionado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|partição\=<n>|O número da partição para receber o foco. Você pode exibir os números de todas as partições do disco selecionado no momento usando o **lista a partição** comando DiskPart.|  
  
## <a name="remarks"></a>Comentários  
  
-   Antes de selecionar uma partição você deve primeiro selecionar um disco usando o **Selecionar disco** comando.  
  
-   Se nenhum número de partição for especificado, esse comando exibe a partição que atualmente tem o foco no disco selecionado.  
  
-   Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.  
  
-   Se uma partição é selecionada com um volume correspondente, o volume será selecionado automaticamente.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para deslocar o foco para a partição 3, digite:  
  
```  
select partitition=3  
```  
  
Para exibir a partição que atualmente tem o foco no disco selecionado, digite:  
  
```  
select partition  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

