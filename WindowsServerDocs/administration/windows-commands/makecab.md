---
title: makecab
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7b120cf990abe2024fd6c96ca2f1ef11fa2350ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437534"
---
# <a name="makecab"></a>makecab

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empacote os arquivos existentes em um arquivo de gabinete (. cab).
## <a name="syntax"></a>Sintaxe
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
### <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                        Descrição                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     Arquivo para compactar.                                                                     |
|    <destination>     | Nome do arquivo para fornecer o arquivo compactado. Se omitido, o último caractere do nome do arquivo de origem é substituído por um sublinhado (_) e usado como o destino. |
| /f <directives_file> |                                                   Um arquivo com **makecab** diretivas (podem ser repetidas).                                                   |
|    /d var=<value>    |                                                          Define a variável com o valor especificado.                                                           |
|       /l <dir>       |                                               Local onde colocar o destino (o padrão é o diretório atual).                                               |
|       /v[<n>]        |                                                    Definir nível de detalhes de depuração (0 = nenhum,..., 3 = full).                                                     |
|          /?          |                                                           Exibe a ajuda no prompt de comando.                                                            |

## <a name="remarks"></a>Comentários
-   Consulte a [formato do Microsoft Cabinet](https://go.microsoft.com/fwlink/?LinkId=226852) no MSDN para obter informações sobre directive_file.

## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

