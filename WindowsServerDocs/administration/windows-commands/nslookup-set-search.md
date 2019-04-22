---
title: nslookup set search
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf952a0337e23c0426265c6c0a4a8387a6ab45e1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816987"
---
# <a name="nslookup-set-search"></a>nslookup set search



Acrescenta os nomes de domínio do sistema de nome de domínio (DNS) na lista de pesquisa de domínio DNS para a solicitação até que uma resposta seja recebida. Isso se aplica quando o conjunto e a pesquisa contêm pelo menos um ponto de solicitação, mas não terminam com um ponto à direita.

## <a name="syntax"></a>Sintaxe

```
set [no]search
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|**nosearch**|Interrompe a acrescentar os nomes de domínio do sistema de nome de domínio (DNS) na lista de pesquisa de domínio DNS para a solicitação.|
|**search**|Acrescenta os nomes de domínio do sistema de nome de domínio (DNS) na lista de pesquisa de domínio DNS para a solicitação até que uma resposta seja recebida. A sintaxe padrão é **pesquisa**.|
|{Ajuda | ?}|Exibe um resumo breve dos **nslookup** subcomandos.|

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)