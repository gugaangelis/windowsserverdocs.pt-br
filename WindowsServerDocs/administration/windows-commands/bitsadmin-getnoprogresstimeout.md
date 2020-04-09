---
title: bitsadmin getnoprogresstimeout
description: O tópico de comandos do Windows para **Bitsadmin getnoprogresstimeout**, que recupera o período de tempo, em segundos, que o serviço tentará transferir o arquivo depois que ocorrer um erro transitório.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf2cfd77b494e221b60c8816ff46eed5f9252f39
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850599"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout

Recupera o período de tempo, em segundos, que o serviço tentará transferir o arquivo depois que ocorrer um erro transitório.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getnoprogresstimeout <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera o valor de tempo limite de progresso para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getnoprogresstimeout myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)