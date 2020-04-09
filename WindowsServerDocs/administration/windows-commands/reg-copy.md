---
title: cópia reg
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2acfdd3c0ad66d93313a11f8025b690ea0157c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836529"
---
# <a name="reg-copy"></a>cópia reg



Copia uma entrada de registro para um local especificado no computador local ou remoto.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName1 >|Especifica o caminho completo da subchave a ser copiada. Para especificar um computador remoto, inclua o nome do computador (no formato \\\\ComputerName\) como parte do *KeyName*. Omitir \\\\computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU.|
|\<KeyName2 >|Especifica o caminho completo do destino da subchave. Para especificar um computador remoto, inclua o nome do computador (no formato \\\\ComputerName\) como parte do *KeyName*. Omitir \\\\computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU.|
|/s|Copia todas as subchaves e entradas na subchave especificada.|
|/f|Copia a subchave sem solicitar confirmação.|
|/?|Exibe a ajuda para a cópia **reg** no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O reg não solicita confirmação ao copiar uma subchave.
-   A tabela a seguir lista os valores de retorno para a operação **reg Copy** .

|{1&gt;Valor&lt;1}|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para copiar todas as subchaves e valores na chave MyApp para a chave SaveMyApp, digite:
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
Para copiar todos os valores sob a chave MyCo no computador chamado ZODIAC para a chave MyCo1 no computador atual, digite:
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)