---
title: ksetup delhosttorealmmap
description: Tópico de referência para o comando ksetup delhosttorealmmap, que remove um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17fc30e76247c570c653d5ec38501a2199435c7f
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817856"
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
