---
title: Reg Restore
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e511694247c04f2befc9c0148498e43b85f664ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836359"
---
# <a name="reg-restore"></a>Reg Restore



Grava subchaves e entradas salvas de volta no registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Reg restore <KeyName> <FileName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName >|Especifica o caminho completo da subchave a ser restaurada. A operação de restauração funciona apenas com o computador local. O KeyName deve incluir uma chave de raiz válida. As chaves raiz válidas são: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<nome de arquivo >|Especifica o nome e o caminho do arquivo com o conteúdo a ser gravado no registro. Esse arquivo deve ser criado com antecedência com a operação **reg Save** usando uma extensão. HIV.|
|/?|Exibe a ajuda para **reg Restore** no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Antes de editar as entradas do registro, salve a subchave pai com a operação **reg Save** . Se a edição falhar, restaure a subchave original com a operação **reg Restore** .
-   A tabela a seguir lista os valores de retorno para a operação **reg Restore** .

|{1&gt;Valor&lt;1}|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para restaurar o arquivo chamado NTRKBkUp. HIV na chave HKLM\Software\Microsoft\ResKit e substituir o conteúdo existente da chave, digite:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)