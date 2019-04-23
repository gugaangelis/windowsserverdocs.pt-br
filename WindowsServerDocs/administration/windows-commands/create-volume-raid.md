---
title: Criar volume raid
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 432d661d8c0ce4cae6fe08a2671e8f9d613ce351
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846777"
---
# <a name="create-volume-raid"></a>Criar volume raid

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria um RAID\-especificado de volume 5 usando três ou mais discos dinâmicos.  
  
> [!IMPORTANT]  
> Esse comando DiskPart não está disponível em nenhuma edição do Windows Vista.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|size\=<n>|A quantidade de espaço em disco, em megabytes \(MB\), que o volume ocupará em cada disco. Se nenhum tamanho for fornecido, o maior possível RAID\-5 volume será criado. O disco com o menor espaço livre contíguo disponível determina o tamanho para o RAID\-volume 5 e a mesma quantidade de espaço é alocado em cada disco. A quantidade real de espaço em disco utilizável no RAID\-volume 5 é menor que a quantidade combinada de espaço em disco, como parte do espaço em disco é necessária para paridade.|  
|disco\=<n>,<n>,<n>\[,<n>,...\]|Os discos dinâmicos no qual criar o RAID\-volume 5. Você precisa de pelo menos três discos dinâmicos para criar um RAID\-volume 5. Uma quantidade de espaço igual a **tamanho\= <n>**  é alocada em cada disco.|  
|align\=<n>|Alinha-se todas as extensões de volume para o limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica do RAID de hardware \(LUN\) matrizes para melhorar o desempenho. *n* é o número de kilobytes \(KB\) desde o início do disco para o limite de alinhamento mais próximo.|  
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para criar um RAID\-volume 5 de 1000 MB, usando discos 1, 2 e 3, tipo:  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

