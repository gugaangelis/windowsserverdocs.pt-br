---
title: bitsadmin setnotifycmdline
description: O tópico de comandos do Windows para Bitsadmin setnotifycmdline, que define o comando de linha de comando que será executado quando o trabalho terminar de transferir dados ou quando um trabalho entrar em um estado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 761a7003e44e8dc15cb2dd2f1ce5a1a23be53286
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849329"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Define o comando de linha de comando que será executado quando o trabalho terminar de transferir dados ou quando um trabalho entrar em um estado.

**BITS 1,2 e anteriores**: sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|programName|Nome do comando a ser executado quando o trabalho for concluído.|
|Programparameters|Parâmetros que você deseja passar para *ProgramName*.|

## <a name="remarks"></a>Comentários

Você pode especificar NULL para *ProgramName* e *programparameters*. Se *ProgramName* for nulo, *programparameters* deverá ser nulo.

> [!IMPORTANT]
> Se *programparameters* não for nulo, o primeiro parâmetro em *programparameters* deverá corresponder a *ProgramName*.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define o comando de linha de comando usado pelo serviço para executar o bloco de notas quando o trabalho chamado *myDownloadJob* é concluído.
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)