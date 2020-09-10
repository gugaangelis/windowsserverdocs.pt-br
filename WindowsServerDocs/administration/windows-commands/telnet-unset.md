---
title: telnet unset
description: Artigo de referência para desdefinição de Telnet, que desativa as opções definidas anteriormente.
ms.topic: reference
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 96b8126d758d5277129f88f193cfea4b25e80584
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640816"
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
