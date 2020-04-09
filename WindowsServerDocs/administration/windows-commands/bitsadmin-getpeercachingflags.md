---
title: bitsadmin getpeercachingflags
description: O tópico de comandos do Windows para **Bitsadmin getpeercachingflags**, que recupera sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e atendidos aos pares e se o bits pode baixar conteúdo para o trabalho de pares.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cdf9683d1a65400286b4604bd9420a5ab863d4af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850549"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os pares e se o BITS pode baixar conteúdo para o trabalho de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getpeercachingflags <job> 
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera os sinalizadores para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getpeercachingflags myJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)