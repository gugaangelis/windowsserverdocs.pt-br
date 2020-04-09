---
title: logman delete
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0ab38eab988770de4fbcef8af2c7be6a6137b16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840749"
---
# <a name="logman-delete"></a>logman delete

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

excluir um coletor de dados existente.  

## <a name="syntax"></a>Sintaxe  
```  
logman delete <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parâmetros  

|        Parâmetro        |                                                                               Descrição                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Exibe a ajuda contextual.                                                                     |
|   -s <computer name>    |                                                          Execute o comando no computador remoto especificado.                                                          |
|     -config <value>     |                                                         Especifica o arquivo de configurações que contém as opções de comando.                                                         |
|       [-n] <name>       |                                                                   Nome do coletor de dados de destino.                                                                    |
|          -ETS           |                                              Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar.                                               |
| -[-] u < usuário [senha] > | Especifica o usuário a ser executado como. Inserir um \* para a senha produz uma solicitação para a senha. A senha não é exibida quando você a digita no prompt de senha. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso  
O comando a seguir exclui o coletor de dados perf_log.  
```  
logman delete perf_log  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
