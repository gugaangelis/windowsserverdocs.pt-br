---
title: Manage-bde WipeFreeSpace
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a9995c6872380af61bec5d3b200e733c034ea6b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839709"
---
# <a name="manage-bde-wipefreespace"></a>Manage-bde: WipeFreeSpace



Apaga o espaço livre no volume, removendo quaisquer fragmentos de dados que possam ter existido no espaço. Executar esse comando em um volume que foi criptografado usando o método de criptografia somente espaço usado fornece o mesmo nível de proteção que o método de criptografia de criptografia de volume completo. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Unidade de \<>|Representa uma letra de unidade seguida por dois-pontos, um caminho de GUID de volume ou um volume montado.|
|-Cancelar|Cancela um apagamento de espaço livre em andamento.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|Nome do \<>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

O exemplo a seguir ilustra o uso do comando **-w** para criar o apagamento do espaço livre na unidade C.
```
manage-bde -w C:
```
O exemplo a seguir ilustra o uso do comando **-w** com o parâmetro **-Cancel** para cancelar o apagamento do espaço livre na unidade C.
```
manage-bde -w -Cancel C:
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)