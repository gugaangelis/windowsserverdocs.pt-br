---
title: finger
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec8040480a7cb75a5a42e051393e3db4a47f8e2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725611"
---
# <a name="finger"></a>finger

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre um usuário ou usuários em um computador remoto especificado (normalmente um computador executando UNIX) que está executando o daemon ou serviço Finger. O computador remoto especifica o formato e a saída da exibição de informações do usuário. Usado sem parâmetros, **Finger** exibe a ajuda. 
## <a name="syntax"></a>Sintaxe
```
finger [-l] [<User>] [@<Host>] [...]
```
#### <a name="parameters"></a>Parâmetros

| Parâmetro |                                                                            Descrição                                                                            |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    -l     |                                                          Exibe informações do usuário em formato de lista longa.                                                           |
|  <User>   | Especifica o usuário sobre o qual você deseja obter informações. Se você omitir o parâmetro de *usuário* , **Finger** exibirá informações sobre todos os usuários no computador especificado. |
|  @<Host>  |        Especifica o computador remoto que executa o serviço Finger em que você está procurando informações do usuário. Você pode especificar um nome de computador ou endereço IP.        |
|    /?     |                                                               Exibe a ajuda no prompt de comando.                                                                |

## <a name="remarks"></a>Comentários
Vários User@Host parâmetros podem ser especificados.
Você deve prefixar parâmetros de **dedo** com um hífen (-) em vez de uma barra (/).
Esse comando estará disponível somente se o protocolo TCP/IP estiver instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.
O Windows Server 2003 não fornece um serviço Finger.
## <a name="examples"></a>Exemplos
Para exibir informações para user1 no computador users.microsoft.com, digite:
```
finger user1@users.microsoft.com
```
Para exibir informações para todos os usuários no computador users.microsoft.com, digite:
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Referências adicionais
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
