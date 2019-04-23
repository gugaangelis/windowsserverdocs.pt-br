---
title: Gerenciar-bde changekey
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 991d05cb79e6596c3efb3e69b681203838cd8c20
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842087"
---
# <a name="manage-bde-changekey"></a>manage-bde: changekey



Modifica a chave de inicialização para uma unidade do sistema operacional. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -changekey [<Drive>] [<PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive>|Representa uma letra de unidade seguida de dois-pontos.|
|\<PathToExternalKeyDirectory>|Representa o local do diretório para salvar o arquivo de chave externo de inicialização que pode ser usado para desbloquear a unidade.|
|-computername|Especifica que gerenciar bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando.|
|\<Nome >|Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.|
|-? ou /?|Exibe uma ajuda breve no prompt de comando.|
|-help ou -h|Exibe uma ajuda detalhada no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **- changekey** comando para criar uma nova chave de inicialização na unidade E usar com a criptografia BitLocker na unidade C.
```
manage-bde –changekey C: E:\
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Gerenciar-bde](manage-bde.md)