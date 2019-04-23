---
title: finger
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42622fdf19cdd50b76d32989769874cbd05e9f4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826937"
---
# <a name="finger"></a>finger

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre um usuário ou usuários em um computador remoto especificado (geralmente um computador executando UNIX) que está executando o serviço ou daemon finger. O computador remoto especifica o formato e a saída da exibição de informações do usuário. Usado sem parâmetros, **dedo** exibe a Ajuda. 
## <a name="syntax"></a>Sintaxe
```
finger [-l] [<User>] [@<Host>] [...]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|-l|Exibe informações de usuário no formato de lista longa.|
|<User>|Especifica o usuário sobre o qual você deseja informações. Se você omitir a *usuário* parâmetro, **dedo** exibe informações sobre todos os usuários no computador especificado.|
|@<Host>|Especifica o computador remoto que executa o serviço de dedo em que você estiver procurando por informações do usuário. Você pode especificar um nome de computador ou endereço IP.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
Vários User@Host parâmetros podem ser especificados.
Você deve prefixar **dedo** parâmetros com um hífen (-) em vez de uma barra (/).
Esse comando está disponível somente se o protocolo de Internet Protocol (TCP/IP) é instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.
O Windows Server 2003 não fornece um serviço de dedo.
## <a name="BKMK_Examples"></a>Exemplos
Para exibir informações de user1 no computador users.microsoft.com, digite:
```
finger user1@users.microsoft.com
```
Para exibir informações para todos os usuários no computador users.microsoft.com, digite:
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
