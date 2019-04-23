---
title: bitsadmin setnoprogresstimeout
description: Tópico de comandos do Windows para **setnoprogresstimeout bitsadmin** -define o período de tempo, em segundos, que o serviço tenta transferir o arquivo depois que ocorre um erro transitório.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45dd8a7ddfae877984a98db66c742e0af4d18f0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873767"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Define o período de tempo, em segundos, que BITS tenta transferir o arquivo após o primeiro erro transitório ocorre.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|TimeOutvalue|Um número representado em segundos.|

## <a name="remarks"></a>Comentários

-   Nenhum intervalo de tempo limite de progresso começa quando o trabalho encontra um erro transitório.
-   O intervalo de tempo limite para ou redefine quando um byte de dados é transferido com êxito.
-   Se nenhum intervalo de tempo limite de progresso exceder os *ValorDoTempoLimite*, em seguida, o trabalho será colocado em um estado de erro fatal.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir não define a nenhum valor de tempo limite de andamento do trabalho nomeado *myDownloadJob* para 20 segundos
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)