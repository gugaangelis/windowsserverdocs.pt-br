---
title: exit
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13cf7a7658394e59ce6cc7e66c3083cd3d359574
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844879"
---
# <a name="exit"></a>exit

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sai do programa cmd. exe (o interpretador de comando) ou o script em lote atual.  
Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).  
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

## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para fechar o interpretador de comandos, cmd. exe, digite:  
```  
exit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  

