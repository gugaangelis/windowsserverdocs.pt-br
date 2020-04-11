---
title: bitsadmin setaclflag
description: Tópico de comandos do Windows para **Bitsadmin setaclflag**, que define os sinalizadores de propagações da ACL (lista de controle de acesso).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0aae550e94d04db518edccafb1d6bcf46d0320b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123060"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Define os sinalizadores de propagações da ACL (lista de controle de acesso) para o trabalho. Os sinalizadores indicam que você deseja manter as informações de proprietário e ACL com o arquivo que está sendo baixado. Por exemplo, para manter o proprietário e o grupo com o arquivo, defina o parâmetro **flags** como `og`.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setaclflag <job> <flags>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| {1&gt;flags&lt;1} | Especifique um ou mais dos valores, incluindo:<ul><li>**o** -copie as informações do proprietário com o arquivo.</li><li>**g** -copiar informações do grupo com o arquivo.</li><li>**d** – copiar informações de DACL (lista de controle de acesso discricionário) com o arquivo.</li><li>**s** -copiar informações de SACL (lista de controle de acesso) do sistema com o arquivo.</li></ul> |

## <a name="remarks"></a>Comentários

A opção/setaclflag é usada para manter informações de proprietário e de lista de controle de acesso quando um trabalho está baixando dados de um compartilhamento do Windows (SMB).

## <a name="examples"></a>Exemplos

O exemplo a seguir define os sinalizadores de propagação da lista de controle de acesso para o trabalho chamado *myDownloadJob* para manter as informações de proprietário e grupo com os arquivos baixados.

```
C:\>bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)&reg;'    