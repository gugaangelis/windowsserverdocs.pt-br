---
title: Manage-bde pacote de pacotes
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0a1b4fd0fff1153a0f778eca105ecfc618a4689
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374049"
---
# <a name="manage-bde-keypackage"></a>Manage-bde Pacote de



Gera um pacote de chaves para uma unidade. O pacote de chaves pode ser usado em conjunto com a ferramenta de reparo para reparar unidades corrompidas. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive >|Representa uma letra de unidade seguida de dois-pontos.|
|-ID|Crie um pacote de chaves usando o protetor de chave com o identificador especificado por esse valor de ID.|
|-path|Local no qual salvar o pacote de chaves criado.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Nome >|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir ilustra o uso do comando **-KeyPackage** para criar um pacote de chaves para a unidade C com base no protetor de chave identificado pelo GUID e salvar o pacote de chaves em F:\folder.
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path "f:\Folder"
```

> [!TIP]
> Use **Manage-bde – protectors – obtenha** junto com a letra da unidade para a qual você deseja criar um pacote de chaves para obter uma lista de GUIDs disponíveis para usar como o valor de ID.

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)