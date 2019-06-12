---
title: aspas de FTP
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1f468bfc384673818dc53be303f82cd4803cb2eb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438497"
---
# <a name="ftp-quote"></a>ftp: quote

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia argumentos literais para o servidor ftp remoto. Um código de resposta de ftp único é retornado.   
## <a name="syntax"></a>Sintaxe  
```  
quote <Argument>[ ]  
```  
### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                    Descrição                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica o argumento a ser enviado ao servidor ftp. |

## <a name="remarks"></a>Comentários  
O **cotação** comando é idêntico de **literal** comando.  
## <a name="BKMK_Examples"></a>Exemplos  
Enviar uma **encerrar** comando ao servidor ftp remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: literal_1](ftp-literal_1.md)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
