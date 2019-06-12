---
title: exit
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e599f84389b23e527e3718a620d5fdfefe24edb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439466"
---
# <a name="exit"></a>exit

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

sai do programa Cmd.exe (o interpretador de comando) ou o script em lotes atual.  
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).  
## <a name="syntax"></a>Sintaxe  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parâmetros  

| Parâmetro  |                                                                                         Descrição                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      sai do script em lotes atual em vez de fechar Cmd.exe. Se executado fora de um script em lotes, será fechado Cmd.exe.                                      |
| <exitCode> | Especifica um número. Se **/b** for especificado, a variável de ambiente ERRORLEVEL é definida para esse número. Se você estiver saindo **Cmd.exe**, o código de saída do processo é definido para esse número. |
|     /?     |                                                                             Exibe a ajuda no prompt de comando.                                                                             |

## <a name="BKMK_examples"></a>Exemplos  
Para fechar o interpretador de comandos, Cmd.exe, digite:  
```  
exit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  

