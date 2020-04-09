---
title: bitsadmin setnoprogresstimeout
description: O tópico de comandos do Windows para Bitsadmin setnoprogresstimeout, que define o período de tempo, em segundos, que o serviço tenta transferir o arquivo após ocorrer um erro transitório.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 544a6c73f29684bc4091ec05fa28016fbc718bb2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849349"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Define o período de tempo, em segundos, que o BITS tenta transferir o arquivo depois que o primeiro erro transitório ocorre.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Tempo limite|Um número representado em segundos.|

## <a name="remarks"></a>Comentários

-   O intervalo sem tempo limite de progresso começa quando o trabalho encontra um erro transitório.
-   O intervalo de tempo limite é interrompido ou redefinido quando um byte de dados é transferido com êxito.
-   Se nenhum intervalo de tempo limite de progresso exceder o *TimeOutValue*, o trabalho será colocado em um estado de erro fatal.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define o valor de tempo limite sem progresso para o trabalho chamado *myDownloadJob* para 20 segundos
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)