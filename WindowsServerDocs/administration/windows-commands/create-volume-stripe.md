---
title: criar faixa de volume
description: Tópico de referência para criar faixa de volume, que cria um volume distribuído usando dois ou mais discos dinâmicos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98ca692d27135aa8ba4da0ff85ecd3074cdd73c4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716875"
---
# <a name="create-volume-stripe"></a>criar faixa de volume

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|         tamanho\=<n>         |             A quantidade de espaço em disco, em \(megabytes\)MB, que o volume ocupará em cada disco. Se nenhum tamanho for fornecido, o novo volume ocupará o espaço livre restante no menor disco e uma quantidade igual de espaço em cada disco subsequente.             |
| disco\=<n><n>,\[,<n>,...\] |                                  Os discos dinâmicos nos quais o volume distribuído é criado. Você precisa de pelo menos dois discos dinâmicos para criar um volume distribuído. Uma quantidade de espaço igual ao **tamanho\= ** é alocada em cada disco.                                   |
|        alinha\=<n>         | Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Normalmente usado com matrizes de LUN \(\) de número de unidade lógica RAID de hardware para melhorar o desempenho. *n* é o número de kilobytes \(KB\) desde o início do disco até o limite de alinhamento mais próximo. |
|           NOERR           |                               somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                |
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="examples"></a>Exemplos  
Para criar um volume distribuído de 1000 megabytes de tamanho, nos discos 1 e 2, digite:  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

