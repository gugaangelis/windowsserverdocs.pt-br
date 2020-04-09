---
title: makecab
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46efbae1d59a85071622df51fc018d0bf73dc504
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840259"
---
# <a name="makecab"></a>makecab

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empacotar arquivos existentes em um arquivo de gabinete (. cab).
## <a name="syntax"></a>Sintaxe
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
#### <a name="parameters"></a>Parâmetros

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
-   Consulte [formato de gabinete da Microsoft](https://go.microsoft.com/fwlink/?LinkId=226852) no MSDN para obter informações sobre directive_file.

## <a name="additional-references"></a>Referências adicionais
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

