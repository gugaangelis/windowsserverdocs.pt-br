---
title: Bitsadmin gettemporárioname
description: O tópico de comandos do Windows para **Bitsadmin gettemporárioname**, que relata o nome de arquivo temporário do arquivo fornecido dentro do trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c331ecf12cb02d34c76692158c79eafbe5691c5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850449"
---
# <a name="bitsadmin-gettemporaryname"></a>Bitsadmin gettemporárioname

Informa o nome de arquivo temporário do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gettemporaryname <job> <file_index>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| file_index | Começa em 0. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir relata o nome de arquivo temporário de File 2 para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)