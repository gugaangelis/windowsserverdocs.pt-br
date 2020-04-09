---
title: lpq
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 051b1983fcc0fddd7b69e561c0a27a120f78d998
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840389"
---
# <a name="lpq"></a>lpq

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe o status de uma fila de impressão em um computador que executa o LPD (daemon de impressora de linha).  

## <a name="syntax"></a>Sintaxe  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
### <a name="parameters"></a>Parâmetros  

|    Parâmetro     |                                                                        Descrição                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | Especifica (por nome ou endereço IP) o computador ou o dispositivo de compartilhamento de impressora que hospeda a fila de impressão LPD com um status que você deseja exibir. Obrigatório. |
| -P <printerName> |                           Especifica (por nome) a impressora para a fila de impressão com um status que você deseja exibir. Obrigatório.                           |
|        -l        |                                      Especifica que você deseja exibir detalhes sobre o status da fila de impressão.                                      |
|        /?        |                                                           Exibe a ajuda no prompt de comando.                                                            |

## <a name="remarks"></a>Comentários  
Os parâmetros **-S** e **-P** diferenciam maiúsculas de minúsculas e devem ser digitados em letras maiúsculas.  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Este exemplo mostra como exibir o status da fila de impressora Laserprinter1 em um host LPD em 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
[Referência de comando de impressão](print-command-reference.md)  
