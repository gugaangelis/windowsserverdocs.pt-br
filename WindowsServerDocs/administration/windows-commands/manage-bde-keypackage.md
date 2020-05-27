---
title: Manage-bde pacote de pacotes
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 012377013ace07a2b90597c708847062e6923b2f
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820666"
---
# <a name="manage-bde-keypackage"></a>Manage-bde: KeyPackage



Gera um pacote de chaves para uma unidade. O pacote de chaves pode ser usado em conjunto com a ferramenta de reparo para reparar unidades corrompidas.

## <a name="syntax"></a>Sintaxe

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> da unidade|Representa uma letra de unidade seguida de dois-pontos.|
|-ID|Crie um pacote de chaves usando o protetor de chave com o identificador especificado por esse valor de ID.|
|-caminho|Local no qual salvar o pacote de chaves criado.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Name>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a>Exemplos

Para ilustrações usando o comando **-KeyPackage** para criar um pacote de chaves para a unidade C com base no protetor de chave identificado pelo GUID e para salvar o pacote de chaves em F:\folder.
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

> [!TIP]
> Use **Manage-bde – protectors – obtenha** junto com a letra da unidade para a qual você deseja criar um pacote de chaves para obter uma lista de GUIDs disponíveis para usar como o valor de ID.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)