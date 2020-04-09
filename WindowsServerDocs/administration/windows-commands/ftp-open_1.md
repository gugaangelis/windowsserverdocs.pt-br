---
title: open_1 FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8bd3063a52908d65f336afcda6b6982d5bc9bf94
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843179"
---
# <a name="ftp-open_1"></a>FTP: open_1

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
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
