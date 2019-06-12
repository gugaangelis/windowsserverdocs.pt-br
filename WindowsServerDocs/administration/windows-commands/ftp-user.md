---
title: usuário de FTP
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c9406af0868421fa54fe757742cf2a120561b9c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438339"
---
# <a name="ftp-user"></a>FTP: usuário

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica um usuário no computador remoto.   
## <a name="syntax"></a>Sintaxe  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                                                                      Descrição                                                                      |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <UserName>  |                                          Especifica um nome de usuário com o qual fazer logon no computador remoto.                                           |
| [<Password>] |               Especifica a senha para *nome de usuário*. Se uma senha não for especificada, mas é necessária, **ftp** solicitará a senha.               |
| [<Account>]  | Especifica uma conta com o qual fazer logon no computador remoto. Se um *conta* não for especificado, mas é necessário, **ftp** solicitará a conta. |

## <a name="BKMK_Examples"></a>Exemplos  
Especifique o User1 com a senha Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
