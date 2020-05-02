---
title: Manage-bde WipeFreeSpace
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f1e0f99c226edae467ecb09222b18098ac399ee
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724041"
---
# <a name="manage-bde-wipefreespace"></a>Manage-bde: WipeFreeSpace



Apaga o espaço livre no volume, removendo quaisquer fragmentos de dados que possam ter existido no espaço. Executar esse comando em um volume que foi criptografado usando o método de criptografia somente espaço usado fornece o mesmo nível de proteção que o método de criptografia de criptografia de volume completo.

## <a name="syntax"></a>Sintaxe

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> da unidade|Representa uma letra de unidade seguida por dois-pontos, um caminho de GUID de volume ou um volume montado.|
|-Cancelar|Cancela um apagamento de espaço livre em andamento.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Name>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a>Exemplos

Para ilustrações usando o comando **-w** para criar, apague o espaço livre na unidade C.
```
manage-bde -w C:
```
Para ilustrações usando o comando **-w** com o parâmetro **-Cancel** para cancelar o apagamento do espaço livre na unidade C.
```
manage-bde -w -Cancel C:
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)