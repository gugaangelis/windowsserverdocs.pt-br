---
title: pushprinterconnections
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c571d3adffd0e6a28f63f6d821b2524dc055aa9a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873717"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



Lê as configurações de Conexão de impressora implantada da diretiva de grupo e implanta/remove as conexões de impressora conforme necessário.

## <a name="syntax"></a>Sintaxe

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|<-log>|Grava um por arquivo de log de depuração do usuário para % temp ou gravações um por log de depuração do computador para % windir%\temp.|
|<-?>|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Esse utilitário é para uso em scripts de logon de usuário ou inicialização de máquina e não deve ser executado da linha de comando.

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Implantar impressoras usando a diretiva de grupo](https://go.microsoft.com/fwlink/?LinkId=230627)