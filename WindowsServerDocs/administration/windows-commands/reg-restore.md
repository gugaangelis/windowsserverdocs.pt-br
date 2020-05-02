---
title: Reg Restore
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d490f99f032b38c8bbbe9352b8571b4a85202e1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722518"
---
# <a name="reg-restore"></a>Reg Restore



Grava subchaves e entradas salvas de volta no registro.



## <a name="syntax"></a>Sintaxe

```
Reg restore <KeyName> <FileName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave a ser restaurada. A operação de restauração funciona apenas com o computador local. O KeyName deve incluir uma chave de raiz válida. As chaves raiz válidas são: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<Nome de arquivo>|Especifica o nome e o caminho do arquivo com o conteúdo a ser gravado no registro. Esse arquivo deve ser criado com antecedência com a operação **reg Save** usando uma extensão. HIV.|
|/?|Exibe a ajuda para **reg Restore** no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Antes de editar as entradas do registro, salve a subchave pai com a operação **reg Save** . Se a edição falhar, restaure a subchave original com a operação **reg Restore** .
-   A tabela a seguir lista os valores de retorno para a operação **reg Restore** .

|Valor|Descrição|
|-----|-----------|
|0|Sucesso|
|1|Falha|

## <a name="examples"></a>Exemplos

Para restaurar o arquivo chamado NTRKBkUp. HIV na chave HKLM\Software\Microsoft\ResKit e substituir o conteúdo existente da chave, digite:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)