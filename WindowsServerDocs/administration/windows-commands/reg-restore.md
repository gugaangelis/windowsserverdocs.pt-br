---
title: Reg Restore
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6c121256cecaebc26e2c402d9b9ced8890eddc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384667"
---
# <a name="reg-restore"></a>Reg Restore



Grava subchaves e entradas salvas de volta no registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Reg restore <KeyName> <FileName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName >|Especifica o caminho completo da subchave a ser restaurada. A operação de restauração funciona apenas com o computador local. O KeyName deve incluir uma chave de raiz válida. As chaves raiz válidas são: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<Nome de arquivo >|Especifica o nome e o caminho do arquivo com o conteúdo a ser gravado no registro. Esse arquivo deve ser criado com antecedência com a operação **reg Save** usando uma extensão. HIV.|
|/?|Exibe a ajuda para **reg Restore** no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Antes de editar as entradas do registro, salve a subchave pai com a operação **reg Save** . Se a edição falhar, restaure a subchave original com a operação **reg Restore** .
-   A tabela a seguir lista os valores de retorno para a operação **reg Restore** .

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Disso

Para restaurar o arquivo chamado NTRKBkUp. HIV na chave HKLM\Software\Microsoft\ResKit e substituir o conteúdo existente da chave, digite:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)