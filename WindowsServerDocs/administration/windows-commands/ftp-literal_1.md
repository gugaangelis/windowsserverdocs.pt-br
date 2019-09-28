---
title: literal_1 FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 393dea27e8567a72a5bd25c927282ade93e317c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376267"
---
# <a name="ftp-literal_1"></a>FTP: literal_1

>Aplica-se a: O Windows Server (canal semestral), o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012 envia argumentos textuais para o servidor FTP remoto. Um único código de resposta FTP é retornado.   

## <a name="syntax"></a>Sintaxe  
```  
literal <Argument> [ ]  
```  
### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                    Descrição                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica o argumento a ser enviado ao servidor FTP. |

## <a name="remarks"></a>Comentários  
O comando **literal** é idêntico ao comando **quote** .  
## <a name="BKMK_Examples"></a>Disso  
Envie um comando **Quit** para o servidor FTP remoto.  
```  
literal quit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: cotação](ftp-quote.md)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
