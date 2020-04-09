---
title: criar partição lógica
description: O tópico de comandos do Windows para criar partição lógica, que cria uma partição lógica em uma partição estendida existente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b16c161bf12476eee9d3959e5f313fd844ff3519
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847059"
---
# <a name="create-partition-logical"></a>criar partição lógica

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria uma partição lógica em uma partição estendida existente. Você só pode usar esse comando em discos MBR (registro mestre de inicialização).

## <a name="syntax"></a>Sintaxe  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                                                                                                                                                       Descrição                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamanho\=<n>  |                                                                                                              Especifica o tamanho da partição lógica em megabytes \(MB\), que deve ser menor do que a partição estendida. Se nenhum tamanho for fornecido, a partição continuará até que não haja mais espaço livre na partição estendida.                                                                                                               |
| \=de deslocamento <n> | Especifica o deslocamento em kilobytes \(KB\), no qual a partição é criada. O deslocamento é arredondado para preencher completamente qualquer tamanho de cilindro usado. Se nenhum deslocamento for fornecido, a partição será colocada na primeira extensão de disco grande o suficiente para contê-la. A partição é pelo menos tão longa em bytes quanto o número especificado pelo **tamanho\=<n>** . Se você especificar um tamanho para a partição lógica, ela deverá ser menor do que a partição estendida. |
| alinhar\=<n>  |                                                                                     Alinha todas as extensões de volume ou partição ao limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica RAID de hardware \(matrizes de\) LUN para melhorar o desempenho.  <n> é o número de kilobytes \(KB\) desde o início do disco até o limite de alinhamento mais próximo.                                                                                      |
|    NOERR    |                                                                                                                           somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                                                                                                           |
  
## <a name="remarks"></a>Comentários  
  
-   Se os parâmetros de **tamanho** e **deslocamento** não forem especificados, a partição lógica será criada na maior extensão de disco disponível na partição estendida.  
  
-   Depois que a partição tiver sido criada, o foco mudará automaticamente para a nova partição lógica.  
  
-   Um disco MBR básico deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para criar uma partição lógica de 1000 megabytes de tamanho, na partição estendida do disco selecionado, digite:  
  
```  
create partition logical size=1000  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

