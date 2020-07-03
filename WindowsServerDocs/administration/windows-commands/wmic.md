---
title: wmic
description: Artigo de referência para WMIC, que exibe informações de WMI dentro de um shell de comando interativo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c14f877c226bdd376da39cfa6e8f11116d59fe56
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936112"
---
# <a name="wmic"></a>wmic



Exibe informações de WMI dentro de um shell de comando interativo.



## <a name="syntax"></a>Syntax

```
wmic </parameter>
```

## <a name="sub-commands"></a>Subcomandos

Os seguintes subcomandos estão disponíveis o tempo todo:

|Subcomando|Descrição|
|-----------|-----------|
|class|Sai do modo de alias padrão do WMIC para acessar classes diretamente no esquema WMI.|
|path|Sai do modo de alias padrão do WMIC para acessar instâncias diretamente no esquema do WMI.|
|contexto|Exibe os valores atuais de todas as opções globais.|
|[sair \| sair]|Sai do Shell de comando do WMIC.|

## <a name="examples"></a>Exemplos

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
