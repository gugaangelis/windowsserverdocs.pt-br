---
title: wmic
description: Tópico de comandos do Windows para WMIC, que exibe informações de WMI dentro de um shell de comando interativo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03ba4ecb4b12b03e010318bf6ca260dec00f28f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829049"
---
# <a name="wmic"></a>wmic



Exibe informações de WMI dentro de um shell de comando interativo.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
wmic </parameter>
```

## <a name="sub-commands"></a>Subcomandos

Os seguintes subcomandos estão disponíveis o tempo todo:

|Subcomando|Descrição|
|-----------|-----------|
|{1&gt;classe&lt;1}|Sai do modo de alias padrão do WMIC para acessar classes diretamente no esquema WMI.|
|path|Sai do modo de alias padrão do WMIC para acessar instâncias diretamente no esquema do WMI.|
|contexto|Exibe os valores atuais de todas as opções globais.|
|[sair \| sair]|Sai do Shell de comando do WMIC.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
