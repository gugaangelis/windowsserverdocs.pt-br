---
title: gerenciar o desbloqueio automático do bde
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6927c964a3f35c70fc3b9467cc2d16bbd973b17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374144"
---
# <a name="manage-bde-autounlock"></a>Manage-bde: desbloqueio automático



Gerencia o desbloqueio automático de unidades de dados protegidas pelo BitLocker. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]

```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-habilitar|Habilita o desbloqueio automático para uma unidade de dados.|
|-desabilitar|Desabilita o desbloqueio automático de uma unidade de dados.|
|-clearallkeys|Remove todas as chaves externas armazenadas na unidade do sistema operacional.|
|\<Drive >|Representa uma letra de unidade seguida de dois-pontos.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Nome >|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir ilustra o uso do comando **-autounlock** para habilitar o desbloqueio automático da unidade de dados E.
```
manage-bde –autounlock -enable E:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)