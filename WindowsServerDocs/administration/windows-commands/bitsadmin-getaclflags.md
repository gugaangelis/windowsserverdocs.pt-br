---
title: bitsadmin getaclflags
description: Tópico de comandos do Windows para **Bitsadmin getaclflags**, que recupera os sinalizadores de propagações da ACL (lista de controle de acesso).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d53018e2fa5c659c8cf4b0ec985beda848a8c1af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850789"
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

## <a name="remarks"></a>Comentários

Exibe um ou mais dos seguintes valores de sinalizador:

- **o** -copie as informações do proprietário com o arquivo.

- **g** -copiar informações do grupo com o arquivo.

- **d** – copiar informações de DACL (lista de controle de acesso discricionário) com o arquivo.

- **s** -copiar informações de SACL (lista de controle de acesso) do sistema com o arquivo.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera os sinalizadores de propagação da lista de controle de acesso para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)