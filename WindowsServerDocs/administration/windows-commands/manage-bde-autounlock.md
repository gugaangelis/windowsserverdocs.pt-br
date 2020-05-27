---
title: gerenciar o desbloqueio automático do bde
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 929469ad3d4bd8b3a76c3681a5f24424ba6d99df
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820716"
---
# <a name="manage-bde-autounlock"></a>Manage-bde: desbloqueio automático



Gerencia o desbloqueio automático de unidades de dados protegidas pelo BitLocker.

## <a name="syntax"></a>Sintaxe

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]

```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-habilitar|Habilita o desbloqueio automático para uma unidade de dados.|
|-desabilitar|Desabilita o desbloqueio automático de uma unidade de dados.|
|-clearallkeys|Remove todas as chaves externas armazenadas na unidade do sistema operacional.|
|\<> da unidade|Representa uma letra de unidade seguida de dois-pontos.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Name>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a>Exemplos

Para ilustrações usando o comando **-autounlock** para habilitar o desbloqueio automático da unidade de dados E.
```
manage-bde –autounlock -enable E:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)