---
title: bitsadmin getnotifycmdline
description: Tópico de comandos do Windows para **getnotifycmdline bitsadmin** -recupera a linha de comando que é executado quando o trabalho for concluído, transferência de dados.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ca7b2e67c0b5672733a25465fba89d1bd69d07a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817287"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

Recupera a linha de comando para ser executado quando o trabalho for concluído, transferência de dados.

**BITS 1.2 e anteriores**: Sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetNotifyCmdLine <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera a linha de comando usada pelo serviço quando o trabalho chamado *myDownloadJob* é concluída.
```
C:\>bitsadmin /GetNotifyCmdLine myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)