---
title: corrige
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46b98938394c10e31d4999ff0e060e10f7da9bdc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835929"
---
# <a name="repair"></a>corrige

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Repara o volume RAID\-5 com foco, substituindo a região do disco com falha pelo disco dinâmico especificado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
| Parâmetro  |                                                                                             Descrição                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \=de disco <n>  |                                                                 Especifica o disco dinâmico que substituirá a região do disco com falha.                                                                 |
| alinhar\=<n> |          Alinha todas as extensões de volume ou partição ao limite de alinhamento mais próximo. *n* é o número de kilobytes \(KB\) desde o início do disco até o limite de alinhamento mais próximo.           |
|   NOERR    | somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |
  
## <a name="remarks"></a>Comentários  
  
-   O disco dinâmico especificado deve ter espaço livre maior ou igual ao tamanho total da região do disco com falha no volume RAID\-5.  
  
-   Um volume em uma matriz RAID\-5 deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para substituir o volume com foco, substituindo-o pelo disco dinâmico 4, digite:  
  
```  
repair disk=4  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

