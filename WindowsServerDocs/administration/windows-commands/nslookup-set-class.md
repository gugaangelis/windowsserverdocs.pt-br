---
title: nslookup set class
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7953f450c17afdee849515f8d8945631a30f4b98
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436844"
---
# <a name="nslookup-set-class"></a>nslookup set class



Altera a classe da consulta. A classe especifica o grupo de protocolo das informações.

## <a name="syntax"></a>Sintaxe

```
set class=<Class>
```

## <a name="parameters"></a>Parâmetros

| Parâmetro |                                                                                                                                    Descrição                                                                                                                                    |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<Class>  | A classe padrão é in. O exemplo a seguir lista os valores válidos para esse comando.</br>-IN: Especifica a classe de Internet.</br>-CAOS: Especifica a classe de Chaos.</br>-HESIOD: Especifica a classe MIT Athena Hesiod.</br>-QUALQUER: Especifica qualquer um dos caracteres curinga listados anteriormente. |
|   {Ajuda   |                                                                                                                                        ?}                                                                                                                                         |

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)