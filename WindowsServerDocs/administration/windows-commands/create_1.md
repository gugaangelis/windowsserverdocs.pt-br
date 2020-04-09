---
title: criar
description: O tópico de comandos do Windows para criar, que inicia o processo de criação de cópia de sombra, usando o contexto atual e as configurações de opção.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d29285517ca678a15828079c95663fc4d501eaf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846829"
---
# <a name="create"></a>criar

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