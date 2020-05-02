---
title: exit
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e47dfa42f2bacb3fe9f12d1da9163bcf828e9488
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725701"
---
# <a name="exit"></a>exit

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sai do programa cmd. exe (o interpretador de comando) ou o script em lote atual.  
  
## <a name="syntax"></a>Sintaxe  
```  
exit [/b] [<exitCode>]  
```  
### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                                                                                         Descrição                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      Sai do script do lote atual em vez de sair do cmd. exe. Se executado de fora de um script em lotes, o fechará cmd. exe.                                      |
| `<exitCode>` | Especifica um número numérico. Se **/b** for especificado, a variável de ambiente ERRORLEVEL será definida para esse número. Se você estiver encerrando **cmd. exe**, o código de saída do processo será definido como esse número. |
|     /?     |                                                                             Exibe a ajuda no prompt de comando.                                                                             |

## <a name="examples"></a>Exemplos  
Para fechar o interpretador de comandos, cmd. exe, digite:  
```  
exit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  

