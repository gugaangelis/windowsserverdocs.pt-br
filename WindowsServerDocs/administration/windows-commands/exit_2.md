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
ms.openlocfilehash: b3490c6bc95a762bf2cb1da70f389fb8f583344f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819487"
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
|Parâmetro|Descrição|  
|-------|--------|  
|/b|sai do script em lotes atual em vez de fechar Cmd.exe. Se executado fora de um script em lotes, será fechado Cmd.exe.|  
|<exitCode>|Especifica um número. Se **/b** for especificado, a variável de ambiente ERRORLEVEL é definida para esse número. Se você estiver saindo **Cmd.exe**, o código de saída do processo é definido para esse número.|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="BKMK_examples"></a>Exemplos  
Para fechar o interpretador de comandos, Cmd.exe, digite:  
```  
exit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  
