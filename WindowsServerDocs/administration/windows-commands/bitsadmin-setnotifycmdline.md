---
title: bitsadmin setnotifycmdline
description: Tópico de comandos do Windows para * * *-bitsadmin setnotifycmdlineSets a linha de comando que será executado quando o trabalho for concluído, transferência de dados ou quando um trabalho entra em um estado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1cea4e99cbaaf3881c6f436bdb932090ad6b006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859067"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Define a linha de comando que será executado quando o trabalho for concluído, transferência de dados ou quando um trabalho entra em um estado.

**BITS 1.2 e anteriores**: Sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|ProgramName|Nome do comando ser executado quando o trabalho for concluído.|
|ProgramParameters|Parâmetros que você deseja passar para *ProgramName*.|

## <a name="remarks"></a>Comentários

Você pode especificar NULL para *ProgramName* e *ProgramParameters*. Se *ProgramName* for NULL, *ProgramParameters* deve ser NULL.

> [!IMPORTANT]
> Se *ProgramParameters* não for nulo, então o primeiro parâmetro na *ProgramParameters* deve corresponder ao *ProgramName*.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define a linha de comando usada pelo serviço para executar o bloco de notas, quando o trabalho chamado *myDownloadJob* é concluída.
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe "notepad c:\eula.txt"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)