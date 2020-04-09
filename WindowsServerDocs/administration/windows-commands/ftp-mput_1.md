---
title: mput_1 FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 489e18da937e12a1fc69e0ee84d9dda00309ccd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843229"
---
# <a name="ftp-mput_1"></a>FTP: mput_1

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia arquivos locais para o computador remoto usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
mput <LocalFile>[ ]  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro  |                       Descrição                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Especifica o arquivo local a ser copiado para o computador remoto. |

## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Copie **Program1. exe** e **Program2. exe** para o computador remoto usando o tipo de transferência de arquivo atual.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binário](ftp-binary.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
