---
title: ksetup
description: Tópico de referência para o comando ksetup setrealm, que define o nome de um realm Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03b33977f57e187a8bea69be78c1e9c094b9a73e
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817276"
---
# <a name="ksetup-setrealm"></a>ksetup

Define o nome de um realm Kerberos.

> [!IMPORTANT]
> Não há suporte para a definição do realm Kerberos em um controlador de domínio. A tentativa de fazer isso causa um aviso e uma falha de comando.

## <a name="syntax"></a>Sintaxe

```
ksetup /setrealm <DNSdomainname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<DNSdomainname>` | Especifica o nome DNS em maiúsculas, como CORP. CONTOSO.COM. Você pode usar o nome de domínio totalmente qualificado ou uma forma simples do nome. Se você não usar letras maiúsculas para o nome DNS, será solicitado que você faça a verificação para continuar. |

### <a name="examples"></a>Exemplos

Para definir o realm deste computador para um nome de domínio específico e para restringir o acesso por um controlador que não seja de domínio apenas ao realm Kerberos do CONTOSO, digite:

```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [ksetup removerealm](ksetup-removerealm.md)
