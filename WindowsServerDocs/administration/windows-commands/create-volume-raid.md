---
title: criar RAID de volume
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a3c13cb5b78ae3e771b461a35a7130a48e7ec01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378856"
---
# <a name="create-volume-raid"></a>criar RAID de volume

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um volume RAID @ no__t-05 usando três ou mais discos dinâmicos especificados.  
  
> [!IMPORTANT]  
> Esse comando do DiskPart não está disponível em nenhuma edição do Windows Vista.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|           Parâmetro           |                                                                                                                                                                                                                                              Descrição                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           tamanho @ no__t-0 @ no__t-1           | A quantidade de espaço em disco, em megabytes \(MB @ no__t-1, que o volume ocupará em cada disco. Se nenhum tamanho for fornecido, o maior volume de RAID @ no__t-05 possível será criado. O disco com o menor espaço livre contíguo disponível determina o tamanho do volume RAID @ no__t-05 e a mesma quantidade de espaço é alocada de cada disco. A quantidade real de espaço em disco utilizável no volume RAID @ no__t-05 é menor do que a quantidade combinada de espaço em disco porque parte do espaço em disco é necessária para a paridade. |
| disco @ no__t-0 @ no__t-1, <n>, <n> @ no__t-4, <n>,... \] |                                                                                                                                               Os discos dinâmicos nos quais criar o volume RAID @ no__t-05. Você precisa de pelo menos três discos dinâmicos para criar um volume RAID @ no__t-05. Uma quantidade de espaço igual ao **tamanho @ no__t-1 @ no__t-2** é alocada em cada disco.                                                                                                                                                |
|          alinhar @ no__t-0 @ no__t-1           |                                                                                                                   Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica RAID de hardware \(LUN @ no__t-1 matrizes para melhorar o desempenho. *n* é o número de kilobytes \( KB @ no__t-2 desde o início do disco até o limite de alinhamento mais próximo.                                                                                                                   |
|             NOERR             |                                                                                                                                                 Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                                                                                                                                  |
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="BKMK_examples"></a>Disso  
Para criar um volume RAID @ no__t-05 de 1000 megabytes de tamanho, usando os discos 1, 2 e 3, digite:  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

