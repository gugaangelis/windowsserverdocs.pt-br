---
title: nslookup set search
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9972919eae1be21d5dd30820d64dd1576b935666
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838299"
---
# <a name="nslookup-set-search"></a>nslookup set search



Acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida. Isso se aplica quando o conjunto e a solicitação de pesquisa contêm pelo menos um ponto, mas não terminam com um ponto à direita.

## <a name="syntax"></a>Sintaxe

```
set [no]search
```

### <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                                          Descrição                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nosearch** |                            Interrompe a anexação dos nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação.                            |
|  **procurando**  | Acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida. A sintaxe padrão é **Search**. |
|    {ajuda     |                                                                              ?}                                                                               |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)