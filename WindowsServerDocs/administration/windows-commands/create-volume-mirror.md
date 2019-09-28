---
title: criar espelho de volume
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72ecc4e0ede163857c47c5b7013aacdd49719ac8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378870"
---
# <a name="create-volume-mirror"></a>criar espelho de volume

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um espelho de volume usando os dois discos dinâmicos especificados.  
  
> [!NOTE]  
> Este comando só está disponível no Windows 7 e no Windows Server 2008 R2.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|         Parâmetro         |                                                                                                                                     Descrição                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         tamanho @ no__t-0 @ no__t-1         |                 Especifica a quantidade de espaço em disco, em megabytes \(MB @ no__t-1, que o volume ocupará em cada disco. Se nenhum tamanho for fornecido, o novo volume ocupará o espaço livre restante no menor disco e uma quantidade igual de espaço em cada disco subsequente.                 |
| disco @ no__t-0 @ no__t-1, <n> @ no__t-3, <n>,... \] |                       Especifica os discos dinâmicos nos quais o volume espelho é criado. Você precisa de dois discos dinâmicos para criar um volume de espelho. Uma quantidade de espaço igual ao tamanho especificado com o parâmetro de **tamanho** é alocada em cada disco.                        |
|        alinhar @ no__t-0 @ no__t-1         | Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Esse parâmetro é normalmente usado com o número de unidade lógica RAID de hardware \(LUN @ no__t-1 matrizes para melhorar o desempenho. *n* é o número de kilobytes \( KB @ no__t-2 desde o início do disco até o limite de alinhamento mais próximo. |
|           NOERR           |                                        Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um erro.                                         |
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="BKMK_examples"></a>Disso  
Para criar um volume espelhado de 1000 megabytes de tamanho, nos discos 1 e 2, digite:  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

