---
title: criar faixa de volume
description: O tópico de comandos do Windows para criar distribuição de volume, que cria um volume distribuído usando dois ou mais discos dinâmicos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8123d2b5852f606398e3be7161ef0341698b7fb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846899"
---
# <a name="create-volume-stripe"></a>criar faixa de volume

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um volume distribuído usando dois ou mais discos dinâmicos especificados.  
  
> [!IMPORTANT]  
> para o Windows Vista, esse comando do DiskPart só está disponível nas edições Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business.

## <a name="syntax"></a>Sintaxe  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|         Parâmetro         |                                                                                                                            Descrição                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         tamanho\=<n>         |             A quantidade de espaço em disco, em megabytes \(MB\), que o volume ocupará em cada disco. Se nenhum tamanho for fornecido, o novo volume ocupará o espaço livre restante no menor disco e uma quantidade igual de espaço em cada disco subsequente.             |
| \=de disco <n>,<n>\[,<n>,...\] |                                  Os discos dinâmicos nos quais o volume distribuído é criado. Você precisa de pelo menos dois discos dinâmicos para criar um volume distribuído. Uma quantidade de espaço igual ao **tamanho\=<n>** é alocada em cada disco.                                   |
|        alinhar\=<n>         | Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica RAID de hardware \(matrizes de\) LUN para melhorar o desempenho. *n* é o número de kilobytes \(KB\) desde o início do disco até o limite de alinhamento mais próximo. |
|           NOERR           |                               somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                |
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para criar um volume distribuído de 1000 megabytes de tamanho, nos discos 1 e 2, digite:  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

