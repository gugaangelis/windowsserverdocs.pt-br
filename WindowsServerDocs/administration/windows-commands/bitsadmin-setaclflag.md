---
title: bitsadmin setaclflag
description: Tópico de referência para o comando Bitsadmin setaclflag, que define os sinalizadores de propagações da ACL (lista de controle de acesso).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1852bd267fe22825d55f7522a81179e9290e2a00
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716983"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Define os sinalizadores de propagações da ACL (lista de controle de acesso) para o trabalho. Os sinalizadores indicam que você deseja manter as informações de proprietário e ACL com o arquivo que está sendo baixado. Por exemplo, para manter o proprietário e o grupo com o arquivo, defina **flags** o parâmetro flags `og`como.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setaclflag <job> <flags>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| sinalizadores | Especifique um ou mais dos valores, incluindo:<ul><li>**o** -copie as informações do proprietário com o arquivo.</li><li>**g** -copiar informações do grupo com o arquivo.</li><li>**d** – copiar informações de DACL (lista de controle de acesso discricionário) com o arquivo.</li><li>**s** -copiar informações de SACL (lista de controle de acesso) do sistema com o arquivo.</li></ul> |

## <a name="examples"></a>Exemplos

Para definir os sinalizadores de propagação da lista de controle de acesso para o trabalho chamado *myDownloadJob*, portanto, ele mantém as informações de proprietário e grupo com os arquivos baixados.

```
bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
