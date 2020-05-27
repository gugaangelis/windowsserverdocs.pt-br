---
title: desdefinição de Telnet
description: Tópico de referência para desdefinição de Telnet, que desativa as opções definidas anteriormente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c121d6501f87ae1218381871bcbf536d9407ba31
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821036"
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
