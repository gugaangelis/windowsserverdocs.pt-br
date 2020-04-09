---
title: bitsadmin setpriority
description: Tópico de comandos do Windows para Bitsadmin setanteriority, que define a prioridade do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d007c62402a3d70910e1c79fab5c406295a63a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849209"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

Define a prioridade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetPriority <Job> <Priority>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Prioridade|Um dos seguintes valores:</br>-PRIMEIRO plano</br>-ALTA</br>-NORMAL</br>-BAIXO|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define a prioridade para o trabalho chamado *myDownloadJob* como normal.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)