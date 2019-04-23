---
title: Selecionar o vdisk
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a71a5c15c05a1e969d0720bc8e67e669d553f649
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852567"
---
# <a name="select-vdisk"></a>Selecionar o vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleciona o disco rígido virtual especificado \(VHD\) e desloca o foco para ele.  
  
> [!NOTE]  
> Este comando só é aplicável ao Windows 7 e Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|file\=<full path>|Especifica o nome de arquivo e caminho completo de um arquivo VHD existente.|  
|noerr|Usado somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|  
  
## <a name="BKMK_examples"></a>Exemplos  
Para deslocar o foco para o VHD chamado Test. vhd, digite:  
  
```  
select vdisk file="c:\test\test.vhd"  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [Compactar vdisk](compact-vdisk.md)  
  
  
  
-   [Desanexar vdisk](detach-vdisk.md)  
  
-   [Detalhar vdisk](detail-vdisk.md)  
  
-   [Expandir vdisk](expand-vdisk.md)  
  
-   [Mesclar vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

