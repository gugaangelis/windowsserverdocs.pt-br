---
title: rpcinfo
description: Saiba como listar os programas em um computador remoto.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 3931dceea48c0e995a15f4966529fed4d5e85e34
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384466"
---
# <a name="rpcinfo"></a>rpcinfo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lista os programas em computadores remotos. O utilitário de linha de comando **rpcinfo** faz uma chamada de procedimento remoto (RPC) para um servidor RPC e relata o que ele encontra. 

## <a name="syntax"></a>Sintaxe
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/p [\<Node >]|lista todos os programas registrados com o mapeador de porta no host especificado. Se você não especificar um nome de nó (computador), o programa consultará o mapeador de porta no host local.|
|/b versão de \<Program >|Solicita uma resposta de todos os nós de rede que têm o programa e a versão especificados registrados com o mapeador de porta. Você deve especificar um nome de programa ou número e um número de versão.|
|/t \<Node Program > [\<version >]|Usa o protocolo de transporte TCP para chamar o programa especificado. Você deve especificar um nome de nó (computador) e um nome de programa. Se você não especificar uma versão, o programa chamará todas as versões.|
|/u \<Node Program > [\<version >]|Usa o protocolo de transporte UDP para chamar o programa especificado. Você deve especificar um nome de nó (computador) e um nome de programa. Se você não especificar uma versão, o programa chamará todas as versões.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_Examples"></a>Disso
Para listar todos os programas registrados com o mapeador de porta, digite:
```
rpcinfo /p [<Node>]
```
Para solicitar uma resposta de nós de rede que têm um programa especificado, digite:
```
rpcinfo /b <Program version>
```
Para usar o protocolo TCP para chamar um programa, digite:
```
rpcinfo /t <Node Program> [<version>]
```
Use UDP (User Datagram Protocol) para chamar um programa:
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
