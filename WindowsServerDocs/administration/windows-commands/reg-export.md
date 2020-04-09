---
title: exportar reg
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5901014511fed0c17a641e1ed183ddbf40dd44c8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836489"
---
# <a name="reg-export"></a>exportar reg



Copia as subchaves, entradas e valores especificados do computador local em um arquivo para transferência para outros servidores.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Reg export KeyName FileName [/y]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName >|Especifica o caminho completo da subchave. A operação de exportação funciona apenas com o computador local. O KeyName deve incluir uma chave de raiz válida. As chaves raiz válidas são: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<nome de arquivo >|Especifica o nome e o caminho do arquivo a ser criado durante a operação. O arquivo deve ter uma extensão. reg.|
|/y|Substitui qualquer arquivo existente pelo *nome nome do arquivo sem solicitar* confirmação.|
|/?|Exibe a ajuda para o **reg export** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retorno para a operação de **exportação de reg** .

|{1&gt;Valor&lt;1}|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exportar o conteúdo de todas as subchaves e valores da chave MyApp para o arquivo AppBkUp. reg, digite:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)