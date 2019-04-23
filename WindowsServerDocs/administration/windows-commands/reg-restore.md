---
title: restauração de reg
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0025a37ed8ca50b47e7750501a7362659b500537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858767"
---
# <a name="reg-restore"></a>restauração de reg



Grava salvadas subchaves e entradas de volta para o registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Reg restore <KeyName> <FileName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave a ser restaurado. A operação de restauração funciona apenas com o computador local. O KeyName deve incluir uma chave de raiz válido. As chaves válidas são: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<FileName>|Especifica o nome e caminho do arquivo com o conteúdo a ser gravado no registro. Esse arquivo deve ser criado previamente com o **reg salvar** operação usando uma extensão do HIV.|
|/?|Exibe a Ajuda para **restauração reg** no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Antes de editar as entradas de registro, salve a subchave pai com o **reg salvar** operação. Se houver falha na edição, restaure a subchave com o **restauração reg** operação.
-   A tabela a seguir lista os valores retornados para o **restauração reg** operação.

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Exemplos

Para restaurar o arquivo chamado NTRKBkUp. hiv na chave HKLM\Software\Microsoft\ResKit e substituir o conteúdo existente da chave, digite:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)