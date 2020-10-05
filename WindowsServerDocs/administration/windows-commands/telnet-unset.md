---
title: telnet unset
description: Artigo de referência para o comando de desdefinição de Telnet, que desativa as opções definidas anteriormente.
ms.topic: reference
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 51604615e989325b2e5719d02c51f70dfd84bd0c
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717913"
---
# <a name="telnet-unset"></a>Telnet: remover definição

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desativa as opções definidas anteriormente.

## <a name="syntax"></a>Sintaxe

```
u {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| bsasdel | Envia **Backspace** como um **Backspace**. |
| CRLF | Envia a tecla **Enter** como uma CR. Também conhecido como modo de alimentação de linha. |
| delasbs | Envia **delete** como **delete**. |
| escape | Remove a configuração de caractere de escape. |
| localecho | Desativa o localecho. |
| registro em log | Desliga o log. |
| NTLM | Desativa a autenticação NTLM. |
| ? | Exibe a ajuda para este comando. |

## <a name="example"></a>Exemplo

Desative o registro em log.

```
u logging
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
