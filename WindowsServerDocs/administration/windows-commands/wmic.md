---
title: wmic
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c68866fbe0c8f5b16dae77e2121331f06cdc726
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885837"
---
# <a name="wmic"></a>wmic



Exibe informações de WMI dentro de um shell de comando interativo.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
command </parameter>
```

## <a name="sub-commands"></a>Subcomandos

Os subcomandos a seguir estão disponíveis em todos os momentos:

|Subcomando|Descrição|
|-----------|-----------|
|class|Sai do modo de alias padrão do WMIC para acessar classes diretamente no esquema do WMI.|
|path|Sai do modo de alias padrão do WMIC para acessar instâncias diretamente no esquema do WMI.|
|context|Exibe os valores atuais de todas as opções globais.|
|[sair \| sair]|Fecha o WMIC shell de comando.|

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|</parameter>|\<Uma descrição concisa, começa com um verbo. >|
|</param2>|\<Outra descrição concisa, começa com um verbo. >|


## <a name="BKMK_examples"></a>Exemplos

Para exibir os valores atuais de todas as opções globais, digite:
```
wmic context
```
Saída semelhante à seguinte exibida:
```
NAMESPACE    : root\cimv2
ROLE         : root\cli
NODE(S)      : BOBENTERPRISE
IMPLEVEL     : IMPERSONATE
[AUTHORITY   : N/A]
AUTHLEVEL    : PKTPRIVACY
LOCALE       : ms_409
PRIVILEGES   : ENABLE
TRACE        : OFF
RECORD       : N/A
INTERACTIVE  : OFF
FAILFAST     : OFF
OUTPUT       : STDOUT
APPEND       : STDOUT
USER         : N/A
AGGREGATE    : ON
```
Para alterar o idioma a ID usada pela linha de comando para inglês (localidade ID 409), digite:
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)