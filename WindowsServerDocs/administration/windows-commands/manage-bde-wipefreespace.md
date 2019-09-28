---
title: Manage-bde WipeFreeSpace
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35d5f5fe079485d35fa412502bec745a136fbce9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373782"
---
# <a name="manage-bde-wipefreespace"></a>Manage-bde WipeFreeSpace



Apaga o espaço livre no volume, removendo quaisquer fragmentos de dados que possam ter existido no espaço. Executar esse comando em um volume que foi criptografado usando o método de criptografia "somente espaço usado" fornece o mesmo nível de proteção que o método de criptografia "criptografia de volume completo". Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive >|Representa uma letra de unidade seguida por dois-pontos, um caminho de GUID de volume ou um volume montado.|
|-Cancelar|Cancela um apagamento de espaço livre em andamento.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Nome >|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir ilustra o uso do comando **-w** para criar o apagamento do espaço livre na unidade C.
```
manage-bde -w C:
```
O exemplo a seguir ilustra o uso do comando **-w** com o parâmetro **-Cancel** para cancelar o apagamento do espaço livre na unidade C.
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)