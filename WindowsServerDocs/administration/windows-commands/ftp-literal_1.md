---
title: literal_1 FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc4f8aff5a22da93330a12a75e5f368285366216
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725253"
---
# <a name="ftp-literal_1"></a>FTP: literal_1

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, o Windows Server 2012 envia argumentos textuais para o servidor FTP remoto. Um único código de resposta FTP é retornado.   

## <a name="syntax"></a>Sintaxe  
```  
literal <Argument> [ ]  
```  
#### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                    Descrição                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica o argumento a ser enviado ao servidor FTP. |

## <a name="remarks"></a>Comentários  
O comando **literal** é idêntico ao comando **quote** .  
## <a name="examples"></a>Exemplos  
Envie um comando **Quit** para o servidor FTP remoto.  
```  
literal quit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: cotação](ftp-quote.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
