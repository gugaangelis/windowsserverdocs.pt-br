---
title: makecab
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0231b6f1ddd3e81caa7544587f764e2308015b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374150"
---
# <a name="makecab"></a>makecab

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empacotar arquivos existentes em um arquivo de gabinete (. cab).
## <a name="syntax"></a>Sintaxe
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
### <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                        Descrição                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     Arquivo a ser compactado.                                                                     |
|    <destination>     | Nome do arquivo para fornecer o arquivo compactado. Se omitido, o último caractere do nome do arquivo de origem será substituído por um sublinhado (_) e usado como o destino. |
| /f < directives_file > |                                                   Um arquivo com diretivas **Makecab** (pode ser repetido).                                                   |
|    /d var =<value>    |                                                          Define a variável com o valor especificado.                                                           |
|       /l <dir>       |                                               Local para colocação de destino (o padrão é o diretório atual).                                               |
|       /v [<n>]        |                                                    Definir nível de detalhamento de depuração (0 = nenhum,..., 3 = completo).                                                     |
|          /?          |                                                           Exibe a ajuda no prompt de comando.                                                            |

## <a name="remarks"></a>Comentários
-   Consulte [formato de gabinete da Microsoft](https://go.microsoft.com/fwlink/?LinkId=226852) no MSDN para obter informações sobre o directive_file.

## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

