---
title: Reg unload
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaa7d7a9fa82db2968d988e3b7b3fb8275a72337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834977"
---
# <a name="reg-unload"></a>Reg unload



Remove uma seção do registro que foi carregada usando o **reg carga** operação.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg unload <KeyName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave a ser descarregado. Para especificar computadores remotos, inclua o nome do computador (no formato \\ \\ComputerName\) como parte do *KeyName*. A omissão \\ \\ComputerName\ faz com que a operação padrão no computador local. O *KeyName* deve incluir uma chave de raiz válido. Chaves de raiz válido para o computador local são HKCC, HKCU, HKCR, HKU e HKLM. Se um computador remoto for especificado, as chaves válidas são HKLM e HKU.|
|/?|Exibe a Ajuda para **reg unload** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores retornados para o **reg unload** opção.

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Exemplos

Para descarregar o hive TempHive no arquivo HKLM, digite:
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> Não edite o registro diretamente, a menos que você não tenha nenhuma alternativa. O editor do Registro ignora proteções padrão, permitindo configurações que podem prejudicar o desempenho, danificar o sistema ou até mesmo exigir a reinstalação do Windows. Você pode alterar a maioria das configurações de registro com segurança por meio de programas no painel de controle ou o Microsoft Management Console (MMC). Se você deve editar o registro diretamente, faça backup primeiro.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)