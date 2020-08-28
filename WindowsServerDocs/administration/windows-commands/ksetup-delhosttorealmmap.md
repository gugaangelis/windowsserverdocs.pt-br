---
title: ksetup delhosttorealmmap
description: Artigo de referência para o comando ksetup delhosttorealmmap, que remove um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm.
ms.topic: reference
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4401bc186a2471fdd300279b42d4eb1375fc1aa0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033984"
---
# <a name="ksetup-delhosttorealmmap"></a>ksetup delhosttorealmmap

Remove um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm. Esse comando também remove qualquer mapeamento entre um host para o realm (ou vários hosts para o realm).

O mapeamento é armazenado no registro, em `HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm` . Depois de executar esse comando, é recomendável verificar se o mapeamento aparece no registro.

## <a name="syntax"></a>Sintaxe

```
ksetup /delhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<hostname>` | Especifica o nome de domínio totalmente qualificado do computador. |
| `<realmname>` | Especifica o nome DNS em maiúsculas, como CORP. CONTOSO.COM. |

### <a name="examples"></a>Exemplos

Para alterar a configuração da CONTOSO Realm e excluir o mapeamento do computador host IPops897 para o realm, digite:

```
ksetup /delhosttorealmmap IPops897 CONTOSO
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup addhosttorealmmap](ksetup-addhosttorealmmap.md)
