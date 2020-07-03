---
title: telnet unset
description: Artigo de referência para desdefinição de Telnet, que desativa as opções definidas anteriormente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3efbf7d4f2507d16dbe0beb704ab0c80467a56d3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935296"
---
# <a name="telnet-unset"></a>Telnet: remover definição

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desativa as opções definidas anteriormente.

## <a name="syntax"></a>Sintaxe
```
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]
```
#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|bsasdel|Envia **Backspace** como um **Backspace**.|
|CRLF|Envia a tecla **Enter** como uma CR. Também conhecido como modo de alimentação de linha.|
|delasbs|Envia **delete** como **delete**.|
|escape|Remove a configuração de caractere de escape.|
|localecho|Desativa o localecho.|
|registro em log|Desliga o log.|
|NTLM|Desativa a autenticação NTLM.|
|?|Exibe a ajuda para este comando.|
## <a name="examples"></a>Exemplos
Desative o registro em log.
```
u logging
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
