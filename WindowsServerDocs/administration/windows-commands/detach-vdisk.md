---
title: detach vdisk
description: O tópico comandos do Windows para desanexar VDISK, que impede que o VHD (disco rígido virtual) selecionado apareça como uma unidade de disco rígido local no computador host.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14eb66031841624156afb03f492e2afce5bc56f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846499"
---
# <a name="detach-vdisk"></a>detach vdisk

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompe a exibição do VHD (disco rígido virtual) selecionado como uma unidade de disco rígido local no computador host. Quando um VHD é desanexado, você pode copiá-lo a outros locais.  
  
> [!NOTE]  
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
detach vdisk [noerr]  
```  
  
#### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|NOERR|Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   Um VHD deve ser selecionado e desanexado para que essa operação tenha sucesso. Use o comando **Select VDISK** para selecionar um VHD e deslocar o foco para ele.  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Para desanexar o VHD selecionado, digite:  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [anexar vdisk](attach-vdisk.md)  
  
-   [Compact vdisk](compact-vdisk.md)  

-   [detalhes do VDISK](detail-vdisk.md)  
  
-   [expandir vdisk](expand-vdisk.md)  
  
-   [Mesclar vdisk](merge-vdisk.md)  
  
-   [selecionar vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

