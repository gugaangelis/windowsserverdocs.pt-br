---
title: showmount
description: Artigo de referência para o getmount, que exibe diretórios montados.
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0be623eadd56a55a87f2df57fec9b4c6558770c9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882399"
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