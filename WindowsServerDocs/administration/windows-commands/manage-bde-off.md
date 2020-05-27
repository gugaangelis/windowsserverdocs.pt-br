---
title: Manage-bde desativado
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a27c119-d385-45e5-89fe-e311d4429876
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ebbe8e0985c08ba4f156de9b87bce143184f80a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820656"
---
# <a name="manage-bde-off"></a>Manage-bde: off



Descriptografa a unidade e desativa o BitLocker. Todos os protetores de chave são removidos quando a descriptografia é concluída.

## <a name="syntax"></a>Sintaxe

```
manage-bde -off [<Volume>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> de volume|Uma letra de unidade seguida por dois-pontos, um caminho de GUID de volume ou um volume montado.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Name>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a>Exemplos

Para ilustrações usando o comando **-off** para desativar o BitLocker na unidade C.
```
manage-bde –off C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)