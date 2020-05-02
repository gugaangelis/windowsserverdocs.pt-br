---
title: usuário de FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb4f0f47f23bec312c57266479c261c96535e8cc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725076"
---
# <a name="ftp-user"></a>FTP: usuário

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica um usuário para o computador remoto.   
## <a name="syntax"></a>Sintaxe  
```  
user <UserName> [<Password>] [<Account>]  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                                                                      Descrição                                                                      |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <UserName>  |                                          Especifica um nome de usuário com o qual fazer logon no computador remoto.                                           |
| [<Password>] |               Especifica a senha para o *nome de usuário*. Se uma senha não for especificada, mas for necessária, o **FTP** solicitará a senha.               |
| [<Account>]  | Especifica uma conta com a qual fazer logon no computador remoto. Se uma *conta* não for especificada, mas for necessária, o **FTP** solicitará a conta. |

## <a name="examples"></a>Exemplos  
Especifique user1 com a senha password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
