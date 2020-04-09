---
title: bitsadmin listfiles
description: Tópico de comandos do Windows para **Bitsadmin ListFiles**, que lista os arquivos no trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1af11f7876a3d1cd36aa38c7ac26563c01e81ab5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850309"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles

Lista os arquivos no trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /listfiles <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera a lista de arquivos para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /listfiles myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)