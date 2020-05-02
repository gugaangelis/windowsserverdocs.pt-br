---
title: corrige
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89c09608e64b408db9c7c79269046195e005c187
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722390"
---
# <a name="repair"></a>corrige

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Repara o volume\-RAID 5 com foco, substituindo a região do disco com falha pelo disco dinâmico especificado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
| Parâmetro  |                                                                                             Descrição                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disco\=<n>  |                                                                 Especifica o disco dinâmico que substituirá a região do disco com falha.                                                                 |
| alinha\=<n> |          Alinha todas as extensões de volume ou partição ao limite de alinhamento mais próximo. *n* é o número de kilobytes \(KB\) desde o início do disco até o limite de alinhamento mais próximo.           |
|   NOERR    | somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |
  
## <a name="remarks"></a>Comentários  
  
-   O disco dinâmico especificado deve ter espaço livre maior ou igual ao tamanho total da região do disco com falha no volume RAID\-5.  
  
-   Um volume em uma matriz\-RAID 5 deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.  
  
## <a name="examples"></a>Exemplos  
Para substituir o volume com foco, substituindo-o pelo disco dinâmico 4, digite:  
  
```  
repair disk=4  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

