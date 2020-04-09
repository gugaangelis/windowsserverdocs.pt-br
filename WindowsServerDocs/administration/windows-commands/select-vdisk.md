---
title: selecionar vdisk
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65e186413bebbf467cd4c2033d274badd1fbea80
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834739"
---
# <a name="select-vdisk"></a>selecionar vdisk

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona o disco rígido virtual especificado \(VHD\) e desloca o foco para ele.  
  
> [!NOTE]  
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|<full path> de\=de arquivo|Especifica o caminho completo e o nome de arquivo de um arquivo VHD existente.|  
|NOERR|Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|  
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para deslocar o foco para o VHD chamado Test. VHD, digite:  
  
```  
select vdisk file=c:\test\test.vhd  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [anexar vdisk](attach-vdisk.md)  
  
-   [Compact vdisk](compact-vdisk.md)  
  
  
  
-   [Desanexar vdisk](detach-vdisk.md)  
  
-   [detalhes do VDISK](detail-vdisk.md)  
  
-   [expandir vdisk](expand-vdisk.md)  
  
-   [Mesclar vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

