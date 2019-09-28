---
title: lpq
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6a3755c010c9bb4549deed08f26b5a0670fe7318
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374197"
---
# <a name="lpq"></a>lpq

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe o status de uma fila de impressão em um computador que executa o LPD (daemon de impressora de linha).  

## <a name="syntax"></a>Sintaxe  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>Parâmetros  

|    Parâmetro     |                                                                        Descrição                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | Especifica (por nome ou endereço IP) o computador ou o dispositivo de compartilhamento de impressora que hospeda a fila de impressão LPD com um status que você deseja exibir. Obrigatório. |
| -P <printerName> |                           Especifica (por nome) a impressora para a fila de impressão com um status que você deseja exibir. Obrigatório.                           |
|        -l        |                                      Especifica que você deseja exibir detalhes sobre o status da fila de impressão.                                      |
|        /?        |                                                           Exibe a ajuda no prompt de comando.                                                            |

## <a name="remarks"></a>Comentários  
Os parâmetros **-S** e **-P** diferenciam maiúsculas de minúsculas e devem ser digitados em letras maiúsculas.  
## <a name="BKMK_examples"></a>Disso  
Este exemplo mostra como exibir o status da fila de impressora Laserprinter1 em um host LPD em 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
[Referência de comando de impressão](print-command-reference.md)  
