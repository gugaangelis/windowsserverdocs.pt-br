---
title: select volume
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: cc981131c8de2dc4534e390645ef45c39a7b02ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371056"
---
# <a name="select-volume"></a>select volume

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona o volume especificado e desloca o foco para ele. Esse comando também pode ser usado para exibir o volume que atualmente tem o foco no disco selecionado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select volume={<n>|<d>}  
```  
  
## <a name="parameters"></a>Parâmetros  
  
| Parâmetro |                                                                               Descrição                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | O número do volume para receber o foco. Você pode exibir os números de todos os volumes no disco selecionados no momento usando o comando **list volume** no DiskPart. |
|    <d>    |                                                 A letra da unidade ou o caminho do ponto de montagem do volume para receber o foco.                                                 |
  
## <a name="remarks"></a>Comentários  
  
-   Se nenhum volume for especificado, este comando exibirá o volume que atualmente tem o foco no disco selecionado.  
  
-   Em um disco básico, a seleção de um volume também fornece o foco para a partição correspondente.  
  
-   Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.  
  
-   Se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.  
  
## <a name="BKMK_examples"></a>Disso  
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
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

