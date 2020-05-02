---
title: exportar reg
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c697595d5d19c953ef85f7a2e334c6fe05329d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722559"
---
# <a name="reg-export"></a>exportar reg



Copia as subchaves, entradas e valores especificados do computador local em um arquivo para transferência para outros servidores.



## <a name="syntax"></a>Sintaxe

```
Reg export KeyName FileName [/y]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave. A operação de exportação funciona apenas com o computador local. O KeyName deve incluir uma chave de raiz válida. As chaves raiz válidas são: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<Nome de arquivo>|Especifica o nome e o caminho do arquivo a ser criado durante a operação. O arquivo deve ter uma extensão. reg.|
|/y|Substitui qualquer arquivo existente pelo *nome nome do arquivo sem solicitar* confirmação.|
|/?|Exibe a ajuda para o **reg export** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retorno para a operação de **exportação de reg** .

|Valor|Descrição|
|-----|-----------|
|0|Sucesso|
|1|Falha|

## <a name="examples"></a>Exemplos

Para exportar o conteúdo de todas as subchaves e valores da chave MyApp para o arquivo AppBkUp. reg, digite:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)