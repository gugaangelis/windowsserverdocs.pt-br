---
title: criar um espelho de volume
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c66f21f55201d9d784b1ab0d7b729bc272589e5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822967"
---
# <a name="create-volume-mirror"></a>criar um espelho de volume

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria um espelho de volume usando os dois discos dinâmicos especificados.  
  
> [!NOTE]  
> Esse comando só está disponível no Windows 7 e Windows Server 2008 R2.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|size\=<n>|Especifica a quantidade de espaço em disco, em megabytes \(MB\), que o volume ocupará em cada disco. Se nenhum tamanho for especificado, o novo volume ocupará o espaço livre restante no menor disco e uma quantidade igual de espaço em cada disco subsequente.|  
|disco\=<n>,<n>\[,<n>,...\]|Especifica os discos dinâmicos nos quais o volume de espelho é criado. Você precisa de dois discos dinâmicos para criar um volume espelhado. Uma quantidade de espaço que é igual ao tamanho especificado com o **tamanho** parâmetro é alocado em cada disco.|  
|align\=<n>|Alinha-se todas as extensões de volume para o limite de alinhamento mais próximo. Normalmente, esse parâmetro é usado com o número de unidade lógica de RAID de hardware \(LUN\) matrizes para melhorar o desempenho. *n* é o número de kilobytes \(KB\) desde o início do disco para o limite de alinhamento mais próximo.|  
|noerr|Usado somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para criar um volume espelhado de 1000 megabytes em tamanho, em discos 1 e 2, digite:  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

