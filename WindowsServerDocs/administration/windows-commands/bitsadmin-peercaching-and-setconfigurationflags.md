---
title: Bitsadmin de cache e setconfigurationflags
description: Tópico de comandos do Windows para **Bitsadmin de cache** e **setconfigurationflags**, que define os sinalizadores de configuração que determinam se o computador pode fornecer conteúdo a pares e se ele pode baixar conteúdo de pares.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebaa09da2d4594d2762e67dc5884dd15cf4d1da8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850129"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>Bitsadmin de cache e setconfigurationflags

Define os sinalizadores de configuração que determinam se o computador pode fornecer conteúdo a pares e se ele pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| {1&gt;Valor&lt;1} | Um inteiro não assinado com a seguinte interpretação para os bits na representação binária:<ul><li> Para permitir que os dados do trabalho sejam baixados de um par, defina o bit menos significativo.</li><li>Para permitir que os dados do trabalho sejam servidos para os pares, defina o segundo bit à direita.</li></ul>|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir especifica os dados do trabalho a serem baixados de pares para o trabalho chamado *myDownloadJob*.

```
C:\> bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)