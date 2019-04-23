---
title: Reg delete
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 369ef3bda37ab8e143a14f0f9707b9bbf14bd5f8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877077"
---
# <a name="reg-delete"></a>Reg delete



Exclui uma subchave ou entradas do registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave ou entrada a ser excluída. Para especificar um computador remoto, inclua o nome do computador (no formato \\ \\ComputerName\) como parte do *KeyName*. A omissão \\ \\ComputerName\ faz com que a operação padrão no computador local. O *KeyName* deve incluir uma chave de raiz válido. As chaves de raiz válido para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves válidas são: HKLM e HKU.|
|/v \<ValueName>|Exclui uma entrada específica na subchave. Se nenhuma entrada for especificada, todas as entradas e subchaves na subchave serão excluídas.|
|/ve|Especifica que somente as entradas que não têm nenhum valor serão excluídas.|
|/va|Exclui todas as entradas na subchave especificada. As subchaves a subchave especificada não são excluídas.|
|/f|Exclui a subchave do registro existente ou a entrada sem pedir confirmação.|
|/?|Exibe a Ajuda para **reg excluir** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores retornados para o **reg excluir** operação.

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Exemplos

Para excluir a chave do registro tempo limite e suas todas as subchaves e valores, digite:
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Para excluir o valor do registro no computador chamado ZODÍACO MTU em HKLM\Software\MyCo, digite:
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)