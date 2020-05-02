---
title: mput_1 FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3fb654d5a2f44b9b63238abdbaee8d6a0294861
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725225"
---
# <a name="ftp-mput_1"></a>FTP: mput_1

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia arquivos locais para o computador remoto usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
mput <LocalFile>[ ]  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro  |                       Descrição                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Especifica o arquivo local a ser copiado para o computador remoto. |

## <a name="examples"></a>Exemplos  
Copie **Program1. exe** e **Program2. exe** para o computador remoto usando o tipo de transferência de arquivo atual.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binário](ftp-binary.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
