---
title: cmdkey
description: Tópico de comandos do Windows para cmdkey, que cria, lista e exclui nomes de usuário e senhas ou credenciais armazenadas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cdb732bf95e30af012f78d1bad337d6d6d191268
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847589"
---
# <a name="cmdkey"></a>cmdkey

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria, lista e exclui nomes de usuário e senhas ou credenciais armazenados.

## <a name="syntax"></a>Sintaxe
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
### <a name="parameters"></a>Parâmetros

|             Parâmetros             |                                                                                    Descrição                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /Add:<TargetName>          | Adiciona um nome de usuário e senha à lista.<p>Requer o parâmetro de <TargetName> que identifica o computador ou o nome de domínio ao qual essa entrada será associada. |
|       /genérico:<TargetName>        |   Adiciona credenciais genéricas à lista.<p>Requer o parâmetro de <TargetName> que identifica o computador ou o nome de domínio ao qual essa entrada será associada.    |
|             /smartcard             |                                                                    Recupera a credencial de um cartão inteligente.                                                                     |
|          /User:<UserName>          |                                 Especifica o nome do usuário ou da conta a ser armazenada com essa entrada. Se o *nome de usuário* não for fornecido, ele será solicitado.                                  |
|          /Pass:<Password>          |                                       Especifica a senha a ser armazenada com essa entrada. Se a *senha* não for fornecida, ela será solicitada.                                        |
| /Delete{:<TargetName> &#124; /RAS} |  exclui um nome de usuário e uma senha da lista. Se *TargetName* for especificado, essa entrada será excluída. Se/RAS for especificado, a entrada de acesso remoto armazenada será excluída.   |
|         /List:<TargetName>         |                  Exibe a lista de nomes de usuário e credenciais armazenados. Se *TargetName* não for especificado, todos os nomes de usuário e credenciais armazenados serão listados.                   |
|                 /?                 |                                                                        Exibe a ajuda no prompt de comando.                                                                        |

## <a name="remarks"></a>Comentários
- se mais de um cartão inteligente for encontrado no sistema quando a opção de linha de comando/SmartCard for usada, **cmdkey** exibirá informações sobre todos os cartões inteligentes disponíveis e, em seguida, solicitará que o usuário especifique qual deles usar.
- As senhas não serão exibidas depois que elas forem armazenadas.
  ## <a name="examples"></a><a name=BKMK_examples></a>Disso
  Para exibir uma lista de todos os nomes de usuário e credenciais que estão armazenados, digite:
  ```
  cmdkey /list
  ```
  Para adicionar um nome de usuário e uma senha para o usuário Mikedan acessar o computador Server01 com a senha Kleo, digite:
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  Para adicionar um nome de usuário e uma senha para o usuário Mikedan acessar o computador Server01 e solicitar a senha sempre que Server01 for acessado, digite:
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  Para excluir a credencial que o acesso remoto armazenou, digite:
  ```
  cmdkey /delete /ras
  ```
  Para excluir a credencial armazenada para Server01, digite:
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
