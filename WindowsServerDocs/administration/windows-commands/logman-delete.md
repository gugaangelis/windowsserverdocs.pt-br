---
title: logman delete
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8360a4955a5ebed3eb25fda77acf587c56fbf5d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374432"
---
# <a name="logman-delete"></a>logman delete

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

excluir um coletor de dados existente.  

## <a name="syntax"></a>Sintaxe  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  

|        Parâmetro        |                                                                               Descrição                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Exibe a ajuda contextual.                                                                     |
|   -s <computer name>    |                                                          Execute o comando no computador remoto especificado.                                                          |
|     -config <value>     |                                                         Especifica o arquivo de configurações que contém as opções de comando.                                                         |
|       [-n] <name>       |                                                                   Nome do coletor de dados de destino.                                                                    |
|          -ETS           |                                              Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar.                                               |
| -[-] u < usuário [senha] > | Especifica o usuário a ser executado como. Inserir um \* para a senha produz uma solicitação para a senha. A senha não é exibida quando você a digita no prompt de senha. |

## <a name="BKMK_examples"></a>Disso  
O comando a seguir exclui o Data Collector perf_log.  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
