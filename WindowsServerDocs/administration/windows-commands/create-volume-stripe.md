---
title: Criar volume de distribuição
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 197864c6908645af0c0fa47ac811186207c2f247
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853877"
---
# <a name="create-volume-stripe"></a>Criar volume de distribuição

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria um volume distribuído usando dois ou mais discos dinâmicos especificados.  
  
> [!IMPORTANT]  
> para o Windows Vista, esse comando DiskPart só está disponível nas edições Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|size\=<n>|A quantidade de espaço em disco, em megabytes \(MB\), que o volume ocupará em cada disco. Se nenhum tamanho for especificado, o novo volume ocupará o espaço livre restante no menor disco e uma quantidade igual de espaço em cada disco subsequente.|  
|disco\=<n>,<n>\[,<n>,...\]|Os discos dinâmicos no qual o volume distribuído é criado. Você precisa de pelo menos dois discos dinâmicos para criar um volume distribuído. Uma quantidade de espaço igual a **tamanho\= <n>**  é alocada em cada disco.|  
|align\=<n>|Alinha-se todas as extensões de volume para o limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica do RAID de hardware \(LUN\) matrizes para melhorar o desempenho. *n* é o número de kilobytes \(KB\) desde o início do disco para o limite de alinhamento mais próximo.|  
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para criar um volume distribuído de 1000 megabytes em tamanho, em discos 1 e 2, digite:  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

