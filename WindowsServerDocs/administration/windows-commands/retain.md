---
title: manteve
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 891192c0d6b1687cf6bffea0f57d1853e023b776
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835759"
---
# <a name="retain"></a>manteve



Prepara um volume simples dinâmico existente para ser usado como um volume do sistema ou de inicialização.

## <a name="syntax"></a>Sintaxe

```
retain
```

## <a name="remarks"></a>Comentários

-   Em um disco dinâmico MBR (registro mestre de inicialização), esse comando cria uma entrada de partição no registro mestre de inicialização.
-   Em um disco dinâmico GPT (tabela de partição GUID), esse comando cria uma entrada de partição na tabela de partição GUID.

## <a name="additional-references"></a>Referências adicionais

