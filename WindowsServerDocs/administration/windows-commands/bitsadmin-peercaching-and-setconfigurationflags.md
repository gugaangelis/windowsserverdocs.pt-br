---
title: setconfigurationflags e cache de par bitsadmin
description: Tópico de comandos do Windows para **cache de par bitsadmin e setconfigurationflags** -define os sinalizadores de configuração que determinam se o computador pode fornecer conteúdo para pontos e pode baixar conteúdo de pares.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 22408d4aab7f5ea374511bc16751d911a84644f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813327"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>setconfigurationflags e cache de par bitsadmin



Define os sinalizadores de configuração que determinam se o computador pode fornecer conteúdo para pontos e pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Valor|O valor é um inteiro sem sinal com a seguinte interpretação para os bits na representação binária:</br>-Permitir que os dados do trabalho a ser baixado de um par: Definir o bit menos significativo</br>-Permitir que os dados do trabalho a ser atendido para pares: Defina o 2º bit à direita.|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir especifica os dados do trabalho a ser baixado de colegas do trabalho nomeado *myJob*.
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)