---
title: criar espelho de volume
description: O tópico de comandos do Windows para criar espelhamento de volume, que cria um espelho de volume usando os dois discos dinâmicos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa5ea72eb0edacb841f32126f31a257b0573dd6d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846969"
---
# <a name="create-volume-mirror"></a>criar espelho de volume

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um espelho de volume usando os dois discos dinâmicos especificados.  
  
> [!NOTE]  
> Este comando só está disponível no Windows 7 e no Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxe  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|         Parâmetro         |                                                                                                                                     Descrição                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         tamanho\=<n>         |                 Especifica a quantidade de espaço em disco, em megabytes \(MB\), que o volume ocupará em cada disco. Se nenhum tamanho for fornecido, o novo volume ocupará o espaço livre restante no menor disco e uma quantidade igual de espaço em cada disco subsequente.                 |
| \=de disco <n>,<n>\[,<n>,...\] |                       Especifica os discos dinâmicos nos quais o volume espelho é criado. Você precisa de dois discos dinâmicos para criar um volume de espelho. Uma quantidade de espaço igual ao tamanho especificado com o parâmetro de **tamanho** é alocada em cada disco.                        |
|        alinhar\=<n>         | Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Esse parâmetro é normalmente usado com o número de unidade lógica RAID de hardware \(matrizes de\) LUN para melhorar o desempenho. *n* é o número de kilobytes \(KB\) desde o início do disco até o limite de alinhamento mais próximo. |
|           NOERR           |                                        Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um erro.                                         |
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para criar um volume espelhado de 1000 megabytes de tamanho, nos discos 1 e 2, digite:  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

