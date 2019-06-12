---
title: Criar volume simples
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a35d0de5110c0e1616c42921c8402ecc1aff8c41
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434057"
---
# <a name="create-volume-simple"></a>Criar volume simples

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria um volume simples no disco dinâmico especificado.  
  
> [!IMPORTANT]  
> para o Windows Vista, esse comando DiskPart só está disponível nas edições Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
| Parâmetro  |                                                                                                                            Descrição                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| size\=<n>  |                                                                  O tamanho do volume em megabytes \(MB\). Se nenhum tamanho for especificado, o novo volume ocupará o espaço livre restante no disco.                                                                   |
| disco\=<n>  |                                                                                O disco dinâmico em que o volume é criado. Se nenhum for especificado, o disco atual será usado.                                                                                |
| align\=<n> | Alinha-se todas as extensões de volume para o limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica do RAID de hardware \(LUN\) matrizes para melhorar o desempenho. *n* é o número de kilobytes \(KB\) desde o início do disco para o limite de alinhamento mais próximo. |
|   noerr    |                               Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.                                |
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para criar um volume de 1000 megabytes em tamanho, em disco 1, digite:  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

