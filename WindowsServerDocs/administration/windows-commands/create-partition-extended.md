---
title: Criar partição estendida
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a1cca93a064cfb6e5c18f4a472ea837b922d07b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434197"
---
# <a name="create-partition-extended"></a>Criar partição estendida

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria uma partição estendida no disco com foco. Você pode usar esse comando somente no registro mestre de inicialização \(MBR\) discos.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                                                             Descrição                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |                                                  Especifica o tamanho da partição em megabytes \(MB\). Se nenhum tamanho for especificado, a partição continuará até que haja espaço livre não mais na partição estendida.                                                  |
| offset\=<n> |                     Especifica o deslocamento, em quilobytes \(KB\), no qual a partição é criada. Se o deslocamento não for especificado, a partição será iniciado no início do espaço livre no disco que seja grande o suficiente para reter a nova partição.                      |
| align\=<n>  | Alinha-se todas as extensões de partição para o limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica do RAID de hardware \(LUN\) matrizes para melhorar o desempenho. <n> é o número de kilobytes \(KB\) desde o início do disco para o limite de alinhamento mais próximo. |
|    noerr    |                                 Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.                                 |
  
## <a name="remarks"></a>Comentários  
  
-   Depois que a partição tiver sido criada, o foco mudará automaticamente para a nova partição.  
  
-   Apenas uma partição estendida pode ser criada por disco.  
  
-   Este comando falhará se você tentar criar uma partição estendida em outra partição estendida.  
  
-   Você deve criar uma partição estendida antes de criar unidades lógicas.  
  
-   Um disco MBR básico deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para criar uma partição estendida de 1000 megabytes em tamanho, digite:  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

