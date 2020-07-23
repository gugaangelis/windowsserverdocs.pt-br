---
title: diantz
description: Artigo de referência para o comando diantz, que empacota arquivos existentes em um arquivo de gabinete (. cab).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 218ed5d7-1203-4d68-ad9b-65cdd022d54f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 339ffaa56b65b2b061923ab38ee172c5e933d675
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958338"
---
# <a name="diantz"></a>diantz

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empacotar arquivos existentes em um arquivo de gabinete (. cab). Esse comando executa as mesmas ações que o [comando Makecab](makecab.md)atualizado.

## <a name="syntax"></a>Sintaxe

```
diantz [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
diantz [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<source>` | Arquivo a ser compactado. |
| `<destination>` | Nome do arquivo para fornecer o arquivo compactado. Se omitido, o último caractere do nome do arquivo de origem será substituído por um sublinhado (_) e usado como o destino. |
| /f `<directives_file>` | Um arquivo com diretivas **diantz** (pode ser repetido). |
| /d var =`<value>` | Define a variável com o valor especificado. |
| /l`<dir>` | Local para colocação de destino (o padrão é o diretório atual). |
| /v [ `<n>` ] | Definir nível de detalhamento de depuração (0 = nenhum,..., 3 = completo). |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Formato de gabinete da Microsoft](/previous-versions/bb417343(v=msdn.10))
