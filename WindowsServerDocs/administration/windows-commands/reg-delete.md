---
title: REG DELETE
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 726a3c700a9278dbc7abb1873aae7ea3c957bbb5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836499"
---
# <a name="reg-delete"></a>REG DELETE



Exclui uma subchave ou entradas do registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName >|Especifica o caminho completo da subchave ou da entrada a ser excluída. Para especificar um computador remoto, inclua o nome do computador (no formato \\\\ComputerName\) como parte do *KeyName*. Omitir \\\\computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU.|
|/v \<valueName >|Exclui uma entrada específica sob a subchave. Se nenhuma entrada for especificada, todas as entradas e subchaves na subchave serão excluídas.|
|/ve|Especifica que somente as entradas que não têm nenhum valor serão excluídas.|
|/va|Exclui todas as entradas na subchave especificada. As subchaves na subchave especificada não são excluídas.|
|/f|Exclui a subchave ou entrada do registro existente sem solicitar confirmação.|
|/?|Exibe a ajuda para o **reg Delete** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retorno para a operação de **exclusão de reg** .

|{1&gt;Valor&lt;1}|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para excluir o tempo limite da chave do registro e suas subchaves e valores, digite:
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Para excluir o MTU do valor do registro em HKLM\Software\MyCo no computador chamado ZODIAC, digite:
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)