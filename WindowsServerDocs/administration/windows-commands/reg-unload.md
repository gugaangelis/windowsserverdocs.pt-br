---
title: Reg Unload
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5fd1436ed1122a09eea11d358a3711aedddf2c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836269"
---
# <a name="reg-unload"></a>Reg Unload



Remove uma seção do registro que foi carregado usando a operação **reg Load** .

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg unload <KeyName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName >|Especifica o caminho completo da subchave a ser descarregada. Para especificar computadores remotos, inclua o nome do computador (no formato \\\\ComputerName\) como parte do *KeyName*. Omitir \\\\computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. Chaves de raiz válidas para o computador local são HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão HKLM e HKU.|
|/?|Exibe a ajuda para o **reg Unload** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retorno para a opção **reg Unload** .

|{1&gt;Valor&lt;1}|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para descarregar o hive TempHive no arquivo HKLM, digite:
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> Não edite o registro diretamente, a menos que você não tenha nenhuma alternativa. O editor do registro ignora as proteções padrão, permitindo que as configurações possam prejudicar o desempenho, danificar o sistema ou até mesmo exigir que você reinstale o Windows. Você pode alterar com segurança a maioria das configurações do registro usando os programas no painel de controle ou no console de gerenciamento Microsoft (MMC). Se você precisar editar o registro diretamente, faça o backup primeiro.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)