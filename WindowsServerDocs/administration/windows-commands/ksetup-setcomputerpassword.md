---
title: ksetup:setcomputerpassword
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0679bb9ee429e05c7679411c5493bd21b530ef8e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831537"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup:setcomputerpassword



Define a senha para o computador local. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Senha >|Usa a senha fornecida para definir a conta de computador no computador local.</br>A senha só pode ser definida usando uma conta com privilégios administrativos. A senha pode ser de 1 a 156 alfanuméricos ou caracteres especiais.|

## <a name="remarks"></a>Comentários

Esse comando afeta apenas a conta de computador.

Você deve reiniciar o computador para que a alteração de senha entrar em vigor.

A senha da conta de computador não é exibida no registro ou como saída o **ksetup** comando.

## <a name="BKMK_Examples"></a>Exemplos

Altere a senha da conta de computador no computador local de IPops897 para IPop$ 897!.
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)