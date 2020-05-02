---
title: open_1 FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15a27d2f7512da352a0f4ddf02fa2511ffce7c1d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725191"
---
# <a name="ftp-open_1"></a>FTP: open_1

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se ao servidor FTP especificado.   
## <a name="syntax"></a>Sintaxe  
```  
open <computer> [<Port>]  
```  
#### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                                           Descrição                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Especifica o computador remoto ao qual você está tentando se conectar.                 |
|  [<Port>]  | Especifica um número de porta TCP a ser usado para se conectar a um servidor FTP. Por padrão, a porta TCP 21 é usada. |

## <a name="remarks"></a>Comentários  
Você pode usar um endereço IP ou nome do computador (nesse caso, um servidor DNS ou um arquivo hosts deve estar disponível) para especificar o **computador**.  
## <a name="examples"></a>Exemplos  
Conecte-se ao servidor FTP em **ftp.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Conecte-se ao servidor FTP em **ftp.Microsoft.com** que está escutando na porta TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
