---
title: aspas de FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bf13704150d602fbfa4e3b1a3fb1774d3bf7363
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843029"
---
# <a name="ftp-quote"></a>FTP: cotação

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Envie um comando **Quit** para o servidor FTP remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: literal_1](ftp-literal_1.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
