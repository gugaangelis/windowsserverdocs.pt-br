---
title: MGET FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72b45f84622fcd6abf7a743f606fb514def584cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843349"
---
# <a name="ftp-mget"></a>FTP: MGET

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia arquivos remotos para o computador local usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
mget <remoteFile>[ ]  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                        Descrição                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Especifica os arquivos remotos a serem copiados para o computador local. |

## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Copie os arquivos remotos a **. exe** e **b. exe** para o computador local usando o tipo de transferência de arquivo atual.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binário](ftp-binary.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
