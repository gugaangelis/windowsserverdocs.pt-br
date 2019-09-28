---
title: MGET FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 666025c92b6fb1a612cbe7b83833557a8a7d5017
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376303"
---
# <a name="ftp-mget"></a>FTP: MGET

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia arquivos remotos para o computador local usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                        Descrição                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Especifica os arquivos remotos a serem copiados para o computador local. |

## <a name="BKMK_Examples"></a>Disso  
Copie os arquivos remotos a **. exe** e **b. exe** para o computador local usando o tipo de transferência de arquivo atual.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binário](ftp-binary.md)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
