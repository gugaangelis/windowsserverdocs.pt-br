---
title: bitsadmin getreplyfilename
description: Tópico de referência para o comando Bitsadmin getreplyfilename, que obtém o caminho do arquivo que contém a resposta de upload do servidor para o trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: daed755e0ddc045174b98a8d4f9ee84da155cba6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717599"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Obtém o caminho do arquivo que contém a resposta de carregamento do servidor para o trabalho.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getreplyfilename <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o nome de arquivo de upload-resposta para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getreplyfilename myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
