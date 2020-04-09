---
title: selecionar partição
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97145d73cbbe1bdc9b27e545b047b78fe89e4984
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834799"
---
# <a name="select-partition"></a>selecionar partição

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona a partição especificada e desloca o foco para ela. Esse comando também pode ser usado para exibir a partição que atualmente tem o foco no disco selecionado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select partition=<n>  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|   Parâmetro    |                                                                                    Descrição                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \=de partição <n> | O número da partição que receberá o foco. Você pode exibir os números de todas as partições no disco selecionado no momento usando o comando **listar partição** no DiskPart. |
  
## <a name="remarks"></a>Comentários  
  
-   Para poder selecionar uma partição, primeiro você deve selecionar um disco usando o comando **selecionar disco** .  
  
-   Se nenhum número de partição for especificado, esse comando exibirá a partição que atualmente tem o foco no disco selecionado.  
  
-   Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.  
  
-   se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para deslocar o foco para a partição 3, digite:  
  
```  
select partitition=3  
```  
  
Para exibir a partição que atualmente tem o foco no disco selecionado, digite:  
  
```  
select partition  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

