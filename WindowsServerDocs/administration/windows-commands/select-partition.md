---
title: selecionar partição
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9a186e2678fde64396a8b4b57a2d14e4b0b7bf26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371075"
---
# <a name="select-partition"></a>selecionar partição

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona a partição especificada e desloca o foco para ela. Esse comando também pode ser usado para exibir a partição que atualmente tem o foco no disco selecionado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|   Parâmetro    |                                                                                    Descrição                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \=de partição <n> | O número da partição que receberá o foco. Você pode exibir os números de todas as partições no disco selecionado no momento usando o comando **listar partição** no DiskPart. |
  
## <a name="remarks"></a>Comentários  
  
-   Para poder selecionar uma partição, primeiro você deve selecionar um disco usando o comando **selecionar disco** .  
  
-   Se nenhum número de partição for especificado, esse comando exibirá a partição que atualmente tem o foco no disco selecionado.  
  
-   se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.  
  
-   se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.  
  
## <a name="BKMK_examples"></a>Disso  
Para deslocar o foco para a partição 3, digite:  
  
```  
select partitition=3  
```  
  
Para exibir a partição que atualmente tem o foco no disco selecionado, digite:  
  
```  
select partition  
```  
  
#### <a name="additional-references"></a>referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

