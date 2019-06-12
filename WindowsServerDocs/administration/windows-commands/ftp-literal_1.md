---
title: literal_1 de FTP
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 693916507488a9a480315a8e9299baa93a223b8a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438662"
---
# <a name="ftp-literal1"></a>FTP: literal_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 envia argumentos literais para o servidor ftp remoto. Um código de resposta de ftp único é retornado.   

## <a name="syntax"></a>Sintaxe  
```  
literal <Argument> [ ]  
```  
### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                    Descrição                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica o argumento a ser enviado ao servidor ftp. |

## <a name="remarks"></a>Comentários  
O **literal** comando é idêntico de **cotação** comando.  
## <a name="BKMK_Examples"></a>Exemplos  
Enviar uma **encerrar** comando ao servidor ftp remoto.  
```  
literal quit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: quote](ftp-quote.md)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
