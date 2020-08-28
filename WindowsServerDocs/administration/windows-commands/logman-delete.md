---
title: logman delete
description: Artigo de referência para o comando logman Delete, que exclui um coletor de dados existente.
ms.topic: reference
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbcae06f149f0ea1c443f91fec45d87b5a358136
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023800"
---
# <a name="logman-delete"></a>logman delete

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui um coletor de dados existente.

## <a name="syntax"></a>Sintaxe

```
logman delete <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -s `<computer name>` | Executa o comando no computador remoto especificado. |
| -configuração `<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| [-n] `<name>` | O nome do objeto de destino. |
| -ETS | Envia comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar. |
| -[-] u `<user [password]>` | Especifica o usuário a ser executado como. Inserir um \* para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
| /? | Exibe a ajuda contextual. |

### <a name="examples"></a>Exemplos

Para excluir a *perf_log*do coletor de dados, digite:

```
logman delete perf_log
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando logman](logman.md)
