---
title: bitsadmin setnoprogresstimeout
description: O tópico de comandos do Windows para **Bitsadmin setnoprogresstimeout** -define o período de tempo, em segundos, que o serviço tenta transferir o arquivo depois que um erro transitório ocorre.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 761d0d76a2c70af9d4ad68aa564c1a9816691d0d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380502"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Define o período de tempo, em segundos, que o BITS tenta transferir o arquivo depois que o primeiro erro transitório ocorre.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Tempo limite|Um número representado em segundos.|

## <a name="remarks"></a>Comentários

-   O intervalo sem tempo limite de progresso começa quando o trabalho encontra um erro transitório.
-   O intervalo de tempo limite é interrompido ou redefinido quando um byte de dados é transferido com êxito.
-   Se nenhum intervalo de tempo limite de progresso exceder o *TimeOutValue*, o trabalho será colocado em um estado de erro fatal.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir define o valor de tempo limite sem progresso para o trabalho chamado *myDownloadJob* para 20 segundos
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)