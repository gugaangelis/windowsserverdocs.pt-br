---
title: Manage-bde ChangeKey
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2885723f7712ee507fb43465035513bc8c7a1b2b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840189"
---
# <a name="manage-bde-changekey"></a>Manage-bde: ChangeKey



Modifica a chave de inicialização para uma unidade do sistema operacional. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -changekey [<Drive>] [<PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Unidade de \<>|Representa uma letra de unidade seguida de dois-pontos.|
|\<PathToExternalKeyDirectory >|Representa o local do diretório para salvar o arquivo de chave de inicialização externa que pode ser usado para desbloquear a unidade.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|Nome do \<>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

O exemplo a seguir ilustra o uso do comando **-ChangeKey** para criar uma nova chave de inicialização na unidade E para usar com a criptografia BitLocker na unidade C.
```
manage-bde -changekey C: E:\
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)
