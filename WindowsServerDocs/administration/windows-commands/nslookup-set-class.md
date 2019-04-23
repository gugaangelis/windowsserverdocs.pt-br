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
ms.openlocfilehash: 5e02a6f473a7c2dc6d6b66eb5b23fded930bc817
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856347"
---
# <a name="nslookup-set-class"></a>nslookup set class



Altera a classe da consulta. A classe especifica o grupo de protocolo das informações.

## <a name="syntax"></a>Sintaxe

```
set class=<Class>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Class>|A classe padrão é in. O exemplo a seguir lista os valores válidos para esse comando.</br>-IN: Especifica a classe de Internet.</br>-CAOS: Especifica a classe de Chaos.</br>-HESIOD: Especifica a classe MIT Athena Hesiod.</br>-QUALQUER: Especifica qualquer um dos caracteres curinga listados anteriormente.|
|{Ajuda | ?}|Exibe um resumo breve dos **nslookup** subcomandos.|

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)