---
title: mput_1 FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6308f7b47d58bc25d964944f96fbc83a26350962
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376209"
---
# <a name="ftp-mput_1"></a>FTP: mput_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia arquivos locais para o computador remoto usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro  |                       Descrição                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Especifica o arquivo local a ser copiado para o computador remoto. |

## <a name="BKMK_Examples"></a>Disso  
Copie **Program1. exe** e **Program2. exe** para o computador remoto usando o tipo de transferência de arquivo atual.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binário](ftp-binary.md)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
