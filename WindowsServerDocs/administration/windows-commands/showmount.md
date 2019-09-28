---
title: showmount
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e1d197072db93130de880b5ec52d1875720b1d26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383907"
---
# <a name="showmount"></a>showmount

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar a **montagem** para exibir diretórios montados.  
  
## <a name="syntax"></a>Sintaxe  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descrição  
O utilitário **getmount** comando @ no__t-1line exibe informações sobre os sistemas de arquivos montados exportados pelo servidor para NFS no computador especificado pelo *servidor*. Se o *servidor* não for fornecido, a **montagem** exibirá informações sobre o computador no qual o comando de **conmontagem** é executado.  
  
Você deve fornecer uma das seguintes opções:  
  
- **\-e** -exibe todos os sistemas de arquivos exportados no servidor.  
- **\-a** -exibe todos os clientes do sistema de arquivos de rede \(NFS @ no__t-3 e os diretórios no servidor que cada um tiver montado.  
- **\-D** -exibe todos os diretórios no servidor que estão montados atualmente por clientes NFS.  
  
## <a name="see-also"></a>Consulte também  
[Referência aos comandos dos Serviços para Network File System](services-for-network-file-system-command-reference.md)  