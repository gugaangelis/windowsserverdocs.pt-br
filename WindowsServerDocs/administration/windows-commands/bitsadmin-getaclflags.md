---
title: bitsadmin getaclflags
description: Artigo de referência para o comando Bitsadmin getaclflags, que recupera os sinalizadores de propagações da ACL (lista de controle de acesso).
ms.topic: reference
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4c5c7101db157b7fa5b56833b8d5f89619bd8d51
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632327"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera os sinalizadores de propagações da ACL (lista de controle de acesso), refletindo se os itens são herdados por objetos filho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getaclflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

### <a name="remarks"></a>Comentários

Retorna um ou mais dos seguintes valores de sinalizador:

- **o** -copie as informações do proprietário com o arquivo.

- **g** -copiar informações do grupo com o arquivo.

- **d** – copiar informações de DACL (lista de controle de acesso discricionário) com o arquivo.

- **s** -copiar informações de SACL (lista de controle de acesso) do sistema com o arquivo.

## <a name="examples"></a>Exemplos

Para recuperar os sinalizadores de propagação da lista de controle de acesso para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
