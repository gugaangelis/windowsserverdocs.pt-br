---
title: volume de atributos
description: Tópico de comandos do Windows para **atributos volume** – exibe, define ou limpa os atributos de um volume.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 225a10307123763d1a024fcc08fbae536fd0b5df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382577"
---
# <a name="attributes-volume"></a>volume de atributos

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe, define ou limpa os atributos de um volume.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|set|Define o atributo especificado do volume com foco.|  
|clear|Limpa o atributo especificado do volume com foco.|  
|readonly|Especifica que o volume é somente leitura\-.|  
|oculto|Especifica que o volume está oculto.|  
|nodefaultdriveletter|Especifica que o volume não recebe uma letra de unidade por padrão.|  
|shadowcopy|Especifica que o volume é um volume de cópia de sombra.|  
|NOERR|somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   No registro de inicialização mestre básico \(discos\) MBR, os parâmetros **Hidden**, **ReadOnly**e **nodefaultdriveletter** se aplicam a todos os volumes no disco.  
  
-   Na tabela de partição GUID básica \(discos\) GPT e em discos MBR e GPT dinâmicos, os parâmetros **Hidden**, **ReadOnly**e **nodefaultdriveletter** se aplicam somente ao volume selecionado.  
  
-   Um volume deve ser selecionado para que o comando de **volume de atributos** seja bem-sucedidos. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.  
  
## <a name="BKMK_examples"></a>Disso  
Para exibir os atributos atuais no volume selecionado, digite:  
  
```  
attributes volume  
```  
  
Para definir o volume selecionado como oculto e somente leitura\-, digite:  
  
```  
attributes volume set hidden readonly  
```  
  
Para remover os atributos oculto e de leitura somente\-no volume selecionado, digite:  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

