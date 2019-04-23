---
title: volume de atributos
description: Tópico de comandos do Windows para **atributos de volume** -exibe, define ou limpa os atributos de um volume.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37af55ee2a041fbcf8068e0def72147732d3a687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846577"
---
# <a name="attributes-volume"></a>volume de atributos

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe, define ou limpa os atributos de um volume.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|set|Define o atributo especificado do volume com o foco.|  
|clear|Limpa o atributo especificado do volume com o foco.|  
|somente leitura|Especifica que o volume é ler\-apenas.|  
|Oculto|Especifica que o volume está oculta.|  
|NODEFAULTDRIVELETTER|Especifica que o volume não recebe uma letra de unidade por padrão.|  
|cópia de sombra|Especifica que o volume é um volume de cópia de sombra.|  
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   No registro mestre de inicialização básica \(MBR\) discos, o **oculto**, **readonly**, e **nodefaultdriveletter** parâmetros se aplicam a todos os volumes no o disco.  
  
-   Na tabela de partição GUID básica \(gpt\) discos e em dinâmico discos MBR e gpt, o **oculto**, **readonly**, e **nodefaultdriveletter** parâmetros se aplicam apenas ao volume selecionado.  
  
-   Um volume deve ser selecionado para o **atributos de volume** comando tenha êxito. Use o **selecionar volume** comando para selecionar um volume e mudar o foco a ele.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para exibir os atributos atuais no volume selecionado, digite:  
  
```  
attributes volume  
```  
  
Para definir o volume selecionado como oculto e leitura\-apenas, digite:  
  
```  
attributes volume set hidden readonly  
```  
  
Para remover as ocultas e leitura\-apenas os atributos no volume selecionado, digite:  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

