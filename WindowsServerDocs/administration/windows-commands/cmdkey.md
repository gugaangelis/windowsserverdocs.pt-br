---
title: cmdkey
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c06a04fa6473bc30c3b354f049a55775d2308a0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434316"
---
# <a name="cmdkey"></a>cmdkey

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria, lista e exclui nomes de usuário e senhas ou credenciais armazenados.

## <a name="syntax"></a>Sintaxe
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
## <a name="parameters"></a>Parâmetros

|             Parâmetros             |                                                                                    Descrição                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /add:<TargetName>          | Adiciona um nome de usuário e senha à lista.<br /><br />Requer que o parâmetro de <TargetName> que identifica o nome do computador ou domínio que será associada a essa entrada. |
|       /generic:<TargetName>        |   Adiciona credenciais genéricas à lista.<br /><br />Requer que o parâmetro de <TargetName> que identifica o nome do computador ou domínio que será associada a essa entrada.    |
|             /smartcard             |                                                                    Recupera as credenciais de um cartão inteligente.                                                                     |
|          /user:<UserName>          |                                 Especifica o usuário ou o nome da conta de armazenamento com essa entrada. Se *nome de usuário* não é fornecida, ela será solicitada.                                  |
|          /pass:<Password>          |                                       Especifica a senha para armazenar com essa entrada. Se *senha* não é fornecida, ela será solicitada.                                        |
| /delete{:<TargetName> &#124; /ras} |  Exclui um nome de usuário e senha da lista. Se *TargetName* for especificado, que a entrada será excluída. Se /ras for especificado, a entrada de acesso remoto armazenados será excluída.   |
|         /list:<TargetName>         |                  Exibe a lista de nomes de usuário armazenado e as credenciais. Se *TargetName* não é nomes de usuário especificado, tudo armazenados e as credenciais serão listadas.                   |
|                 /?                 |                                                                        Exibe a ajuda no prompt de comando.                                                                        |

## <a name="remarks"></a>Comentários
- Se mais de um cartão inteligente for encontrado no sistema quando a opção de linha de comando /smartcard é usada, **cmdkey** exibirá informações sobre todos os cartões inteligentes disponíveis e, em seguida, solicitar que o usuário especifique qual deles usar.
- As senhas não serão exibidas depois que eles são armazenados.
  ## <a name="BKMK_examples"></a>Exemplos
  Para exibir uma lista de todos os nomes de usuário e credenciais armazenadas, digite:
  ```
  cmdkey /list
  ```
  Para adicionar um nome de usuário e a senha do usuário Mikedan a acesse o computador Server01 com a senha Kleo, digite:
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  Para adicionar um nome de usuário e senha do usuário Mikedan acesse o computador Server01 e solicitará a senha sempre que é acessado Server01, digite:
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  Para excluir a credencial que armazenou o acesso remoto, digite:
  ```
  cmdkey /delete /ras
  ```
  Para excluir a credencial que é armazenada para o Server01, digite:
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>Referências adicionais
  [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
