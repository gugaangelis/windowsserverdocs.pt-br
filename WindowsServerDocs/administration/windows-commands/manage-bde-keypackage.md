---
title: Manage-bde pacote de pacotes
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a53edd060cb8c7a7a52e86130b2136893a42150
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840099"
---
# <a name="manage-bde-keypackage"></a>Manage-bde: KeyPackage



Gera um pacote de chaves para uma unidade. O pacote de chaves pode ser usado em conjunto com a ferramenta de reparo para reparar unidades corrompidas. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Unidade de \<>|Representa uma letra de unidade seguida de dois-pontos.|
|-ID|Crie um pacote de chaves usando o protetor de chave com o identificador especificado por esse valor de ID.|
|-path|Local no qual salvar o pacote de chaves criado.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|Nome do \<>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

O exemplo a seguir ilustra o uso do comando **-KeyPackage** para criar um pacote de chaves para a unidade C com base no protetor de chave identificado pelo GUID e salvar o pacote de chaves em F:\folder.
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

> [!TIP]
> Use **Manage-bde – protectors – obtenha** junto com a letra da unidade para a qual você deseja criar um pacote de chaves para obter uma lista de GUIDs disponíveis para usar como o valor de ID.

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)