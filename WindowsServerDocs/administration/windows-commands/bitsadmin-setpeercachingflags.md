---
title: bitsadmin setpeercachingflags
description: O tópico de comandos do Windows para Bitsadmin setpeercachingflags, que define sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os colegas e se o trabalho pode baixar conteúdo de pares.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d19e4d14b47e4aa96e9ad9d4367e872350ad4d43
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849239"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

Define sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os colegas e se o trabalho pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|{1&gt;Valor&lt;1}|O valor é um inteiro não assinado com a seguinte interpretação para os bits na representação binária.</br>1-o trabalho pode baixar o conteúdo de pares.</br>2-os arquivos do trabalho podem ser armazenados em cache e servidos para os pares.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define sinalizadores para o trabalho chamado *myJob* , que permite que ele baixe o conteúdo de pares.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)