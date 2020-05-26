---
title: domínio ksetup
description: Tópico de referência para o comando de domínio ksetup, que define o nome de domínio para todas as operações Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d497f2bc76bae8a95b077658c661e0fdc1e93f3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817796"
---
# <a name="ksetup-domain"></a>domínio ksetup

Define o nome de domínio para todas as operações Kerberos.

## <a name="syntax"></a>Sintaxe

```
ksetup /domain <domainname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<domainname>` | Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou uma forma simples do nome, como contoso.com ou contoso.|

### <a name="examples"></a>Exemplos

Para estabelecer uma conexão com um domínio válido, como a Microsoft, usando o `ksetup /mapuser` subcomando, digite:

```
ksetup /mapuser principal@realm domain-user /domain domain-name
```

Após uma conexão bem-sucedida, você receberá uma nova TGT ou um TGT existente será atualizado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup mapuser](ksetup-mapuser.md)