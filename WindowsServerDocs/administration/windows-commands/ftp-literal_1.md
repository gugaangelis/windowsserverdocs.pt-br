---
title: literal_1 FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f502bb56c94734870962f56cfb85dcc17ddc3f93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843389"
---
# <a name="ftp-literal_1"></a>FTP: literal_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, o Windows Server 2012 envia argumentos textuais para o servidor FTP remoto. Um único código de resposta FTP é retornado.   

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
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Envie um comando **Quit** para o servidor FTP remoto.  
```  
literal quit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: cotação](ftp-quote.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
