---
title: criar volume simples
description: O tópico comandos do Windows para criar volume simples, que cria um volume simples no disco dinâmico especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45c707a92692c5531a0e33c9537705558f2ac309
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846889"
---
# <a name="create-volume-simple"></a>criar volume simples

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um volume simples no disco dinâmico especificado.  
  
> [!IMPORTANT]  
> para o Windows Vista, esse comando do DiskPart só está disponível nas edições Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business.
  
## <a name="syntax"></a>Sintaxe  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
| Parâmetro  |                                                                                                                            Descrição                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tamanho\=<n>  |                                                                  O tamanho do volume em megabytes \(MB\). Se nenhum tamanho for fornecido, o novo volume ocupará o espaço livre restante no disco.                                                                   |
| \=de disco <n>  |                                                                                O disco dinâmico no qual o volume é criado. Se nenhum disco for especificado, o disco atual será usado.                                                                                |
| alinhar\=<n> | Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica RAID de hardware \(matrizes de\) LUN para melhorar o desempenho. *n* é o número de kilobytes \(KB\) desde o início do disco até o limite de alinhamento mais próximo. |
|   NOERR    |                               somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                |
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar o volume, o foco mudará automaticamente para o novo volume.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para criar um volume de 1000 megabytes de tamanho, no disco 1, digite:  
  
```  
create volume simple size=1000 disk=1  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

