---
title: create
description: Tópico de referência para criar, que inicia o processo de criação de cópia de sombra, usando o contexto atual e as configurações de opção.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfddbebd5744d8cd222d67e46690ce8b5d2e0fde
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716833"
---
# <a name="create"></a>create

Inicia o processo de criação de cópia de sombra usando as configurações de contexto e opção atuais. Requer pelo menos um volume no conjunto de cópias de sombra.

## <a name="syntax"></a>Sintaxe

```
create
```

## <a name="remarks"></a>Comentários

-   Você deve adicionar pelo menos um volume com o comando **Add volume** antes de poder usar o comando **Create** .
-   Você pode usar o comando **Iniciar backup** para especificar um backup completo, em vez de um backup de cópia.
-   Depois de executar o comando **Create** , você pode usar o comando **exec** para executar um script de duplicação para backup da cópia de sombra.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)