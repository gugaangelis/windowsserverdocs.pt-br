---
title: Desanexar vdisk
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b6a1ecd3d787506c89f120bed204cc30e6d68d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822727"
---
# <a name="detach-vdisk"></a>Desanexar vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompe o disco rígido virtual selecionado \(VHD\) apareça como uma unidade de disco rígido local no computador host. Quando um VHD é desanexado, você pode copiá-lo a outros locais.  
  
> [!NOTE]  
> Este comando só é aplicável ao Windows 7 e Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
detach vdisk [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|noerr|Usado somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   Um VHD deve ser selecionado e desanexado para essa operação seja bem-sucedida. Use o **selecionar o vdisk** comando para selecionar um VHD e mudar o foco a ele.  
  
## <a name="BKMK_Examples"></a>Exemplos  
Para desanexar o VHD selecionado, digite:  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [Compactar vdisk](compact-vdisk.md)  
  
  
  
-   [Detalhar vdisk](detail-vdisk.md)  
  
-   [Expandir vdisk](expand-vdisk.md)  
  
-   [Mesclar vdisk](merge-vdisk.md)  
  
-   [Selecionar o vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

