---
title: select volume
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9337d7e4b37adcc22084249e53fb272335bf4f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834699"
---
# <a name="select-volume"></a>select volume

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona o volume especificado e desloca o foco para ele. Esse comando também pode ser usado para exibir o volume que atualmente tem o foco no disco selecionado.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select volume={<n>|<d>}  
```  
  
### <a name="parameters"></a>Parâmetros  
  
| Parâmetro |                                                                               Descrição                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | O número do volume para receber o foco. Você pode exibir os números de todos os volumes no disco selecionados no momento usando o comando **list volume** no DiskPart. |
|    <d>    |                                                 A letra da unidade ou o caminho do ponto de montagem do volume para receber o foco.                                                 |
  
## <a name="remarks"></a>Comentários  
  
-   Se nenhum volume for especificado, este comando exibirá o volume que atualmente tem o foco no disco selecionado.  
  
-   Em um disco básico, a seleção de um volume também fornece o foco para a partição correspondente.  
  
-   Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.  
  
-   se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para deslocar o foco para o volume 2, digite:  
  
```  
select volume=2  
```  
  
Para deslocar o foco para a unidade C, digite:  
  
```  
select volume=c  
```  
  
Para deslocar o foco para o volume montado em uma pasta chamada mountpath, digite:  
  
```  
select volume=c:\mountpath  
```  
  
Para exibir o volume que atualmente tem o foco no disco selecionado, digite:  
  
```  
select volume  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

