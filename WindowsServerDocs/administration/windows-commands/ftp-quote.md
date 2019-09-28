---
title: aspas de FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65660cf7311713295dae8a94c9174229f5ee44be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376082"
---
# <a name="ftp-quote"></a>FTP: cotação

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia argumentos textuais para o servidor FTP remoto. Um único código de resposta FTP é retornado.   
## <a name="syntax"></a>Sintaxe  
```  
quote <Argument>[ ]  
```  
### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                    Descrição                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica o argumento a ser enviado ao servidor FTP. |

## <a name="remarks"></a>Comentários  
O comando **quote** é idêntico ao comando **literal** .  
## <a name="BKMK_Examples"></a>Disso  
Envie um comando **Quit** para o servidor FTP remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: literal_1](ftp-literal_1.md)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
