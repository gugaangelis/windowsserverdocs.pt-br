---
title: logman delete
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e396d79dc936f56a69fac9469c020348640f1094
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437784"
---
# <a name="logman-delete"></a>logman delete

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclua coletor de dados existente.  

## <a name="syntax"></a>Sintaxe  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  

|        Parâmetro        |                                                                               Descrição                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Exibe contextual a Ajuda.                                                                     |
|   -s <computer name>    |                                                          Execute o comando no computador remoto especificado.                                                          |
|     -config <value>     |                                                         Especifica o arquivo de configurações que contém opções de comando.                                                         |
|       [-n] <name>       |                                                                   Nome do coletor de dados de destino.                                                                    |
|          -ets           |                                              Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendamento.                                               |
| -u [-] < usuário [senha] > | Especifica o usuário executar como. Inserindo um \* User um prompt para a senha. A senha não é exibida quando ela for digitada no prompt de senha. |

## <a name="BKMK_examples"></a>Exemplos  
O comando a seguir exclui o perf_log do coletor de dados.  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
