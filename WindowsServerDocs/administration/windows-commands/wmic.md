---
title: wmic
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e9840bc20ddf6193241fe36055698e2bd3222496
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361881"
---
# <a name="wmic"></a>wmic



Exibe informações de WMI dentro de um shell de comando interativo.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
command </parameter>
```

## <a name="sub-commands"></a>Subcomandos

Os seguintes subcomandos estão disponíveis o tempo todo:

|Subcomando|Descrição|
|-----------|-----------|
|class|Sai do modo de alias padrão do WMIC para acessar classes diretamente no esquema WMI.|
|path|Sai do modo de alias padrão do WMIC para acessar instâncias diretamente no esquema do WMI.|
|noticioso|Exibe os valores atuais de todas as opções globais.|
|[sair \| sair]|Sai do Shell de comando do WMIC.|

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|</parameter>|\<Descrição concisa, começa com um verbo. >|
|</param2>|\<outra descrição concisa, começa com um verbo. >|


## <a name="BKMK_examples"></a>Disso

Para exibir os valores atuais de todas as opções globais, digite:
```
wmic context
```
Saída semelhante às seguintes exibições:
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
Para alterar a ID de idioma usada pela linha de comando para inglês (ID de localidade 409), digite:
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)