---
title: Reparo
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1e4b9cde10e11558aaa95edda94921144dac1f86
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441807"
---
# <a name="repair"></a>Reparo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Repara o RAID\-volume 5 com foco, substituindo a região do disco com falha com o disco dinâmico especificado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
| Parâmetro  |                                                                                             Descrição                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disco\=<n>  |                                                                 Especifica o disco dinâmico que substituirá a região do disco com falha.                                                                 |
| align\=<n> |          Alinha-se todas as extensões de volume ou partição até o limite de alinhamento mais próximo. *n* é o número de kilobytes \(KB\) desde o início do disco para o limite de alinhamento mais próximo.           |
|   noerr    | Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro. |
  
## <a name="remarks"></a>Comentários  
  
-   O disco dinâmico especificado deve ter espaço livre maior ou igual ao tamanho total da região do disco com falha no RAID\-volume 5.  
  
-   Um volume em um RAID\-matriz 5 deve ser selecionado para essa operação seja bem-sucedida. Use o **selecionar volume** comando para selecionar um volume e mudar o foco a ele.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para substituir o volume com foco, substituindo-disco dinâmico 4, digite:  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

