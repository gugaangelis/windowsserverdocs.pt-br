---
title: nslookup set timeout
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b7a35a4a8e9e9cc10ea5548875f710fb5a86036
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837807"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o número inicial de segundos a aguardar uma resposta a uma solicitação de pesquisa.
## <a name="syntax"></a>Sintaxe
```
set timeout=<Number>
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|<Number>|Especifica o número de segundos a aguardar uma resposta. O número padrão de segundos de espera é 5.|
|{Ajuda &#124; ?}|Exibe um resumo breve dos **nslookup** subcomandos.|
## <a name="remarks"></a>Comentários
-   Quando uma resposta a uma solicitação não for recebida dentro do período de tempo especificado, o tempo limite é duplicado e a solicitação é enviada novamente. Você pode usar o **repetição conjunto** comando para controlar o número de repetições.
## <a name="BKMK_examples"></a>Exemplos
O exemplo a seguir define o tempo limite para obter uma resposta para 2 segundos:
```
set timeout=2
```
## <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[nslookup definir repetição](nslookup-set-retry.md)
