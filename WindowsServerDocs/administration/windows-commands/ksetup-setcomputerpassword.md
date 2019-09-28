---
title: 'ksetup: setcomputerpassword'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d1d3742476385eb770c9cb5c798c1f6ab27c74f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374937"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup: setcomputerpassword



Define a senha para o computador local. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Password >|Usa a senha fornecida para definir a conta de computador no computador local.</br>A senha só pode ser definida usando uma conta com privilégios administrativos. A senha pode ter de 1 a 156 caracteres alfanuméricos ou especiais.|

## <a name="remarks"></a>Comentários

Esse comando afeta apenas a conta do computador.

Você deve reiniciar o computador para que a alteração da senha entre em vigor.

A senha da conta de computador não é exibida no registro nem como saída do comando **ksetup** .

## <a name="BKMK_Examples"></a>Disso

Altere a senha da conta de computador no computador local de IPops897 para IPop $897!.
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)