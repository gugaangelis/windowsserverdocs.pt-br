---
title: logman delete
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b30fd6eb7915d3d0296988a98968dcde58bdbc2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724370"
---
# <a name="logman-delete"></a>logman delete

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

excluir um coletor de dados existente.  

## <a name="syntax"></a>Sintaxe  
```  
logman delete <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parâmetros  

|        Parâmetro        |                                                                               Descrição                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Exibe a ajuda contextual.                                                                     |
|   -s<computer name>    |                                                          Execute o comando no computador remoto especificado.                                                          |
|     -config <value>     |                                                         Especifica o arquivo de configurações que contém as opções de comando.                                                         |
|       [-n]<name>       |                                                                   Nome do coletor de dados de destino.                                                                    |
|          -ETS           |                                              Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar.                                               |
| -[-] u <usuário [senha] > | Especifica o usuário a ser executado como. Inserir um \* para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |

## <a name="examples"></a>Exemplos  
O comando a seguir exclui o coletor de dados perf_log.  
```  
logman delete perf_log  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
