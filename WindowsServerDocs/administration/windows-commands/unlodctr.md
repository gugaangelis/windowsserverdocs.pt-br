---
title: unlodctr
description: Artigo de referência para o comando Unlodctr, que remove nomes de contadores de desempenho e explica o texto de um serviço ou driver de dispositivo do registro do sistema.
ms.topic: reference
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3983f98df0de6a8048f78b6ebdb16cccafea8da4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156626"
---
# <a name="unlodctr"></a>unlodctr

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove os **nomes de contadores de desempenho** e o **texto explicativo** para um serviço ou driver de dispositivo do registro do sistema.

> [!WARNING]
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

## <a name="syntax"></a>Sintaxe

```
unlodctr <drivername>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<drivername>` | Remove as configurações do **nome do contador de desempenho** e o **texto explicativo** para driver ou serviço `<drivername>` do registro do Windows Server. Se o `<drivername>` incluir espaços, você deverá usar aspas ao contrário do texto, por exemplo, "nome do driver". |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para remover os **nomes do contador de desempenho** atual e o **texto explicativo** para o serviço SMTP, digite:

```
unlodctr SMTPSVC
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
