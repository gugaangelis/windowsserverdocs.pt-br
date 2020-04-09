---
title: bitsadmin wrap
description: O tópico de comandos do Windows para Bitsadmin Wrap, que encapsula qualquer linha de texto de saída que se estende além da borda mais à direita da janela de comando para a próxima linha.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 009a0452f44c4944ae110ca6b9e0570793c32a72
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848749"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Encapsula qualquer linha de texto de saída que se estenda além da borda mais à direita da janela de comando para a próxima linha.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Wrap Job
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Trabalho|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Especifique antes de outras opções. Por padrão, todas as opções, exceto a opção de [Monitor Bitsadmin](bitsadmin-monitor.md) , encapsulam a saída.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera informações para o trabalho chamado *myDownloadJob* e encapsula a saída.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
