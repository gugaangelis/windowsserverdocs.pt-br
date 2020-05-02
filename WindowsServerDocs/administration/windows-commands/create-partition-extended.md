---
title: criar partição estendida
description: Tópico de referência para Create Partition Extended, que cria uma partição estendida no disco com foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b8af7247a9084b722f5b510df1d6af4622fc4ac2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719244"
---
# <a name="create-partition-extended"></a>criar partição estendida

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria uma partição estendida no disco com foco. Você pode usar esse comando somente em discos MBR (registro mestre de inicialização).

## <a name="syntax"></a>Sintaxe  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                                                             Descrição                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamanho\=<n>  |                                                  Especifica o tamanho da partição em megabytes \(MB.\) Se nenhum tamanho for fornecido, a partição continuará até que não haja mais espaço livre na partição estendida.                                                  |
| desvio\=<n> |                     Especifica o deslocamento em kilobytes \(KB\), no qual a partição é criada. Se nenhum deslocamento for fornecido, a partição será iniciada no início do espaço livre no disco que é grande o suficiente para manter a nova partição.                      |
| alinha\=<n>  | Alinha todas as extensões de partição com o limite de alinhamento mais próximo. Normalmente usado com matrizes de LUN \(\) de número de unidade lógica RAID de hardware para melhorar o desempenho. <n>é o número de kilobytes \(KB\) desde o início do disco até o limite de alinhamento mais próximo. |
|    NOERR    |                                 somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                 |
  
## <a name="remarks"></a>Comentários  
  
-   Depois que a partição tiver sido criada, o foco mudará automaticamente para a nova partição.  
  
-   Somente uma partição estendida pode ser criada por disco.  
  
-   Esse comando falhará se você tentar criar uma partição estendida dentro de outra partição estendida.  
  
-   Você deve criar uma partição estendida antes de poder criar unidades lógicas.  
  
-   Um disco MBR básico deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.  
  
## <a name="examples"></a>Exemplos  
Para criar uma partição estendida de 1000 megabytes de tamanho, digite:  
  
```  
create partition extended size=1000  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

