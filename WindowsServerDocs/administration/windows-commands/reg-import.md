---
title: importação de reg
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e7e033091752f97086fd27fcb94e62469f0cced
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722547"
---
# <a name="reg-import"></a>importação de reg



Copia o conteúdo de um arquivo que contém subchaves de registro exportadas, entradas e valores no registro do computador local.



## <a name="syntax"></a>Sintaxe

```
Reg import FileName
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Nome de arquivo>|Especifica o nome e o caminho do arquivo que tem o conteúdo a ser copiado no registro do computador local. Esse arquivo deve ser criado com antecedência usando **reg export**.|
|/?|Exibe a ajuda para o **reg Import** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retorno para a operação de **importação de reg** .

|Valor|Descrição|
|-----|-----------|
|0|Sucesso|
|1|Falha|

## <a name="examples"></a>Exemplos

Para importar entradas do registro do arquivo chamado AppBkUp. reg, digite:
```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)