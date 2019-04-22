---
title: rpcinfo
description: Saiba como listar os programas em um computador remoto.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4aba1e57d5a61103310fbe7abcac391e543be5aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826367"
---
# <a name="rpcinfo"></a>rpcinfo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lista os programas em computadores remotos. O **rpcinfo** utilitário de linha de comando faz com que um procedimento remoto (RPC) é chamada para um servidor RPC e relata o que encontrar. 

## <a name="syntax"></a>Sintaxe
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/p [\<Node>]|lista todos os programas registrados com o mapeador de porta no host especificado. Se você não especificar um nome de nó (computador), o programa consulta o mapeador de porta no host local.|
|/ b \<versão do programa >|Solicita uma resposta de todos os nós de rede que têm o programa especificado e a versão registrada com o mapeador de porta. Você deve especificar um nome de programa ou número e um número de versão.|
|/t \<programa de nó > [\<versão >]|Usa o protocolo de transporte TCP para chamar o programa especificado. Você deve especificar um nome de nó (computador) e um nome de programa. Se você não especificar uma versão, o programa chama todas as versões.|
|/u \<programa de nó > [\<versão >]|Usa o protocolo de transporte UDP para chamar o programa especificado. Você deve especificar um nome de nó (computador) e um nome de programa. Se você não especificar uma versão, o programa chama todas as versões.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos
Para listar todos os programas registrados com o mapeador de porta, digite:
```
rpcinfo /p [<Node>]
```
Para solicitar uma resposta de nós de rede que têm um programa especificado, digite:
```
rpcinfo /b <Program version>
```
Para usar o protocolo TCP (Transmission Control) para chamar um programa, digite:
```
rpcinfo /t <Node Program> [<version>]
```
Use o protocolo UDP (User Datagram) para chamar um programa:
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
