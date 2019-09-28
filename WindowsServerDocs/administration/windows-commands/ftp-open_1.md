---
title: open_1 FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c5da1c73362c0396300f712b2e45b906d1652604
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376192"
---
# <a name="ftp-open_1"></a>FTP: open_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se ao servidor FTP especificado.   
## <a name="syntax"></a>Sintaxe  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                                           Descrição                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Especifica o computador remoto ao qual você está tentando se conectar.                 |
|  [<Port>]  | Especifica um número de porta TCP a ser usado para se conectar a um servidor FTP. Por padrão, a porta TCP 21 é usada. |

## <a name="remarks"></a>Comentários  
Você pode usar um endereço IP ou nome do computador (nesse caso, um servidor DNS ou um arquivo hosts deve estar disponível) para especificar o **computador**.  
## <a name="BKMK_Examples"></a>Disso  
Conecte-se ao servidor FTP em **ftp.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Conecte-se ao servidor FTP em **ftp.Microsoft.com** que está escutando na porta TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
