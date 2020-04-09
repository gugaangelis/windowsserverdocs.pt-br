---
title: bitsadmin replaceremoteprefix
description: O tópico de comandos do Windows para **Bitsadmin replaceremoteprefix**, que altera a URL remota para todos os arquivos no trabalho de *oldprefix* para *newprefix*, conforme necessário.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0cea0108a292815e31e893e91dc4079305c1da9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849809"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Altera a URL remota para todos os arquivos no trabalho de *oldprefix* para *newprefix*, conforme necessário.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /replaceremoteprefix <job> <oldprefix> <newprefix>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| oldprefix | Prefixo de URL existente. |
| newprefix | Novo prefixo de URL. |

## <a name="examples"></a>Exemplos

O exemplo a seguir altera a URL remota para todos os arquivos no trabalho denominado *myDownloadJob*, de *http://stageserver* para *http://prodserver* .

```
C:\>bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>{1&gt;{2&gt;Informações adicionais&lt;2}&lt;1}

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)