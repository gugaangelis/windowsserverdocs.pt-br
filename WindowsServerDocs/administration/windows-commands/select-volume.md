---
title: select volume
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0ebf9896621268c384ea8129d32c985028054d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890727"
---
# <a name="select-volume"></a>select volume

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleciona o volume especificado e desloca o foco para ele. Este comando também pode ser usado para exibir o volume que atualmente tem o foco no disco selecionado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select volume={<n>|<d>}  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|<n>|O número do volume para receber o foco. Você pode exibir os números para todos os volumes no disco selecionado no momento usando o **listar volume** comando DiskPart.|  
|<d>|A unidade montagem ou letra ponto de caminho do volume para receber o foco.|  
  
## <a name="remarks"></a>Comentários  
  
-   Se nenhum volume for especificado, esse comando exibe o volume que atualmente tem o foco no disco selecionado.  
  
-   Em um disco básico, seleção de um volume também fornece o foco para a partição correspondente.  
  
-   Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.  
  
-   Se uma partição é selecionada com um volume correspondente, o volume será selecionado automaticamente.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para deslocar o foco para o volume 2, digite:  
  
```  
select volume=2  
```  
  
Para deslocar o foco para a unidade C, digite:  
  
```  
select volume=c  
```  
  
Para deslocar o foco para o volume montado em uma pasta chamada "mountpath", digite:  
  
```  
select volume=c:\mountpath  
```  
  
Para exibir o volume que atualmente tem o foco no disco selecionado, digite:  
  
```  
select volume  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

