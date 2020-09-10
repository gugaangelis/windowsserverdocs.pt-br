---
title: showmount
description: Artigo de referência para o getmount, que exibe diretórios montados.
ms.topic: reference
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e0a03c7e08c80e4cf0b99bbcb74aeff1deccdb1e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640957"
---
# <a name="showmount"></a>showmount

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar a **montagem** para exibir diretórios montados.

## <a name="syntax"></a>Sintaxe
```
showmount {-e|-a|-d} <Server>
```

## <a name="description"></a>Descrição
O utilitário de linha de comando de **addmount** \- exibe informações sobre sistemas de arquivos montados exportados pelo servidor para NFS no computador especificado pelo *servidor*. Se o *servidor* não for fornecido, a **montagem** exibirá informações sobre o computador no qual o comando de **conmontagem** é executado.

Você deve fornecer uma das seguintes opções:

- ** \- e** -exibe todos os sistemas de arquivos exportados no servidor.
- ** \- a** -exibe todos os clientes NFS do sistema de arquivos de rede \( \) e os diretórios no servidor que cada um montou.
- ** \- d** -exibe todos os diretórios no servidor que estão atualmente montados por clientes NFS.

## <a name="see-also"></a>Consulte Também
[Referência aos comandos dos Serviços para Network File System](services-for-network-file-system-command-reference.md)