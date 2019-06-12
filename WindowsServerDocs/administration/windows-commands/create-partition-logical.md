---
title: Criar partição lógica
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3af60aed6c8305e410c6ebfba3cf2e006034ad7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434155"
---
# <a name="create-partition-logical"></a>Criar partição lógica

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria uma partição lógica em uma partição estendida existente. Você só pode usar esse comando no registro mestre de inicialização \(MBR\) discos.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                                                                                                                                                       Descrição                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |                                                                                                              Especifica o tamanho da partição lógica em megabytes \(MB\), que deve ser menor do que a partição estendida. Se nenhum tamanho for especificado, a partição continuará até que haja espaço livre não mais na partição estendida.                                                                                                               |
| offset\=<n> | Especifica o deslocamento, em quilobytes \(KB\), no qual a partição é criada. O deslocamento Arredonda para cima para preencher completamente todo o cilindro é usado. Se o deslocamento não for especificado, a partição é colocada na primeira extensão de disco que seja grande o suficiente para contê-la. A partição é pelo menos o mesmo tamanho em bytes, como o número especificado por **tamanho\=<n>** . Se você especificar um tamanho para a partição lógica, ele deve ser menor do que a partição estendida. |
| align\=<n>  |                                                                                     Alinha-se todas as extensões de volume ou partição até o limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica do RAID de hardware \(LUN\) matrizes para melhorar o desempenho.  <n> é o número de kilobytes \(KB\) desde o início do disco para o limite de alinhamento mais próximo.                                                                                      |
|    noerr    |                                                                                                                           Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.                                                                                                                           |
  
## <a name="remarks"></a>Comentários  
  
-   Se o **tamanho** e **deslocamento** parâmetros não forem especificados, a partição lógica é criada na maior extensão de disco disponível na partição estendida.  
  
-   Depois que a partição tiver sido criada, o foco mudará automaticamente para a nova partição lógica.  
  
-   Um disco MBR básico deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para criar uma partição lógica de 1000 megabytes em tamanho, em uma partição estendida do disco selecionado, digite:  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

