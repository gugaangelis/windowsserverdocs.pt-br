---
title: Criar partição msr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79288fe90d037659f5e3934f1925dd8b7c21ad7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873427"
---
# <a name="create-partition-msr"></a>Criar partição msr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria um Microsoft Reserved \(MSR\) partição em uma tabela de partição GUID \(gpt\) disco.  
  
> [!CAUTION]  
> Tenha muito cuidado ao usar esse comando. Como os discos gpt exigem um layout de partição específica, a criação de partições Microsoft Reserved pode causar o disco ilegível.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|size\=<n>|O tamanho da partição em megabytes \(MB\). A partição é pelo menos o mesmo tamanho em bytes, como o número especificado por <n>. Se nenhum tamanho for especificado, a partição continuará até que haja espaço livre não mais na região atual.|  
|offset\=<n>|Especifica o deslocamento, em quilobytes \(KB\), no qual a partição é criada. O deslocamento Arredonda para cima para preencher completamente a qualquer tamanho de setor é usado. Se o deslocamento não for especificado, a partição será colocada na primeira extensão de disco que seja grande o suficiente para contê-la.|  
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   Em discos gpt que são usados para inicializar o sistema operacional Windows, a Interface de Firmware extensível \(EFI\) partição do sistema é a primeira partição no disco, seguida pela partição Microsoft Reserved. os discos GPT que são usados somente para armazenamento de dados não tem uma partição do sistema EFI, em cujo caso a partição Microsoft Reserved é a primeira partição.  
  
-   Windows não montagem partições Microsoft Reserved. Você não pode armazenar dados sobre eles e você não pode excluí-los.  
  
-   Uma partição Microsoft Reserved é necessária em cada disco gpt. O tamanho dessa partição depende do tamanho total do disco gpt. O tamanho do disco gpt deve ser pelo menos 32 MB para criar uma partição Microsoft Reserved.  
  
-   Um disco gpt básico deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco gpt básica e mudar o foco a ele.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para criar uma partição Microsoft Reserved de 1000 megabytes em tamanho, digite:  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

