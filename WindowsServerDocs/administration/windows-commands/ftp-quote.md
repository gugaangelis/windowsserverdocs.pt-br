---
title: aspas de FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1101dd6a5fa163df8d43d182e9d0dfe66e340b60
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725147"
---
# <a name="ftp-quote"></a>FTP: cotação

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia argumentos textuais para o servidor FTP remoto. Um único código de resposta FTP é retornado.   
## <a name="syntax"></a>Sintaxe  
```  
quote <Argument>[ ]  
```  
#### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                    Descrição                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica o argumento a ser enviado ao servidor FTP. |

## <a name="remarks"></a>Comentários  
O comando **quote** é idêntico ao comando **literal** .  
## <a name="examples"></a>Exemplos  
Envie um comando **Quit** para o servidor FTP remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: literal_1](ftp-literal_1.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
