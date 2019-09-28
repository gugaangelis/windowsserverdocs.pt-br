---
title: Bitsadmin de cache e setconfigurationflags
description: O tópico de comandos do Windows para o **Bitsadmin de cache e o setconfigurationflags** -define os sinalizadores de configuração que determinam se o computador pode fornecer conteúdo a pares e pode baixar conteúdo de pares.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a65d54bcaa2bce26eb2b7c98250837ab09c7a423
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381106"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>Bitsadmin de cache e setconfigurationflags



Define os sinalizadores de configuração que determinam se o computador pode fornecer conteúdo a pares e pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Valor|O valor é um inteiro sem sinal com a seguinte interpretação para os bits na representação binária:</br>-Permitir que os dados do trabalho sejam baixados de um par: Definir o bit menos significativo</br>-Permitir que os dados do trabalho sejam servidos para os pares: Defina o 2º bit à direita.|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir especifica os dados do trabalho a serem baixados de pares para o trabalho chamado *myJob*.
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)