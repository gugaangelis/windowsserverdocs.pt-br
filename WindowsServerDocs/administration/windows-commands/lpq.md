---
title: lpq
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18ff1ff3ecbc2df0a437ec8a465dec9a12123ede
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437515"
---
# <a name="lpq"></a>lpq

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe o status de uma fila de impressão em um computador executando o Daemon de impressora de linha (LPD).  

## <a name="syntax"></a>Sintaxe  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>Parâmetros  

|    Parâmetro     |                                                                        Descrição                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | Especifica (por nome ou endereço IP), o computador ou dispositivo que hospeda a fila de impressão LPD com um status que você deseja exibir de compartilhamento de impressora. Obrigatório. |
| -P <printerName> |                           Especifica (por nome) da impressora para a fila de impressão com um status que você deseja exibir. Obrigatório.                           |
|        -l        |                                      Especifica que você deseja exibir detalhes sobre o status da fila de impressão.                                      |
|        /?        |                                                           Exibe a ajuda no prompt de comando.                                                            |

## <a name="remarks"></a>Comentários  
O **-S** e **-P** parâmetros diferenciam maiusculas de minúsculas e devem ser digitados em letras maiusculas.  
## <a name="BKMK_examples"></a>Exemplos  
Este exemplo mostra como exibir o status da fila da impressora Laserprinter1 em um host LPD em 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
[Referência do comando Imprimir](print-command-reference.md)  
