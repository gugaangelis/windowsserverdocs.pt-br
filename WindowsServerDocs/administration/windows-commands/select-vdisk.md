---
title: selecionar vdisk
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1bfa6450d1704cde1e5ff2a50a8e3b61a30d0766
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384195"
---
# <a name="select-vdisk"></a>selecionar vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona o disco rígido virtual especificado \(VHD @ no__t-1 e desloca o foco para ele.  
  
> [!NOTE]  
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|arquivo @ no__t-0 @ no__t-1|Especifica o caminho completo e o nome de arquivo de um arquivo VHD existente.|  
|NOERR|Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|  
  
## <a name="BKMK_examples"></a>Disso  
Para deslocar o foco para o VHD chamado Test. VHD, digite:  
  
```  
select vdisk file="c:\test\test.vhd"  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [anexar vdisk](attach-vdisk.md)  
  
-   [Compact vdisk](compact-vdisk.md)  
  
  
  
-   [Desanexar vdisk](detach-vdisk.md)  
  
-   [detalhes do VDISK](detail-vdisk.md)  
  
-   [expandir vdisk](expand-vdisk.md)  
  
-   [Mesclar vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

