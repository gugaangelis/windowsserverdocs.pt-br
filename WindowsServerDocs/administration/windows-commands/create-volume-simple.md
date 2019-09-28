---
title: criar volume simples
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1afb97c5bdb167eaf6ecfcd34ca3607b7b5a4c71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378884"
---
# <a name="create-volume-simple"></a>criar volume simples

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um volume simples no disco dinâmico especificado.  
  
> [!IMPORTANT]  
> para o Windows Vista, esse comando do DiskPart só está disponível nas edições Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
| Parâmetro  |                                                                                                                            Descrição                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tamanho @ no__t-0 @ no__t-1  |                                                                  O tamanho do volume em megabytes \(MB @ no__t-1. Se nenhum tamanho for fornecido, o novo volume ocupará o espaço livre restante no disco.                                                                   |
| disco @ no__t-0 @ no__t-1  |                                                                                O disco dinâmico no qual o volume é criado. Se nenhum disco for especificado, o disco atual será usado.                                                                                |
| alinhar @ no__t-0 @ no__t-1 | Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica RAID de hardware \(LUN @ no__t-1 matrizes para melhorar o desempenho. *n* é o número de kilobytes \( KB @ no__t-2 desde o início do disco até o limite de alinhamento mais próximo. |
|   NOERR    |                               Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                |
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="BKMK_examples"></a>Disso  
Para criar um volume de 1000 megabytes de tamanho, no disco 1, digite:  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

