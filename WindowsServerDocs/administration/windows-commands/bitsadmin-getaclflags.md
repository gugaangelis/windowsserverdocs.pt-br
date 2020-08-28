---
title: bitsadmin getaclflags
description: Artigo de referência para o comando Bitsadmin getaclflags, que recupera os sinalizadores de propagações da ACL (lista de controle de acesso).
ms.topic: reference
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5254d65bb5ba3e35fcf5368e24045530a76bfd95
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033694"
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
