---
title: Bitsadmin de cache e getconfigurationflags
description: O tópico de comandos do Windows para **Bitsadmin de cache** e **getconfigurationflags**, que obtém os sinalizadores de configuração que determinam se o computador fornece conteúdo a pares e se pode baixar conteúdo de pares.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: be8d6a719d63c8e9c6250320560b6ce21274c680
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850169"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>Bitsadmin de cache e getconfigurationflags

Obtém os sinalizadores de configuração que determinam se o computador fornece conteúdo a pares e se pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir obtém os sinalizadores de configuração para o trabalho chamado *myDownloadJob*.

```
C:\> bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)