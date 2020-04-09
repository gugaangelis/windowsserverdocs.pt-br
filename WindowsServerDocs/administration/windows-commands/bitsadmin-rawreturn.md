---
title: bitsadmin rawreturn
description: Tópico de comandos do Windows para **Bitsadmin rawreturn**, que retorna dados adequados para análise.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8cd84b6082b1fda8061ee7b324dd66991d3b206
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849879"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retorna os dados adequados para análise. Normalmente, você usa esse comando em conjunto com as opções **/Create** e **/Get*** para receber apenas o valor. Você deve especificar essa opção antes de outras opções.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /rawreturn
```

## <a name="remarks"></a>Comentários

- Remove os caracteres de nova linha e a formatação da saída.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera os dados brutos para o estado do trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)