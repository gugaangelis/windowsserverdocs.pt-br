---
title: 'ksetup: setcomputerpassword'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e65ea6e935d9fde9c23842755c36e418928dec7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841369"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup: setcomputerpassword



Define a senha para o computador local. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /setcomputerpassword <Password>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<senha >|Usa a senha fornecida para definir a conta de computador no computador local.</br>A senha só pode ser definida usando uma conta com privilégios administrativos. A senha pode ter de 1 a 156 caracteres alfanuméricos ou especiais.|

## <a name="remarks"></a>Comentários

Esse comando afeta apenas a conta do computador.

Você deve reiniciar o computador para que a alteração da senha entre em vigor.

A senha da conta de computador não é exibida no registro nem como saída do comando **ksetup** .

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Altere a senha da conta de computador no computador local de IPops897 para IPop $897!.
```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)