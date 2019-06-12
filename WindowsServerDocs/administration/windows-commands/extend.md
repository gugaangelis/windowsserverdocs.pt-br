---
title: extend
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fdf070a733392d89bafe5bed5a1bf23d8e24d57
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439356"
---
# <a name="extend"></a>extend

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

estende o volume ou partição com foco e seu sistema de arquivos para livre \(não alocado\) espaço em um disco.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
| Parâmetro  |                                                                                             Descrição                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| size\=<n>  |      Especifica a quantidade de espaço em megabytes \(MB\) para adicionar ao volume atual ou partição. Se nenhum tamanho for especificado, todo o espaço livre contíguo disponível no disco será usado.       |
| disco\=<n>  |                          Especifica o disco no qual o volume ou partição é estendida. Se nenhum for especificado, o volume ou partição é estendida no disco atual.                          |
| sistema de arquivos |                                   estende o sistema de arquivos do volume com o foco. Para uso somente em discos em que o sistema de arquivos não foi estendido com o volume.                                    |
|   noerr    | Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro. |
  
## <a name="remarks"></a>Comentários  
  
-   Em discos básicos, o espaço livre deve ser no mesmo disco que o volume ou partição com foco. Ele também imediatamente deve seguir o volume ou partição com foco \(ou seja, ele deve começar no próximo deslocamento setor\).  
  
-   Em discos dinâmicos com volumes simples ou estendidos, um volume pode ser estendido para qualquer espaço livre em qualquer disco dinâmico. Usando este comando, você pode converter um volume dinâmico simples em um volume dinâmico estendido. Espelhado RAID\-volumes distribuídos e 5 não podem ser estendidos.  
  
-   Se a partição tiver sido formatada anteriormente com o sistema de arquivos NTFS, o sistema de arquivos será estendido automaticamente para preencher a partição maior e nenhuma perda de dados.  
  
-   Se a partição tiver sido formatada anteriormente com um sistema de arquivos diferentes de NTFS, o comando falhará sem alterar a partição.  
  
-   Se a partição não tiver sido formatada anteriormente com um sistema de arquivos, a partição ainda será estendida.  
  
-   A partição deve ter um volume associado antes que ele pode ser estendido.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para estender o volume ou partição com foco por 500 megabytes no disco 3, digite:  
  
```  
extend size=500 disk=3  
```  
  
Para estender o sistema de arquivos de um volume depois que ele foi estendido, digite:  
  
```  
extend filesystem  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

