---
title: tskill
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 697363c91837ff675a14099fd212f4f0753b739b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392338"
---
# <a name="tskill"></a>tskill

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Finaliza um processo em execução em uma sessão em um servidor Host da Sessão da Área de Trabalho Remota (host de Sessão RD).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|\<ProcessID >|Especifica a ID do processo que você deseja encerrar.|
|\<ProcessName >|Especifica o nome do processo que você deseja encerrar. Esse parâmetro pode incluir caracteres curinga.|
|/Server: \<ServerName >|Especifica o servidor de terminal que contém o processo que você deseja encerrar. Se **/Server** não for especificado, o servidor de host da Sessão RD atual será usado.|
|/ID: \<SessionID >|Encerra o processo que está sendo executado na sessão especificada.|
|SRDF|Encerra o processo que está sendo executado em todas as sessões.|
|/v|Exibe informações sobre as ações que estão sendo executadas.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
- Você pode usar **tskill** para finalizar somente os processos que pertencem a você, a menos que você seja um administrador. Os administradores têm acesso total a todas as funções **tskill** e podem finalizar processos que estão em execução em outras sessões de usuário.
- Quando todos os processos que estão sendo executados em uma sessão terminam, a sessão também termina.
- Se você usar os parâmetros *ProcessName* e **/Server:** <em>ServerName</em> , também deverá especificar o parâmetro **/ID:** <em>SessionID</em> ou **/a** .

## <a name="BKMK_examples"></a>Disso
- Para encerrar o processo 6543, digite:
  ```
  tskill 6543
  ```
- Para finalizar o processo "Explorer" em execução na sessão 5, digite:
  ```
  tskill explorer /id:5
  ```
  #### <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [ &#40;serviços de área de trabalho remota&#41; referência de comando de serviços de terminal](remote-desktop-services-terminal-services-command-reference.md)
